## 1.下载code run和C++插件

![1617518789529](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518789529.png)

## 2.配置code run

![1617518813601](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518813601.png)

![1617518843515](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518843515.png)



## 3.创建并且编写一个简单的c程序

![1617518945833](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518945833.png)

然后运行：

![1617518968110](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518968110.png)



## 3.编辑调试配置文件

前提是已经下载好MingGw并且配置了环境变量

创建.vscode文件夹然后分别创建tasks.json和launch.json文件

![1617519080801](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617519080801.png)

然后分别配置：

```json
/*launch.json*/
{
    "version": "0.2.0",  
    "configurations": [  
        { 
            "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示
            "type": "cppdbg",       // 配置类型，这里只能为cppdbg
            "request": "launch",    // 请求配置类型，可以为launch（启动）或attach（附加）  
            "program": "${fileDirname}/${fileBasenameNoExtension}",// 将要进行调试的程序的路径  
            "args": [],             // 程序调试时传递给程序的命令行参数，一般设为空即可  
            "stopAtEntry": false,   // 设为true时程序将暂停在程序入口处，一般设置为false  
            "cwd": "${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceFolder}即代码所在目录 
            "environment": [],  
            "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台  
            "MIMode": "gdb",  
            "miDebuggerPath": "D:\\mingW\\install\\mingw32\\bin\\gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应  
            "preLaunchTask": "g++", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc  
            "setupCommands": [  
                {   
            "description": "Enable pretty-printing for gdb",  
                    "text": "-enable-pretty-printing",  
                    "ignoreFailures": true  
                }  
            ]  
        }  
    ]  
}

```



```json
/*tasks.json*/
{
    "version": "2.0.0",
    "command": "g++", // 注意对应
    "args": ["-g","${file}","-o","${fileDirname}/${fileBasenameNoExtension}"],    // 编译命令参数
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
            "file": 1,
            "line": 2,
            "column": 3,
            "severity": 4,
            "message": 5
        }
    }
}
```



## 调试：F5或者是菜单栏进行调试

**需要注意的是：调试的源程序文件存放的路径不允许有中文，否则会失败**

![1617519261337](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617519261337.png)



## 控制台出现中文乱码问题

点击右下角：

![1617521745758](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617521745758.png)

将编码模式换成GBK（Simplified Chinese）