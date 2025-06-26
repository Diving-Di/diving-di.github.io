---
title: Task1：Hello Move
categories:
 - move共学营
tags:
 - study
 - move
---

这是我参加move共学营的第一个任务。因为正好赶上烤柿粥，所以也是拖了很久才开始做。<br>

<!--more-->

### Github配置

根据指令 ` ssh-keygen -t rsa -C "你的邮箱"`，来设置密钥，这主要是方便GitHub仓库可以直接通过`ssh`连接，而不是每次都要输入密码。

![ssh装配图片](../images/task1-hello-move/ssh.png "ssh装配图片")

然后运行`cat /home/cindy/.ssh/id_rsa.pub`来查看公钥，并且复制到GitHub设置中的SSH Keys里面即可。

![SSH Keys设置](../images/task1-hello-move/ssh2.png "SSH Keys设置")

因为我已经配置过ssh了的，所以这上面显示两个SSH Keys。

然后前往letsmove的GitHub仓库：<https://github.com/move-cn>

将整个仓库fork下来之后，使用以下命令clone自己fork的这个仓库到本地。

```bash
git clone 你的这个仓库的ssh链接
```

出现以下结果就是clone成功了。

![clone-result](../images/task1-hello-move/git-clone.png "git clone的结果")

### SUI钱包

前往浏览器上的SUI钱包[下载链接](https://chromewebstore.google.com/detail/sui-wallet/opcgpfmipidbgpenhmajoajpbobppdil)下载钱包，然后打开钱包进行登录之后，就可以得到自己的钱包地址啦。

![SUI钱包创建](../images/task1-hello-move/qb.png "SUI钱包创建")

### SUI CLI安装

我选择使用Homebrew来安装SUI，所以要先安装Homebrew。

首先使用以下命令安装Homebrew的相关依赖：

```bash
sudo apt-get install build-essential procps curl file git
```

Homebrew需要有相对较新版本的 gcc 和 glibc，除此之外，还需要 安装Git、Curl 和 procps（用于监控系统进程）。

然后通过以下命令安装Homebrew：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

接着就可以使用`brew install sui`安装SUI。

用brew安装真的要贼久啊，反正我是这样的。安装成功之后，就可以输入`sui -V`来查看是否安装成功。我这里显示`sui 1.37.1-homebrew`，显然是已经安装好了。

### 编写HelloWorld

我是用WSL+Vscode的形式的，感觉这样比较好用且方便。

![wsl连接vscode](../images/task1-hello-move/WSL+vscode.png "Ubuntu连接Vscode")

点击左下角的蓝色小方块，然后在vscode上方的搜索窗口搜索WSL即可连接上Ubuntu。只不过这个配置还需要一些前期工作的，vscode需要先下载拓展WSL等等，感兴趣可以上网查一下。

接着在vscode里打开前面git clone 的letsmove仓库文件夹即可。然后按照task1的要求复制001这个文件夹，并把名字改成自己的**GitHub id**。

将文件夹里最外面的readme.txt里面的基本信息和个人简介填好。还有任务中的钱包截图等要放到images文件夹中。同时记得把打勾框勾起来。最简便的方法是在vscode中安装插件`Markdown Preview Enhanced`，这样就可以一边编辑一边查看了。而且打勾框可以直接在可视化界面点击即可，不需要再在框里面加上`x`，这样写出来的可能会显示为一个`● [x]`，虽然我也不知道为什么，可能是编辑器的关系。

修改完全之后`cd code`进入code文件夹，执行`sui move new task1`创建task1文件。然后打开新建文件中的**task1.move**。

![代码](../images/task1-hello-move/code.png "代码展示")

将string中的Github id改成自己的就行。图中我是下载了move插件的高亮的，看着会舒服很多。不过舒服是**有代价的**，因为这个插件，导致我后面一直报错，所以推荐**不要安装！！！**。报错请看后面——

![bug](../images/task1-hello-move/bug.png)

然后我就被这个bug硬控了一个下午。查了两个小时资料还是没有用，甚至我还把`/etc/resolv.conf`里的IP地址改成了谷歌服务器的地址还是没有用。尝试了一堆方法，甚至找到了一个有同样问题的人的[讨论区](https://github.com/MystenLabs/sui/issues/16906)。虽然我翻了这个讨论区好久都没找到解决方法。主要是感觉并不是网络的问题，我可以ping通[github.com](github.com)，SSH链接也是正常的，就很奇怪。然后在讨论群里问了一下之后，发现`curl google.com`这个命令会失败。可能真是网络问题，所以我又去搜了一下，配置了一下代理。

本来群里的好心人让我用下面命令配置：

```bash
export https_proxy=http://127.0.0.1:1080;
export http_proxy=http://127.0.0.1:1080;
export all_proxy=socks5://127.0.0.1:1080
```

但是我google之后发现这个已经不行了，要用下面这样的命令：

```bash
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
export https_proxy="http://${hostip}:1080"
export http_proxy="http://${hostip}:1080"
```

后面的端口号要改成自己windows代理上的端口号。还要把`/etc/resolv.conf`里的IP改回来。

然后就会变成另外一种报错，如下：

![new-bug](../images/task1-hello-move/new-bug.png "新的bug")

当然这些都没有什么鸟用……只要把最开始的**高亮插件**卸载即可完美解决。

![结果](../images/task1-hello-move/scan.png "最终结果图")

这是最终的结果图！