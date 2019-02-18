### 初始使用

```
git init
git add -A
git commit -m "your msg"
git remote add origin https://github.com/wbnature/test.git
git push -u origin master
```

### 以后使用

```
git add -A
git commit -m "your msg"
git push -u origin master
```

### 更新本地仓库

```
//方法一
$ git fetch origin master //从远程的origin仓库的master分支下载代码到本地的origin master

$ git log -p master.. origin/master//比较本地的仓库和远程参考的区别

$ git merge origin/master//把远程下载下来的代码合并到本地仓库，远程的和本地的合并

//方法二
$ git fetch origin master:temp //从远程的origin仓库的master分支下载到本地并新建一个分支temp

$ git diff temp//比较master分支和temp分支的不同

$ git merge temp//合并temp分支到master分支

$ git branch -d temp//删除temp
```

#### Git安装

~~~
sudo apt-get install git
~~~

#### 初始化

~~~
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
~~~

#### 创建文件夹

~~~
mkdir ~/code/gittest
cd ~/code/gittest
vim readme.txt
~~~

#### 初始化git仓库

~~~
git init
~~~

#### 添加文件给git托管

~~~
git add ./readme.txt
git add -A             
~~~

#### 提交改动

~~~
git commit -m "your msg"
~~~

#### 代码回滚

~~~
#把项目文件夹下的所有文件回退到上一个版本
git reset --hard HEAD^
#自然,回退上上个版本就是
git reset --hard HEAD^^
#如果要向前回退100个版本的话...
git reset --hard HEAD~100
~~~

#### 创建 SSH Key

~~~
ssh-keygen -t rsa -C "youremail@example.com"
~~~

如果一切顺利的话,可以在用户主目录里找到 .ssh 目录,里面有 id_rsa 和 id_rsa.pub 两个文件,这两个就是
SSH Key的秘钥对, id_rsa 是私钥,不能泄露出去, id_rsa.pub 是公钥,可以放心地告诉任何人。登陆GitHub,打开“Account settings”,“SSH Keys”页面, 然后点“Add SSH Key”,填上任意Title,在Key文本框里粘贴 id_rsa.pub 文件的内容:点“Add Key”,你就应该看到已经添加的Key.

#### 创建仓库

首先,登录github上,在右上角找到“create a new repo”创建一个新的仓库。填入仓库名称。点击Create
repository。完成。可以看到有一个http网址,值得注意的是github的http中间是github.com,而gitee是
gitee.com,除此之外此外github和gitee用法完全相同。

#### 将本地仓库关联至新创建的云仓库

~~~
git remote add origin https://github.com/......#这里应该输入的是之前创建云仓库时的出现的http网址
~~~

#### 推送本地代码到云端

~~~
git push -u origin master
~~~
#### 下载

~~~
git clone https://github.com/....这里填写你刚刚复制的链接
~~~
