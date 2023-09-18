# 一、简介

## 1.

## 2.安装

**(1)root目录下载repo工具**

```bash
mkdir ~/bin
PATH=~/bin:$PATH #将/bin目录下的文件都添加到系统环境变量，以便全局调用
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo#下载repo脚本，保存到~/bin目录下
chmod a+x ~/bin/repo
```



**(2)下载android 7.0源码**

首先配置git用户名和邮箱信息

![](ART虚拟机.assets/1694956593600.png)



```bash
export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo' #更换repo下载源
```

```bash
cd android-7.0
#下载nougat-release分支的代码
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-7.0.0_r33
#同步代码
repo sync
```

