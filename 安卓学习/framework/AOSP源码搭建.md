> 参考——
>
> [VMWare环境搭建](https://www.cnblogs.com/EthanS/p/18211302)
>
> [配置过程](https://www.bilibili.com/video/BV14g4y1x7Eo/?spm_id_from=333.1387.collection.video_card.click&vd_source=393163f970d7c1ef4544ef8af35b3690)

## 1.开发环境搭建

- 虚拟机——[VMWare](https://www.cnblogs.com/EthanS/p/18211302)

- 镜像——[Ubuntu 20.04.6 LTS (Focal Fossa)](https://releases.ubuntu.com/focal/)

> 环境搭配参考上面[配置过程](https://www.bilibili.com/video/BV14g4y1x7Eo/?spm_id_from=333.1387.collection.video_card.click&vd_source=393163f970d7c1ef4544ef8af35b3690)和[图文](https://github.com/yuandaimaahao/AndroidFrameworkTutorial/blob/main/2.AOSP%E4%B8%8A%E6%89%8B%E6%8C%87%E5%8D%97/001.AOSP%20%E6%9E%81%E9%80%9F%E4%B8%8A%E6%89%8B.md)

- 下载linux需要的软件包：

```shell
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig python
```

## 2.下载，编译源码

### 下载repo工具

```shell
mkdir ~/bin
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo -o ~/bin/repo
chmod +x ~/bin/repo
```

> **注意：**
>
> repo 的运行过程中会尝试访问官方的 git 源更新自己，如果想使用 tuna 的镜像源进行更新，可以**将如下内容复制到 ~/.bashrc 或者 ~/.zshrc 里**。 
>
> ```shell
> export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'
> PATH=~/bin:$PATH
> ```
>
> 然后source执行，使更改的全局变量生效
>
> ```shell
> source ~/.bashrc
> #如果使用的是 zsh
> #source ~/.zshrc
> ```

### 初始化仓库并同步远程代码

```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
mkdir aosp 
cd asop
#初始化仓库,-b 指示分支，这里使用 android10
repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b android-10.0.0_r41
#同步远程代码
repo sync
```

> 这一步花费的时间相当长，2个小时左右

结束之后的目录：
![1748915192081](AOSP源码搭建.assets/1748915192081.png)

### 编译源码

```shell
source build/envsetup.sh 

lunch  
26. aosp_x86_64-eng #选择x86

#或者直接选择指定的编译版本
lunch aosp_x86_64-eng
```

- 命令一：加载 AOSP 编译环境：该脚本定义了编译 Android 所需的函数、环境变量和别名（如 mm、mma 等快捷命令）。
- 命令二：选择目标编译配置：指定编译的 Android 版本、CPU 架构和构建类型。eng表示构建类型为 **Engineer (eng)**，包含调试工具和 root 权限。 



然后使用`make`来编译

```shell
m #自动根据cpu情况选择线程数

make -j16 #指定16线程
```

> 第一次差不多2个半小时

然后使用emulator命令起模拟器和Android系统：

```shell
emulator -verbose -cores 4 -show-kernel
#运行模拟器可以直接emulator命令
```

> **首次启动一定要带这几个参数，否则起不起来**

![1748926053374](AOSP源码搭建.assets/1748926053374.png)

