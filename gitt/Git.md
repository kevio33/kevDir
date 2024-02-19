# Git

## 一.电脑通过ssh链接github

> 参考：
>
> https://developer.aliyun.com/article/1049888
>
> https://blog.csdn.net/wuyujin1997/article/details/106165502
>
> [菜鸟](https://www.runoob.com/git/git-tutorial.html)

### 1.步骤

- 将自己的电脑与指定github账户关联

  > 创建一个github账户
  >
  > 在本机上设置你的github的邮箱和用户名
  >
  > git config --global user.name "用户名"
  > git config --global user.email "邮箱"
  >
  > > 之后会在`C:\Users\89464\.gitconfig`文件里出现刚刚添加的用户名

- 生成本机的SSH key

  > ```shell
  > ssh-keygen -t rsa -C "邮箱"
  > ```
  >
  > 根据提示，按三次回车键，最后会生成ssh key值，并告诉你key值存放的文件的位置，找到该文件并复制ssh key

- 打开github主页。找到Settings,

  > 选择SSH and GPG keys ，再选择new SSH key，title 随意填，key值就是刚刚生成的本机 SSH KEY（在 `.pub`文件中）

  

### 2.git clone每次输入密码

> 参考：
>
> [git clone 每次输入密码](https://juejin.cn/s/git%20clone%20%E6%AF%8F%E6%AC%A1%E9%83%BD%E8%A6%81%E8%BE%93%E5%85%A5%E5%AF%86%E7%A0%81)

每次使用`git clone`命令从远程仓库克隆代码时都需要输入密码，那么可能是因为使用的是`HTTP协议`进行访问。为了避免每次都需要输入密码，可以尝试使用`SSH协议`进行访问，这样就可以通过SSH密钥进行身份验证，而无需输入密码。 

> 前提是`在1.步骤`中创建好了ssh公钥，并添加到github中
>
> 然后可以使用SSH协议进行克隆 
>
> ```shell
> git clone git@github.com:user/repo.git #就是仓库的ssh路径
> ```



### 3.报错

**(1)The file will have its original line endings in your working directory**

> 参考：
>
> https://blog.csdn.net/qq_37521610/article/details/88327286

**问题描述**

 git add：添加至暂存区，但并未提交至服务器。git add . 是表示把当前目录下的所有更新添加至暂存区。有时在终端操作这个会提示： 

```
warning: LF will be replaced by CRLF in ball_pool/assets/Main.js.
The file will have its original line endings in your working directory
```



**原因：**

因为文件中换行符的差别导致的。这个提示的意思是说：会把windows格式（CRLF（也就是回车换行））转换成Unix格式（LF），这些是转换文件格式的警告，不影响使用。 

**解决办法**

```shell
git rm -r --cached .
git config core.autocrlf false
git add .
git commit -m ''
git push
```



----------



## 二.command

> 参考：
>
> [菜鸟——git命令](https://www.runoob.com/git/git-basic-operations.html)

> base step 4
>
> ```bash
> git add .
> git commit -m ''
> git pull
> git push
> ```



## 三.回退版本

### 1.回退某个文件版本

> [git回退文件版本](https://blog.csdn.net/panweiwei1994/article/details/78501371)

**场景1：**修改了文件/path/to/file，没有提交，但是觉得改的不好，想还原。

```
git checkout -- /path/to/file
```



**场景二：** 修改了文件/path/to/file，已经提交，但是觉得改的不好，想还原到上一版本。 

```shell
#1.首先查看文件的历史版本。
git log /path/to/file

#2. 找到你想要还原的版本。如:
commit 052c0233bcaef35bbf6e6ebd43bfd6a648e3d93b
Author: panww <panww@gmail.com>
Date: Wed Nov 8 11:48:31 2017 +0800
commit modify/path/to/file

#3. 将文件还原到你想要还原的版本。$ git checkout ${commit} /path/to/file。即
git checkout 052c0233bcaef35bbf6e6ebd43bfd6a648e3d93b /path/to/file

```



### 2.回退版本

> [git回退版本](https://blog.csdn.net/yxlshk/article/details/79944535)

**git版本管理知识**
使用git的每次提交，Git都会自动把它们串成一条时间线，这条时间线就是一个分支。如果没有新建分支，那么只有一条时间线，即只有一个分支，在Git里，这个分支叫主分支，即master分支。有一个HEAD指针指向当前分支（只有一个分支的情况下会指向master，而master是指向最新提交）。每个版本都会有自己的版本信息，如特有的版本号、版本名等。如下图，假设只有一个分支：
![1707051546850](Git.assets/1707051546850.png) 

**如何回退版本**

**（1）git reset**

 git reset的作用是修改HEAD的位置，即将HEAD指向的位置改变为之前存在的某个版本 

![1707051522549](Git.assets/1707051522549.png) 



**具体操作：**

 **查看版本号：**
可以使用命令“git log”查看： 

![1707051507624](Git.assets/1707051507624.png) 

> 也可以在github网站查看

然后**使用`git reset --hard 目标版本号`命令将版本回退** 

 **使用“git push -f”提交更改：**
此时如果用“git push”会报错，因为我们本地库HEAD指向的版本比远程库的要旧： 

 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTgwNDE0MjAzNjA1Nzk3) 

 所以要用`git push -f`强制推上去 



**（2）git revert**

**原理**：` git revert`是用于“反做”某一个版本，以达到撤销该版本的修改的目的。比如，我们commit了三个版本（版本一、版本二、 版本三），突然发现版本二不行（如：有bug），想要撤销版本二，但又不想影响撤销版本三的提交，就可以用 git revert 命令来反做版本二，生成新的版本四，这个版本四里会保留版本三的东西，但撤销了版本二的东西。如下图所示：
 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTgwNDE0MjA1ODE2MTg4) 

**查看版本号：**
可以通过命令行查看（输入git log）：
如图，最近的两个版本分别叫：“add text.txt”（即新增了文件text.txt）、“add text2.txt”（新增了文件text2.txt）。这个时候我们不需要text.txt这个文件了，那就是说不想要“add text.txt”那个版本的操作，那可以通过反做“add text.txt”这个版本来实现。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190726105234748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l4bHNoaw==,size_16,color_FFFFFF,t_70) 

 **使用“git revert -n 版本号”反做，并使用“git commit -m 版本名”提交：**
（1）反做，使用“git revert -n 版本号”命令。如下命令，我们反做版本号为8b89621的版本： 

```shell
git revert -n 8b89621019c9adc6fc4d242cd41daeb13aeb9861
```

 **注意：** 这里可能会出现冲突，那么需要手动修改冲突的文件。而且要git add 文件名。
（2）提交，使用“git commit -m 版本名”，如： 

```shell
git commit -m "revert add text.txt" 
```





### 3.撤销add 和commit

https://www.cnblogs.com/FengZeng666/p/15753153.html



## 四.查看远程更新

> https://juejin.cn/s/git%20%E6%9F%A5%E7%9C%8B%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF%E6%98%AF%E5%90%A6%E6%9C%89%E6%9B%B4%E6%96%B0



## 五.本地仓库链接远程仓库

首先在本地项目初始化仓库

```shell
git init
```

然后关联本地仓库和远程仓库

```shell
git remote add origin 仓库地址
```

然后就可以推送内容了

```shell
git add .
git commit -m "Initial commit"
git push -u origin master
```



## 六.问题

### 1.ssh同时绑定gitee和github

> [同时使用Gitee和Github并设置代理](https://duter2016.github.io/2021/01/22/Git%E5%90%8C%E6%97%B6%E4%BD%BF%E7%94%A8Gitee%E5%92%8CGithub%E5%B9%B6%E8%AE%BE%E7%BD%AE%E4%BB%A3%E7%90%86/)
>
> [github和gitee配置](https://www.css3er.com/p/347.html)

最开始我创建一个ssh同时用于`gitee`和`github`，但是之后发现gitee可以用，但是github一直报`ssh: connect to host github.com port 22: Connection timed out`的错，所以怀疑是不是因为一个ssh不能同时用两个网站。

#### (1)分别绑定gitee和GitHub

所以我**首先按照操作给gitee和github分别设置了ssh密钥。**

```shell
#先删除全局的名字和邮箱
$ git config --global --unset user.name "你的名字"
$ git config --global --unset user.email "你的邮箱"

#生成GitHub 的钥匙
ssh-keygen -t rsa -f "~/.ssh/id_rsa.github" -C "邮箱"

#生成Gitee 的钥匙
ssh-keygen -t rsa -f "~/.ssh/id_rsa.gitee" -C "邮箱"
```



**多账户必须配置config文件**，若无 config 文件，则需创建 config 文件 

```
#Default gitHub user Self
Host github.com
Hostname ssh.github.com
Port 443
HostName github.com
User your@email
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa.github

#Add gitee user
Host gitee.com
HostName gitee.com
User your@email
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa.gitee
```

> > - Host
> >   它涵盖了下面一个段的配置，我们可以通过他来替代将要连接的服务器地址。
> >   这里可以使用任意字段或通配符。
> >   当ssh的时候如果服务器地址能匹配上这里Host指定的值，则Host下面指定的HostName将被作为最终的服务器地址使用，并且将使用该Host字段下面配置的所有自定义配置来覆盖默认的/etc/ssh/ssh_config配置信息。
>
> > - Port
> >   自定义的端口。默认为22，可不配置
>
> > - User
> >   自定义的用户名，默认为git，设置为你的注册邮箱,也可不配置，不写邮箱，每次都报please tell who you are！
>
> > - HostName
> >   真正连接的服务器地址
>
> > - PreferredAuthentications
> >   指定优先使用哪种方式验证，支持密码和秘钥验证方式
>
> > - IdentityFile
> >   指定本次连接使用的密钥文件



**之后再把ssh公钥添加到github和gitee上面，在本地进行测试：**

```shell
ssh -T git@github.com

ssh -T git@gitee.com
```

> **如果报错` Bad owner or permissions on /home/username/.ssh/config `**
>
> 可以参考如下：
>
> [报错bad owner](https://support.huaweicloud.com/modelarts_faq/modelarts_05_3211.html)



#### (2)同时绑定gitee和github

> [绑定gitee和GitHub](https://www.css3er.com/p/347.html)



## 文件

### 1.`“.gitignore”`

> 参考：
>
> https://www.freecodecamp.org/chinese/news/gitignore-file-how-to-ignore-files-and-folders-in-git/

#### (1)gitignore应该包含什么

其中一些可能包括：

- 操作系统文件。每个操作系统（如 macOS、Windows 和 Linux）都会生成系统特定的隐藏文件，其他开发者不需要使用这些文件，因为他们的系统也会生成这些文件。例如，在 macOS 上，Finder 会生成一个 `.DS_Store` 文件，其中包括用户对文件夹的外观和显示的偏好，如图标的大小和位置。
- 由代码编辑器和 IDE（IDE 代表集成开发环境）等应用程序生成的配置文件。这些文件是为你、你的配置和你的偏好设置定制的。
- 从你的项目中使用的编程语言或框架自动生成的文件，以及编译后的代码特定文件，如 `.o` 文件。
- 由软件包管理器生成的文件夹，如 npm 的 `node_modules` 文件夹。这是一个用于保存和跟踪你在本地安装的每个软件包的依赖关系的文件夹。
- 包含敏感数据和个人信息的文件。这类文件的一些例子是含有你的凭证（用户名和密码）的文件和含有环境变量的文件，如 `.env` 文件（`.env` 文件含有需要保持安全和隐私的 API 密钥）。
- 运行时文件，如 `.log` 文件。它们提供关于操作系统的使用活动和错误的信息，以及在操作系统中发生的事件的历史。

#### (2)忽略操作

**忽略一个文件和文件夹**

如果想忽略一个根目录下的`text.txt`文件

```
/text.txt
```

如果想忽略一个根目录下的目录`test`下的`text.txt`文件

```
/test/text.txt
或者
test/text.txt
```

如果想忽略所有特定名称的文件：例如忽略任何`text.txt`文件：

```
text.txt
```



如果想忽略目录及其所有内容( 这个命令将忽略位于你的项目中任何地方的名为 `test` 的目录（包括目录中的其他文件和其他子目录))

```
test/
```



忽略所有以`img`开头的文件和目录

```
img*
```

忽略所有以`.md`文件结尾的文件

```
*.md
```



**不忽略某些文件**

有些文件可能是例外，例如`README.md`

```
# 忽略所有 .md 文件
.md

# 不忽略 README.md 文件
!README.md
```

> **注意，如果是一个已经被忽略的目录，要想不忽略它底下的文件和目录是不可行的**
>
> ```
> # 忽略所有名字带有 test 的目录
> test/
> 
> # 试图在一个被忽略的目录内排除一个文件是行不通的
> !test/example.md
> ```
>

