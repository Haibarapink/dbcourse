# Lab0 实验环境配置

- [Lab0 实验环境配置](#lab0-实验环境配置)
  - [Miniob 仓库与Lab文档地址](#miniob-仓库与lab文档地址)
  - [开发环境配置](#开发环境配置)
    - [方式一：(推荐)线上开发环境](#方式一推荐线上开发环境)
    - [方式二：虚拟机+`vscode remote`开发](#方式二虚拟机vscode-remote开发)
      - [1. 安装Ubuntu](#1-安装ubuntu)
      - [2. 配置环境](#2-配置环境)
      - [3. 安装必要软件](#3-安装必要软件)
      - [4. 安装miniob](#4-安装miniob)
      - [5. vscode插件配置](#5-vscode插件配置)
      - [6. debug简单教程](#6-debug简单教程)
    - [方式三： Docker中开发代码](#方式三-docker中开发代码)
  - [实验提交](#实验提交)
    - [准备工作](#准备工作)
    - [提交过程](#提交过程)
  - [注意事项](#注意事项)
  - [可能会出现的问题](#可能会出现的问题)
    - [Github的网络没有问题，但是push代码失败](#github的网络没有问题但是push代码失败)
  

## Miniob 仓库与Lab文档地址
有`C/C++`开发经验同学可以`clone`下面代码。
miniob代码仓库地址: https://github.com/oceanbase/miniob
lab文档: https://oceanbase.github.io/miniob/db_course_lab/overview/

## 开发环境配置

### 方式一：(推荐)线上开发环境
[线上环境文档](https://github.com/oceanbase/miniob/blob/main/docs/docs/db_course_lab/cloudlab_setup.md)

### 方式二：虚拟机+`vscode remote`开发
#### 1. 安装Ubuntu
Ubuntu下载地址：[下载](https://cn.ubuntu.com/download/desktop)

点击下载
![](images/setup_download_ubuntu.png)
选择典型的类型配置，点击下一步
![](images/setup_init_ubuntu1.png)
找到刚刚从网站下载的iso文件，点击下一步
![](images/setup_init_ubuntu2.png)
设置名称和密码
![](images/setup_init_ubuntu4.png)
设置虚拟机名称和虚拟机数据存放位置
![](images/setup_init_ubuntu5.png)
设置磁盘大小，推荐40~80G，点击下一步
![](images/setup_init_ubuntu6.png)
点击完成即可
![](images/setup_init_ubuntu7.png)
虚拟机开机之后，不断点击`Next`即可, 注意这里选择`Install Ubuntu`，后续操作也是不断点击`Next`。
![](images/setup_init_ubuntu8.png)
输入账号名和密码
![](images/setup_init_ubuntu9.png)
后续就点击`Next`，最后安装`Ubuntu`，等待安装`Ubuntu`完毕，安装完毕之后点击`Restart Now`即可。

#### 2. 配置环境
登录，进入终端，输入以下命令:
1. 安装网络工具
```
sudo apt update && sudo apt -y upgrade
sudo apt install -y net-tools openssh-server
```
2. 输入命令`ssh-keygen -t rsa`，然后一路回车，生成密钥。 
![](images/setup_init_sshd.png)
3. 然后检查`ssh`服务器的状态，输入命令：`sudo systemctl status ssh`或`sudo systemctl status sshd`
![](images/setup_check_status_ssh.png)
**注意这里可能出现的错误**：
* 上图中绿色的`active`状态是红色的，表示`sshd`没有启动，使用命令`sudo systemctl restart ssh`或者`sudo systemctl restart sshd`。
* `systemctl`找不到`sshd`/`ssh`服务，这里可以尝试输入下面两个命令:`ssh-keygen -A`和`/etc/init.d/ssh start`，然后再去查看服务器状态。


4. 安装完毕之后，输入`ifconfig`查看虚拟机`ip`
![](images/setup_init_ubuntu_net.png)
5. 然后就可以在本地终端使用`ssh`命令连接虚拟机服务器。
   `ssh <用户名>@<上图操作中的ip地址>`
![](images/setup_ssh_connection.png)
6. 安装`vscode`:[vscode下载地址](https://code.visualstudio.com/download)
7. 安装`ssh remote`插件
![](images/setup_vscode_install_ssh.png)
8. 配置插件,添加刚刚的虚拟机
![](images/setup_vscode_new_remote.png)
9. 输入连接虚拟机的命令，如下图示例
![](images/setup_vscode_new_remote2.png)
10. 打开一个新的远程文件夹：
    ![](images/setup_vscode_new_file.png)
11. 选择一个文件夹作为开发文件夹，这里我选择`/home/pingxu/Public/`
![](images/setup_vscode_workspcae.png)
进入新的文件夹之后，输入完密码，会问是否信任当前目录什么的，选择`yes`就行了，自此，现在虚拟机安装完毕，工作目录是`/home/pingxu/Public/`。
#### 3. 安装必要软件
`vscode`中`crtl`+`~`打开终端，直接把下面命令拷贝过去
```
sudo apt-get update && sudo apt-get install -y locales apt-utils && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
sudo apt-get update \
    && sudo apt-get install -y build-essential gdb cmake git wget flex texinfo libreadline-dev diffutils bison \
    && sudo apt-get install -y clang-format vim
sudo apt-get -y install clangd lldb
```
#### 4. 安装miniob
```
# 从github克隆项目会遇到网络问题，配置网络代理命令
git config --global http.proxy http://<代理ip>:<代理端口>
git config --global https.proxy https://<代理ip>:<代理端口>
```
在`Public`目录下：
```
git clone https://github.com/oceanbase/miniob 
cd miniob
THIRD_PARTY_INSTALL_PREFIX=/usr/local bash build.sh init 
```
完毕之后，我们用`vscode`打开`miniob`，作为新的工作目录。
![](images/open_miniob_as_workspace.png)
![](images/open_miniob2.png)
#### 5. vscode插件配置
1. 首先安装插件`clangd`和`C/C++ Debug`。
安装`clangd`,
![](images/setup-install-clangd.png)
同样的方式安装`C/C++ Debug`。
![](images/setup-install-cppdbg.png)
1. 修改好代码之后，`Ctrl+Shift+B`构建项目，构建完毕后有一个`build_debug`的文件夹，存放编译后的可执行文件。
2. 使用`clangd`作为语言服务器， 构建完毕后，将`build_debug`中的`compile_commands.json`文件复制到`miniob`目录中，随便打开一个cpp文件，就可以看到`clangd`开始工作。
![](images/setup-config-clangd.png)
#### 6. debug简单教程
1. 用`F5`进行调试，关于如何`vscode`如何调试，可以参考相关的资料:[cpp-debug](https://code.visualstudio.com/docs/cpp/cpp-debug)。修改`launch.json`文件中`program`和`args`来调试不同的可执行文件。
![](images/setup-launch-config.png)
![](images/setup-debug.png)

### 方式三： Docker中开发代码
**vscode也运行在Docker中，此方法最方便,可能因为网络原因配置失败，需要自己解决代理配置的问题**

1. 先安装docker:[docker下载](https://docs.docker.com/desktop/setup/install/windows-install/)
*注意*：安装docker遇到了问题希望大家动手上网解决。

1. 在任意文件夹中创建一个`Dockerfile`，并把下面内容粘贴进去。
```
# 注意这一步，可能会因为网络问题失败，需要走代理软件，然后在终端中使用命令 docker pull linuxserver/code-server:latest。
FROM linuxserver/code-server:latest

RUN apt-get  update && apt-get  install -y net-tools vim git curl wget iputils-ping gdb

# ENV LANG=en_US.UTF-8
# locale
RUN apt-get update && apt-get install -y locales apt-utils && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG en_US.utf8

# dev tools
RUN apt-get update \
    && apt-get install -y build-essential gdb cmake git wget flex texinfo libreadline-dev diffutils bison \
    && apt-get install -y clang-format vim

# install openssh
RUN apt-get install -y openssh-server

# install clangd
RUN apt-get install -y clangd
# install lldb
RUN apt-get install -y lldb

# 更新 code-server 所在的容器中的 PATH 环境变量
ENV PATH="/usr/bin/gcc:${PATH}"

# 更新 code-server 所在的容器中的编译器版本
RUN gcc --version

# init miniob dependencies
# 这一步也有可能会因为网络问题失败，需要走代理软件。
# 本地有代理软件可以将下面两个命令解除注释
# RUN git config --global http.proxy http://<本地ip>:<代理软件开放的端口>
# RUN git config --global https.proxy https://<本地ip>:<代理软件开放的端口>
RUN git clone https://github.com/oceanbase/miniob /tmp/miniob \
    && cd /tmp/miniob \
    && THIRD_PARTY_INSTALL_PREFIX=/usr/local bash build.sh init \
    && rm -rf /tmp/miniob

RUN mkdir /var/run/sshd

# install zsh and on-my-zsh
RUN apt-get install -y zsh \
    && git clone https://gitee.com/mirrors/ohmyzsh.git ~/.oh-my-zsh \
    && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc \
    && sed -i "s/robbyrussell/bira/" ~/.zshrc \
    && usermod --shell /bin/zsh root \
    && echo "export LD_LIBRARY_PATH=/usr/local/lib64:\$LD_LIBRARY_PATH" >> ~/.zshrc

RUN mkdir -p /root/docker/bin && touch /etc/.firstrun

# copy starter scripts
# COPY bin/* /root/docker/bin/

# RUN chmod +x /root/docker/bin/*
```
然后将该文件夹在终端中打开，输入下面命令：
`docker build -t miniob:latest .`

打开`docker`软件
![](images/run_docker.png)
设置容器名和端口的映射
![](images/config_docker.png)
然后就可以在`Container`中看到`Miniob-dev`这个容器在运行。
在浏览器中输入`localhost:9962`,就可以访问`vscode`，后续步骤按照上面[]()中的步骤即可[安装mibiob](#安装miniob) [插件配置和debug简单教程](#插件配置和debug简单教程)。


## 实验提交

### 准备工作
1. 实验课作业使用提交平台进行提交，提交平台：[OceanBase训练营](https://open.oceanbase.com/train?questionId=200001)
需要注册平台（不需要实名）


2. 训练营提交代码需要gitee/github的仓库，并且大家创建的仓库必须是设置为*私有仓库*（否则会提交失败），具体教程可以参考下面链接：

[MiniOB GitHub 在训练营中的使用说明](https://oceanbase.github.io/miniob/game/github-introduction/)

[MiniOB Gitee 在训练营中的使用说明](https://oceanbase.github.io/miniob/game/gitee-instructions/)

### 提交过程

点击立即提测，填入对应的仓库代码地址，`Commit id`和分支，就可以进行测试，并且可以看到测试结果。

![](images/commit_lab1.png)

代码提交后，查看提测结果

![](images/commit_lab2.png)

可以在平台上查看失败案例
先点击失败
![](images/commit_lab4.png)
查看失败原因和失败的案例
![](images/commit_lab3.png)


## 注意事项
1. 共有5个实验，实验文档会持续更新。
2. MiniOB的资料：[MiniOB](https://oceanbase.github.io/miniob/)
## 可能会出现的问题
出现了问题优先上网搜索，查询资料然后再可以问问助教，同学和老师。

### Github的网络没有问题，但是push代码失败
1. 使用令牌来做Github的身份验证，详细参考：
[管理个人访问令牌](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

2. 按照上述步骤获得`token`之后，并且按照上文步骤中，导入`miniob`仓库在自己的`github`账号中。

3. 在终端中输入下面命令。
```
 # 先删除远端origin对应的仓库的本地记录。
 git remote rm origin
 # 添加自己的仓库，按照下面格式，其中 <token>替换成自己的token, <username>替换成自己的用户名。
 git remote add origin https://<token>@github.com/<username>/miniob.git
```
4. 然后就可以进行正常的推送和拉取操作了。
