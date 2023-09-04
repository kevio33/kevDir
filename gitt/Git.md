# Git

## 一.本地仓库链接github

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



## 三、文件

### 1、`.gitignore`

> 参考：
>
> https://www.freecodecamp.org/chinese/news/gitignore-file-how-to-ignore-files-and-folders-in-git/