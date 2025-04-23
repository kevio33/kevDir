# 1.Zygote进程

Zygote进程是所有的android进程的父进程，包括SystemServer和各种应用进程都是通过Zygote进程fork出来的。Zygote（孵化）进程相当于是android系统的根进程，后面所有的进程都是通过这个进程fork出来的

**zygote进程又是init进程孵化出来的**

> **进程启动先后顺序：**
>
>  init进程 --> Zygote进程 --> SystemServer进程 -->各种应用进程
>
> - init进程：**linux的根进程**，android系统是基于linux系统的，因此可以算作是整个android操作系统的第一个进程；
> - Zygote进程：**android系统的根进程**，主要作用：可以作用Zygote进程fork出SystemServer进程和各种应用进程；
> - SystemService进程：主要是在这个进程中启动系统的各项服务，比如ActivityManagerService，PackageManagerService，WindowManagerService服务等等；
> - 各种应用进程：启动自己编写的客户端应用时，一般都是重新启动一个应用进程，有自己的虚拟机与运行环境；

## 1.1 zygote进程启动流程

> 源码位置—— `frameworks/base/core/java/com/android/internal/os/ZygoteInit.java `

> **基于Android API 29**

### ZygoteInit类的main方法

init进程在启动Zygote进程时一般都会调用ZygoteInit类的main方法，该方法的具体实现包括：

- 调用enableDdms()，设置DDMS可用。
- 然后调用preload方法实现预加载各种资源
- 然后通过调用forkSystemServer使用fork系统调用，fork除SystemServer
- 注册并启动socket

```java
public static void main(String argv[]) {
    ZygoteServer zygoteServer = null;

    // 防止在Zygote初始化期间创建新线程（保证进程纯净性）
    ZygoteHooks.startZygoteNoThreadCreation();

    // 设置Zygote进程组ID（进程控制相关）
    try {
        Os.setpgid(0, 0); // 将当前进程设置为新的进程组leader
    } catch (ErrnoException ex) {
        throw new RuntimeException("Failed to setpgid(0,0)", ex);
    }

    Runnable caller;
    try {
        // 记录启动指标（仅首次启动时）
        if (!"1".equals(SystemProperties.get("sys.boot_completed"))) {
            MetricsLogger.histogram(null, "boot_zygote_init",
                                    (int) SystemClock.elapsedRealtime());
        }

        // 根据CPU架构选择性能追踪标签
        String bootTimeTag = Process.is64Bit() ? "Zygote64Timing" : "Zygote32Timing";
        TimingsTraceLog bootTimingsTraceLog = new TimingsTraceLog(bootTimeTag,
                                                                  Trace.TRACE_TAG_DALVIK);
        bootTimingsTraceLog.traceBegin("ZygoteInit"); // 开始记录Zygote初始化耗时

        RuntimeInit.enableDdms(); // 启用DDMS调试支持

        // 解析命令行参数
        boolean startSystemServer = false;    // 是否启动system_server（系统服务主进程）
        String zygoteSocketName = "zygote";   // 默认socket名称
        String abiList = null;                // CPU架构列表（如arm64-v8a,armeabi-v7a）
        boolean enableLazyPreload = false;    // 是否延迟预加载资源
        for (int i = 1; i < argv.length; i++) { // 遍历启动参数
            if ("start-system-server".equals(argv[i])) {
                startSystemServer = true;     // 标记需要启动系统服务
            } else if ("--enable-lazy-preload".equals(argv[i])) {
                enableLazyPreload = true;     // 启用延迟预加载模式
            } else if (argv[i].startsWith(ABI_LIST_ARG)) { // 解析ABI列表
                abiList = argv[i].substring(ABI_LIST_ARG.length());
            } else if (argv[i].startsWith(SOCKET_NAME_ARG)) { // 获取socket名称
                zygoteSocketName = argv[i].substring(SOCKET_NAME_ARG.length());
            } else {
                throw new RuntimeException("Unknown command line argument: " + argv[i]);
            }
        }

        // 判断是否是主Zygote进程（可能存在多个Zygote进程）
        final boolean isPrimaryZygote = zygoteSocketName.equals(Zygote.PRIMARY_SOCKET_NAME);

        // 预加载公共资源（类、资源、库等）
        if (!enableLazyPreload) { // 非延迟模式立即预加载
            bootTimingsTraceLog.traceBegin("ZygotePreload");
            EventLog.writeEvent(LOG_BOOT_PROGRESS_PRELOAD_START,
                                SystemClock.uptimeMillis());
            preload(bootTimingsTraceLog); // （1）核心预加载方法（耗时操作）
            EventLog.writeEvent(LOG_BOOT_PROGRESS_PRELOAD_END,
                                SystemClock.uptimeMillis());
            bootTimingsTraceLog.traceEnd(); 
        } else { // 延迟模式则调整优先级
            Zygote.resetNicePriority();
        }

        // 执行GC清理初始化阶段产生的临时对象
        bootTimingsTraceLog.traceBegin("PostZygoteInitGC");
        gcAndFinalize(); // 触发Full GC和Finalization
        bootTimingsTraceLog.traceEnd(); 

        bootTimingsTraceLog.traceEnd(); // 结束ZygoteInit追踪
        Trace.setTracingEnabled(false, 0); // 关闭追踪避免子进程继承

        Zygote.initNativeState(isPrimaryZygote); // 初始化Native层状态
        ZygoteHooks.stopZygoteNoThreadCreation(); // 允许创建线程

        zygoteServer = new ZygoteServer(isPrimaryZygote); // 创建Socket服务端

        // 孵化system_server进程（系统服务核心）
        if (startSystemServer) {
            Runnable r = forkSystemServer(abiList, zygoteSocketName, zygoteServer);
            if (r != null) { // 仅在子进程（system_server）返回Runnable对象
                r.run();    // 进入SystemServer的main()方法
                return;     // 子进程在此返回，不再继续执行Zygote逻辑
            }
        }

        Log.i(TAG, "Accepting command socket connections");
        // 进入主循环，监听Socket请求（接受应用启动请求）
        caller = zygoteServer.runSelectLoop(abiList);
    } catch (Throwable ex) {
        Log.e(TAG, "System zygote died with exception", ex);
        throw ex;
    } finally {
        if (zygoteServer != null) {
            zygoteServer.closeServerSocket(); // 确保关闭Socket
        }
    }

    // 处理来自客户端的命令（fork出的子进程执行流程）
    if (caller != null) {
        caller.run(); // 执行子进程目标代码（如启动新APP）
    }
}
```

**（1）preload**

```java
static void preload(TimingsTraceLog bootTimingsTraceLog) {
    Log.d(TAG, "begin preload");
    //1
    bootTimingsTraceLog.traceBegin("BeginPreload");
    beginPreload();
    bootTimingsTraceLog.traceEnd(); // BeginPreload
    //2
    bootTimingsTraceLog.traceBegin("PreloadClasses");
    preloadClasses();
    bootTimingsTraceLog.traceEnd(); // PreloadClasses
    //3
    bootTimingsTraceLog.traceBegin("CacheNonBootClasspathClassLoaders");
    cacheNonBootClasspathClassLoaders();
    bootTimingsTraceLog.traceEnd(); // CacheNonBootClasspathClassLoaders
    //4
    bootTimingsTraceLog.traceBegin("PreloadResources");
    preloadResources();
    bootTimingsTraceLog.traceEnd(); // PreloadResources
    //5
    Trace.traceBegin(Trace.TRACE_TAG_DALVIK, "PreloadAppProcessHALs");
    nativePreloadAppProcessHALs();
    Trace.traceEnd(Trace.TRACE_TAG_DALVIK);
    //6
    Trace.traceBegin(Trace.TRACE_TAG_DALVIK, "PreloadGraphicsDriver");
    maybePreloadGraphicsDriver();
    Trace.traceEnd(Trace.TRACE_TAG_DALVIK);
    //7
    preloadSharedLibraries();
    //8
    preloadTextResources();
    // Ask the WebViewFactory to do any initialization that must run in the zygote process,
    // for memory sharing purposes.
    WebViewFactory.prepareWebViewInZygote();
    
    endPreload();
    warmUpJcaProviders();
    Log.d(TAG, "end preload");

    sPreloadComplete = true;
}
```

> - preloadClasses()用于初始化Zygote中需要的class类；
> - preloadResources()用于初始化系统资源；
> - nativePreloadAppProcessHALs();
> - maybePreloadGraphicsDriver();
> - preloadSharedLibraries()用于初始化系统libraries；
> - preloadTextResources()用于初始化文字资源；
> - prepareWebViewInZygote()用于初始化webview;

# 2.SystemServer进程

## 2.1SystemServer进程启动流程



