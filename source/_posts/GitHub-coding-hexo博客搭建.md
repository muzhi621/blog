---
title: GitHub & coding + hexo博客搭建
tags: 
  - GitHub 
  - coding 
  - hexo 
categories: 教程
abbrlink: 70f18d21
date: 2019-03-12 19:21:19
copyright:
---

使用 GitHub Pages 和 Hexo 搭建免费独立博客的总结。将自己的域名访问 GitHub Pages 上的博客。同时，为了在多台电脑上都可以更新博客，采用两个分支的方式来存放文件，master 分支存放 Hexo 静态文件， 新建的source分支存放源文件。

### 必要配置

  在自己的 GitHub 账号下创建一个新的仓库，命名为 [username.github.io](http://username.github.io/)（username是你的账号名)。

### 安装 Git

```
sudo apt-get install git
```

### 配置 Git

  当安装完 Git 应该做的第一件事情就是设置用户名称和邮件地址。这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入你的每一次提交中，不可更改：

```
git config --global user.name "username"
git config --global user.email "username@example.com"
```

  对于 user.email，因为在 GitHub 的 commits 信息上是可见的，所以如果你不想让人知道你的 email，可以 Keeping your email address private:

1. 在GitHub右上方点击你的头像，选择`Settings`；
2. 在右边的`Personal settings`侧边栏选择`Emails`；
3. 选择`Keep my email address private`。

  这样，你就可以使用如下格式的 email 进行配置：

```
$ git config --global user.email "username@users.noreply.github.com"
```

### Git 与 GitHub

#### 与github建立联系

  为了能够在本地使用 git 管理 github 上的项目，需要进行一些配置，这里介绍 SSH 的方法。coding与GitHub类似

#### 检查电脑是否已经有 SSH keys。

```
ls ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

  默认情况下，public keys 的文件名是以下的格式之一：id_dsa.pub、id_ecdsa.pub、id_ed25519.pub、id_rsa.pub。因此，如果列出的文件有 public 和 private 钥匙对（例如id_ras.pub和id_rsa），证明已存在 SSH keys。

#### 如果没有 SSH key，则生成新的 SSH key。

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
```

  之后一路回车即可。

#### 向 ssh-agent 添加 key。

  首先确保 ssh-agent 可运行：

```
# start the ssh-agent in the background
ssh-agent -s
```

  然后添加 SSH key：

```
ssh-add ~/.ssh/id_rsa
```

#### 在 GitHub 添加 SSH key。

  首先，拷贝 key：

```
sudo cat ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your cllipboard
```

  然后，在 GitHub 右上方点击头像，选择`Settings`，在右边的`Personal settings`侧边栏选择`SSH Keys`。接着粘贴 key，点击`Add key`按钮。最后，测试链接：

```
ssh -T git@github.com
# Attempts to ssh to GitHub
```

  如果你看到：

```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```

  就键入：yes。之后将会看到如下信息：

```
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
```

### Hexo

#### 搭建 hexo 博客 并创建两个分支：master 与 source

#### 首先建立 hexo 博客

```
mkdir ~/Documents/Hexo
cd ~/Documents/Hexo
sudo npm install -g hexo-cli
hexo init
sudo npm install
sudo npm install hexo-deployer-git
```

#### 创建 source 分支，并使其为默认分支：

可直接在GitHub和coding上操作，并将source设置为默认分支

```
git init
git remote add origin git@github.com:username/username.github.io.git
git add .
#添加修改
git commit -m "init hexo"
#初次提交
git checkout -b source
#建立分支 hexo 并切换到分支 hexo
git push -u origin source
#将分支 hexo 提交到 github
```

#### 创建 空白分支 master

```
cd ..
#退回上一级目录
mkdir new
#创建一个新的文件夹用以创建空白分支
cd ~/Documents/new
git init
touch README.md
#随意创建一个文件，用于提交分支
git add .
git commit -m "new branch"
git remote add origin git@github.com:username/username.github.io.git
git push origin master
#将分支 master 提交到 github
rm README.md
git add .
git commit -m "clear new branch"
git push origin master
cd ~/Documents/
rm -rf new
cd Hexo
git pull
```

  执行完成之后，该仓库的默认分支被设为 source，同时还有空白的 master 分支用于存放网页。

#### 设置域名

  在 [username.github.io](http://username.github.io/) 仓库首页选择`Settings`，向下拉，在`GitHub Pages`部分的`Custom domain`中填上自己的域名，点击`save`保存。此操作会在 master 分支下生成一个 CNAME 文件，里面就是刚填写的域名。

#### 配置 hexo 提交方式

  编辑该文件夹下的`_config.yml`的`deploy`参数，分支应为 master。

默认生成的_config.yml：

```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
```

修改后的_config.yml：

```
deploy:
-
  type: git
  repository:  
    #github: git@github.com:muzhi621/muzhi621.github.io.git,master
    coding: git@git.dev.tencent.com:muzhi621/muzhi621.git,master
    #oschina: git@gitee.com:muzhi621/muzhi620.git
  #branch: master
```

#### 修改博客及部署操作

  修改博客内容后依次执行以下命令来提交网站相关的文件：

```
git add .
git commit -m "自定义内容即可"
git push origin source
```

  然后执行以下任意一条生成网站并部署到 GitHub 上。

```
hexo generate -d
hexo g -d
```

  这样一来，在 GitHub 上的 [username.github.io](http://username.github.io/) 仓库就有两个分支，一个 source 分支用来存放网站的原始文件，一个 master 分支用来存放生成的静态网页。

#### 域名重置问题及解决方案

##### 问题

  每次执行完`hexo g -d`之后，github 仓库设置中的 `Custom domain`总是被重置，导致域名访问出现 404 错误。

##### 解决方案

  在 Hexo 生成的博客的 source 目录下新建一个 CNAME 文件，里面填上自己的域名即可。

#### 博客管理流程

  在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：

1. 依次执行`git add .`、`git commit -m "..."`、`git push origin source`指令将改动推送到 GitHub（此时当前分支应为 source）；
2. 然后才执行 `hexo g -d` 或 `hexo generate -d` 发布网站到 master 分支上。

#### 更换电脑时：

1. 使用 `git clone git@github.com:username/username.github.io.git` 拷贝仓库（默认分支为 source）；
2. 在本地新拷贝的`username.github.io`文件夹下通过终端依次执行下列指令：`sudo npm install -g hexo-cli`、`sudo npm install`、`sudo npm install hexo-deployer-git`