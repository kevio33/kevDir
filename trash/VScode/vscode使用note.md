

# 一、配置开发环境

## 1.配置C/C++环境

> 参考：
>
> [vscode 配置c/c++](https://blog.csdn.net/weixin_42294124/article/details/124766738)

**下载code run和C++插件**

![1617518789529](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518789529.png)

**配置code run**

![1617518813601](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518813601.png)

![1617518843515](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518843515.png)

> 第二个设置是希望运行时候能够在终端运行，方便控制台输入



**下载MinGW**

 C/C++的编译器有很多种，这里选择开源的MinGW编译器。 

> https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/
>
> ![1693119834538](vscode使用note.assets/1693119834538.png)

**配置系统环境变量**

![1693119913024](vscode使用note.assets/1693119913024.png)

**打开cmd，输入gcc -v验证是否成功**





**配置编译器：**

 按快捷键Ctrl+Shift+P调出命令面板，或者点菜单栏 帮助—显示所有命令，输入C/C++，选择“Edit Configurations(UI)”进入配置。 

![1693119987590](vscode使用note.assets/1693119987590.png)

![1693119996747](vscode使用note.assets/1693119996747.png)

**配置两个选项：**

> 1. 编译器路径：D:\A_Software\mingw64\bin\g++.exe
> 2. IntelliSense 模式：windows-gcc-x64

![1693120033587](vscode使用note.assets/1693120033587.png)

![1693120046625](vscode使用note.assets/1693120046625.png)

 配置完成后，此时在侧边栏可以发现多了一个.vscode文件夹，并且里面有一个c_cpp_properties.json文件， 

**创建并且编写一个简单的c程序**

![1617518945833](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518945833.png)

**然后运行：**

![1617518968110](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617518968110.png)



**编辑调试配置文件**

前提是已经下载好MingGw并且配置了环境变量

创建.vscode文件夹然后分别创建tasks.json和launch.json文件

![1617519080801](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617519080801.png)

**然后分别配置：**

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
            "miDebuggerPath": "[自己mingw的路径]bin\\gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应  
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

**调试：F5或者是菜单栏进行调试**

**需要注意的是：调试的源程序文件存放的路径不允许有中文，否则会失败**

![1617519261337](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617519261337.png)

**这样做只可以调试c(遇到sanf会出现问题)，c++不能成功**



#### Cygwin

> [vscode_cygwin配置c++环境](https://blog.csdn.net/chenhao0568/article/details/105300350)

------------

### 问题：

**（1）控制台出现中文乱码问题**

点击右下角：

![1617521745758](https://gitee.com/kevinyong/kevin-gallery/raw/master/1617521745758.png)

将编码模式换成GBK（Simplified Chinese）



### (2)配置cmake

> 参考：
>
> [vscode cmake](https://blog.csdn.net/weixin_43470971/article/details/119621643?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-119621643-blog-93545513.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-119621643-blog-93545513.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=2)
>
> https://www.jianshu.com/p/ad29eee7b736

C++ 工程，有四个必要的`json`配置文件，先`ctrl+shift+p`打开输入指令分别是：

- `c_cpp_properties.json `：配置项目结构，自动生成和更新，输入`C/C++:Edit configuration` 
- `task.json`: 构建和编译运行项目，输入`Task:Configure Task`，模板，Others
- `launch.json`: 调试，读取可执行文件

**下载Cmake**

**在Vscode中创建cmake配置文件：使用的编译器等等**

![1695276544499](vscode使用note.assets/1695276544499.png)

**然后会让你输入要编译的项目名，直接输入写的cpp代码目录即可**

**之后目录下面出现`build`目录和`CMakeLists.txt`**

![1695276738712](vscode使用note.assets/1695276738712.png)

`CMakeLists.txt`文件内容如下

```
cmake_minimum_required(VERSION 3.0.0)
project(CPPTEST VERSION 0.1.0 LANGUAGES C CXX)#项目名字和版本

include(CTest)#包含测试框架，我们可以不用
enable_testing()

add_executable(CPPTEST  test.cpp Student.cpp Student.h)#添加源文件

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
```



**在命令行运行编译文件**

```shell
cmake --build build #build指build文件夹路径
```

然后会在build目录下生成可执行文件，直接运行即可

![1695276924440](vscode使用note.assets/1695276924440.png)

![1695276942253](vscode使用note.assets/1695276942253.png)

如果要更换可执行文件生成位置

```
set(EXECUTABLE_OUTPUT_PATH ..)
```



**手动添加源文件很麻烦**

[cmake：同一目录下多个源文件](https://blog.csdn.net/zhizhengguan/article/details/111713182)

[cmake：不同目录下多个源文件](https://blog.csdn.net/hbsyaaa/article/details/123284434#:~:text=Linux%E4%B8%ADCMake%E7%9A%84%E4%BD%BF%E7%94%A83-%E4%B8%8D%E5%90%8C%E7%9B%AE%E5%BD%95%E5%A4%9A%E4%B8%AA%E6%BA%90%E6%96%87%E4%BB%B6%201%201%20%E6%9C%891%E4%B8%AA%E7%8B%AC%E7%AB%8B%E6%96%87%E4%BB%B6%E5%A4%B9%201.1%20%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%20%E5%85%88%E6%9D%A5%E7%9C%8B%E4%B8%8B%E9%9D%A2%E8%BF%99%E7%A7%8D%E6%83%85%E5%86%B5%EF%BC%8C%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E5%A6%82%E4%B8%8B%EF%BC%8Csum.c%E3%80%81sum.h%E5%92%8Cmain.c%E7%9A%84%E4%BB%A3%E7%A0%81%E5%86%85%E5%AE%B9%E8%A7%81%E5%89%8D%E9%9D%A2%E7%9A%84%E6%96%87%E7%AB%A0%E3%80%82%20...,%E6%80%BB%E7%BB%93%20%E6%9C%AC%E7%AF%87%E4%BB%8B%E7%BB%8D%E4%BA%86%E4%B8%8D%E5%90%8C%E7%9B%AE%E5%BD%95%E4%B8%8B%E6%9C%89%E5%A4%9A%E4%B8%AA%E6%BA%90%E6%96%87%E4%BB%B6%E6%97%B6%E5%A6%82%E4%BD%95%E7%BC%96%E5%86%99CMakeLists.txt%E3%80%82%20%E5%AF%B9%E4%BA%8E%20%E4%B8%8D%E5%90%8C%E6%96%87%E4%BB%B6%E5%A4%B9%E4%B8%8B%E7%9A%84%E5%A4%9A%E4%B8%AA%E6%BA%90%E6%96%87%E4%BB%B6%20%EF%BC%8C%E4%B8%BB%E8%A6%81%E6%98%AF%E4%BD%BF%E7%94%A8%20include_directories%20%E6%9D%A5%E6%B7%BB%E5%8A%A0%E5%A4%B4%E6%96%87%E4%BB%B6%E7%9A%84%E6%90%9C%E7%B4%A2%E7%9B%AE%E5%BD%95%20)

## 2.配置python

**配置调试环境**

> 参考—— [ Vscode工作区调试（虚拟环境）配置](https://blog.csdn.net/q1_6_2_what_123/article/details/109125858) 

首先在调试区的`lauch.json`添加想要调试的虚拟环境路径

> ![1693124461994](vscode使用note.assets/1693124461994.png)

然后选择添加调试名进行调试

![1693124496102](vscode使用note.assets/1693124496102.png)

### 问题：

**(1)虚拟环境包补全问题：**

> 参考——[vscode python 第三方库自动补全 第三方库的智能提示](https://blog.csdn.net/weixin_40583088/article/details/119685872) 

**①首先，先卸载Pylance**

**②配置setting——python——setting.json**

插入三方包的路径

![1693124560768](vscode使用note.assets/1693124560768.png)

**③然后再次安装pylance，重启vscode即可**



## 3.配置JAVA

> [vscode配置Java](https://blog.csdn.net/weixin_51456573/article/details/114727769)



## 4.配置kotlin

> https://blog.csdn.net/m0_50275697/article/details/128002833



## 5.配置jupyter

> https://cangmang.xyz/articles/1642849032438

# 二、usage

## 1.自动格式化代码

![1695190746467](vscode使用note.assets/1695190746467.png)



## 2.cmd终端乱码

> [vscode终端乱码](https://www.cnblogs.com/stu-jyj3621/p/12815080.html)

### (1)方法一

VSCode终端其实调用的是cmd.exe，所以当这里出现中文乱码的时候要解决的是cmd的编码设置问题。

1. 可以通过 chcp 命令查看 cmd 的编码设置，GBK2312 的代码页编号是 *936*，然后改成utf-8的编码即可，
2. utf-8 对应的代码页编号是 65001 ，所以执行 **chcp 65001** 就可以把cmd的编码设置成uft-8了，
3. 这样就解决了乱码问题，然后可以再次运行代码查看输出 

当然每次使用都输入一遍 chcp 65001 太烦了 ，可以直接在setting.json 中加上 

```json
"code-runner.executorMap":{
    "cpp":"chcp 65001 "
},
```

# 三、shortcut

## 1.调出命令窗口

`ctrl+shift+p`

![1696738494336](vscode使用note.assets/1696738494336.png)

也可以通过菜单栏调出

![1696738526931](vscode使用note.assets/1696738526931.png)



## 2.调出设置面板

`ctrl+,`



## 3.切换标签页

> 参考——[切换标签页快捷键](https://juejin.cn/s/vscode%20%E5%88%87%E6%8D%A2%E6%A0%87%E7%AD%BE%E9%A1%B5%E5%BF%AB%E6%8D%B7%E9%94%AE)

`Ctrl + Tab` 或 `Ctrl + Page Down`或`alt+ ->`



## 4.调出侧面板

`ctrl+b`



## 5.查找文件

`ctrl+p`

![1697977081849](vscode使用note.assets/1697977081849.png)

## 6.在代码页面快速移动

`ctrl+>`能够逐个单词的移动

`shift+>`能够逐个字符选中



## 7.选中并修改某个选中的单词

`F2`

> **注意，这会替换文件中所有的相同单词**

## 8.返回顶/底端

`ctrl+home`

`ctrl+end`

## 9.将文件在右侧展开

`ctrl+\ 	 `

## 10.关闭文件

`ctrl+w`

## 11.变换窗口

`win+上下左右键`



# 四、远程连接服务器

> 参考——https://blog.csdn.net/weixin_73757883/article/details/141141303

## 4.1连接linux服务器

主要设计步骤包括：

- Linux安装openssh-server
- vscode安装remote-ssh
- 配置vscode的ssh文件



### linux安装openssh-server

- 对于使用`apt`作为包管理工具的`Debian类系统`或者`Ubuntu`：

```shell
sudo apt-get remove openssh-server      # 先卸载，无论咋样都先做一下，以免出现问题
sudo apt-get install openssh-server     # 安装
sudo service ssh --full-restart     # 重启ssh 服务
sudo systemctl enable ssh       # 自动启动
```

> 对于使用`yum`作为包管理工具的`CentOS`为代表的：
>
> ```shell
> yum install -y openssl openssh-server   # 安装
> systemctl restart sshd.service  # 重启 ssh 服务
> systemctl enable sshd   # 自动启动
> ```

- 来配置一下文件：

```shell
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup # 无论怎样，备份是个好习惯
sudo vim /etc/ssh/sshd_config
```

 文件内容需要修改的地方如下（把对应的内容添加上去或改一下，去掉前面的#注释符）： 

```shell
Port 22     # 默认连接端口为22
PermitRootLogin yes
PasswordAuthentication yes
AllowUsers xxx # 这里的 "xxx" 改成你自己的登陆用户名
RSAAuthentication yes
PubKeyAUthentication yes
```

配置完成后重启ssh服务

```shell
sudo service ssh --full-restart
```

### vscode安装remote-ssh插件

- 首先应用商店搜索插件



- 然后配置ssh：

  在vscode界面搜索界面输入`>`或者直接快捷键`ctrl+shift+p`，然后输入ssh，点击打开ssh配置文件

  ![1749089289714](vscode使用note.assets/1749089289714.png)

查看虚拟机的ip地址，然后再ssh配置文件输入如下内容

![1749089365526](vscode使用note.assets/1749089365526.png)

然后尝试远程连接虚拟机，还是刚才搜索界面输入ssh，然后点击连接当前窗口到主机，会提示输入虚拟机类型、密码，就可以连接到主机了。

![1749089419692](vscode使用note.assets/1749089419692.png)

![1749089476951](vscode使用note.assets/1749089476951.png)

### linux端配置固定IP

下载net-tools之后，进入netplan目录

```shell
sudo apt install net-tools -y  
cd /etc/netplan
# 备份旧的配置文件
sudo cp 01-network-manager-all.yaml  01-network-manager-all.yaml.backup
```

修改文件

```shell
# 修改配置文件：
sudo vim 01-network-manager-all.yaml

network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s5:   # 网卡名称，通过ifconfig查看
      dhcp4: no     # 关闭dhcp
      dhcp6: no
      addresses: [10.0.0.89/24]  # 静态ip，根据自己网络情况配置
      gateway4: 10.0.0.1     # 网关，根据自己网络情况配置
      nameservers:
        addresses: [10.0.0.1, 114.114.114.114] #dns，根据自己网络情况配置
```



## 远程服务器连不了网情况

> https://blog.csdn.net/chongbin007/article/details/126958840
>
> 

### 问题描述

对于有的服务器无法访问外网，在连接的时候会报错，例如：

> Failed to download VS Code Server....

这是因为服务器需要下载vscode远程连接需要的插件包，而服务器没联网就下载不了

### 解决办法

对于不同vscode版本，目前有两种解决方法

首先查看**服务器的`.vscode-server/`目录**中尝试下载的内容是什么

- 下载的tar.gz包带有`cli`
- 下载的tar.gz包只带有server

无论那种方法，都需要先查看vscode的`commitId`

> help->about
>
> ![1751793733893](vscode使用note.assets/1751793733893.png)

我们需要手动下载tar.gz包，然后传到服务器上，才可以



#### 带有cli

这应该是高版本vscode会下载的包

在本机上的浏览器通过下面链接下载

> ```
> #cli包
> https://vscode.download.prss.microsoft.com/dbazure/download/stable/${commit_id}/vscode_cli_alpine_x64_cli.tar.gz
> 
> #server包
> https://vscode.download.prss.microsoft.com/dbazure/download/stable/${commit_id}/vscode-server-linux-x64.tar.gz
> ```
>
> **commit_id替换为自己vscode的id**

>  也可以在vscode的报错日志中找到要下载的`cli`链接

下载完成后发送到远程服务器的`.vscode-server`目录下，然后解压重命名`cli`包

```shell
tar -zxvf vscode_cli_alpine_x64_cli.tar.gz
mv code code-${commit_id} # 重命名，把:${commit_id}替换成自己的Commit ID
```



这个时候在尝试链接还是会报错，但是会创建目录`cli/servers/Stable-$commitId$`

 ![在这里插入图片描述](vscode使用note.assets/1120ff3b0a0e4568984afee082a133da.png) 



**接下来把server包解压，移动到Stable-CommitId中，解压之后重命名为server**

```shell
tar -zxvf vscode-server-linux-x64.tar.gz

mv vscode-server-linux-x64 server

mv server ./cli/server/Stable-${commit_id} # 把:${commit_id}替换成自己的Commit ID   
```

> 这个目录下除了解压的文件不可以有其他目录文件！

之后重启vscode再连接就ok了



#### 只带有server

在`.vscode-server`目录下创建`bin`目录

```shell
mkdir ./vscode-server/bin
```

然后将`vscode-server-linux-x64.tar.gz`包放到bin目录下，解压并重命名为`CommitId`

```shell
mv vscode-server-linux-x64 ./vscode-server/bin/$commit-id$
```

最后删除压缩包，重新连接