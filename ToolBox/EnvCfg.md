# 环境配置

## 1. ubuntu 安装初始环境配置

1. 换源 Ubuntu20.04源,  `\etc\apt\sources.list`

   ```shell
   #阿里源
   deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
   
   #中科大源
   deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
   
   #163源
   deb http://mirrors.163.com/ubuntu/ focal main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ focal-security main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ focal-updates main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ focal-proposed main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ focal-backports main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ focal main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ focal-security main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ focal-updates main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ focal-proposed main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ focal-backports main restricted universe multiverse
   
   ```

   Ubuntu18

   ```shell
   ##中科大源
   
   deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   
   #阿里源
   deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
   
   #163
   deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
   deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
   deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
   
   #清华
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   
   
   
   ```

   

2. 基础软件安装

   ```shell
   $sudo apt-get update		#更新源
   $sudo apt-get upgrade		#更新软件
   $sudo apt-get install openssh-server	#安装ssh
   $sudo apt install build-essential 		#安装gcc，g++
   ```

3. 其他软件安装



环境变量恢复

```shell
$ export PATH=/usr/bin:bin
```



## 2. Debian  tab命令补全

Debian 默认不带命令补全的插件

* 安装补全插件

  ```shell 
  sudo apt-get install bash-completion
  ```

* 在 `/etc/profile` 或 `~/.bash_profile` 添加以下配置

  ```shell
  if [ -f /etc/bash_completion ]; then
  ./etc/bash_completion
  fi
  ```

  



## 3. Unix环境高级编程（第三版）环境配置

系统： wsl2 ubuntu18

1. [下载源码](http://www.apuebook.com/src.3e.tar.gz)

2. 解压文件

   ```shell
   $ tar -zxvf *.tar.gz
   $ cd apue.3e/
   ```

3. make

   ```shell
   $ sudo apt-get install libbsd-dev   #先安装库
   $ make
   ```

4. 将编译的库复制到系统库

   ```shell
   $ sudo cp ./include/apue.h /usr/include/
   $ sudo cp ./lib/libapue.a /usr/local/lib/
   ```

5. 示例

   ```c
   #ls.c
   #include "apue.h"
   #include <dirent.h>
   
   int main(int argc, char *argv[])
   {
       DIR *dp;
       struct dirent *dirp;
   
       if (argc != 2)
       {
           err_quit("usage: ls directory_name");
       }
       if((dp = opendir(argv[1])) == NULL)
           err_sys("can't open %s", argv[1]);
   
       while ((dirp = readdir(dp)) != NULL)
       {
           printf("%s\n",dirp->d_name);
       }
   
       closedir(dp);
   
       exit(0); 
   }
   ```

   ```shell
   gcc ls.c -o ls -lapue	#编译时链接apue库
   ```

   

## 4. Jupyter设置主题

安装主题

```shell
$ pip install jupyterthemes
```

查看可用主题

```shell
$ jt -l
$ jt -r  #恢复默认
```

更换主题，[详细参数设置](https://github.com/dunovank/jupyter-themes)

```shell
jt -t grade3 -f source -fs 12 -cellw 90% -ofs 11 -dfs 11 -T -N
```





## 5. git报错

在clone或push时出现以下错误

![image-20210718095331515](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210718095331515.png)

原因：一般是这是因为服务器的SSL证书没有经过第三方机构的签署，所以才报错

解决方式：解除ssl验证

```shell
$ git config --global http.sslVerify "false"
```



## 6. Anaconda

### 安装





### 1. 更新

```shell
$conda update --all  #更新所有的库

pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 
```



### 2. 虚拟环境创建

```shell
$ conda create -n flask-env python=3.8 #创建flask-env的环境
$ conda activate flask-env	#激活进入环境
$ pip install packge-name	#在环境中安装包
$ conda deactivate 			#退出虚拟环境
$ conda remove --name $your_env_name  $package_name   #删除环境
conda remove --name d2l --all
```



### 3. 在Ubumtu中使用Anacoda

- 安装

  ```shell
  # Path是自定义安装路径
  $ bash Anaconda3-2021.11-Linux-x86_64.sh -p [Path] -u 
  ```



- 配置

  ```shell
  #conda不作为默认的终端，即命令行前面不会有base
  $ conda config --set auto_activate_base false
  #在终端中进入conda
  $ conda activate
  #退出终端
  $ conda deactivate
  ```

  

```shell
$ pip install d2l -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```



## 7. vscode输出框中文乱码

### 1. Python代码

添加环境变量

变量名`PYTHONIOENCODING`，值设置为`UTF8`



### 2. java代码

在coderunner的插件配置文件 `settjing.json` 中添加如下配置

```json
"code-runner.executorMap": {
"java": 
"cd $dir && javac -encoding utf-8  $fileName && java -Dfile.encoding=UTF-8 $fileNameWithoutExt"
    }
```



## 8. JetBrains软件启动

JetBrains系列软件启动时出现卡在启动界面，log出现下面的错误

```shell
2021-10-12 16:29:18,493 [   2921]  ERROR - llij.ide.plugins.PluginManager - java.net.BindException: Address already in use: bind 
java.util.concurrent.CompletionException: java.net.BindException: Address already in use: bind
	at java.base/java.util.concurrent.CompletableFuture.encodeThrowable(CompletableFuture.java:314)
	at java.base/java.util.concurrent.CompletableFuture.completeThrowable(CompletableFuture.java:319)
	at java.base/java.util.concurrent.CompletableFuture$AsyncSupply.run(CompletableFuture.java:1702)
	at java.base/java.util.concurrent.CompletableFuture$AsyncSupply.exec(CompletableFuture.java:1692)
	at java.base/java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:290)
	at java.base/java.util.concurrent.ForkJoinPool$WorkQueue.topLevelExec(ForkJoinPool.java:1020)
	at java.base/java.util.concurrent.ForkJoinPool.scan(ForkJoinPool.java:1656)
	at java.base/java.util.concurrent.ForkJoinPool.runWorker(ForkJoinPool.java:1594)
	at java.base/java.util.concurrent.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:183)
Caused by: java.net.BindException: Address already in use: bind
	at java.base/sun.nio.ch.Net.bind0(Native Method)
	at java.base/sun.nio.ch.Net.bind(Net.java:455)
	at java.base/sun.nio.ch.Net.bind(Net.java:447)
	at java.base/sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:227)
	at io.netty.channel.socket.nio.NioServerSocketChannel.doBind(NioServerSocketChannel.java:134)
	at io.netty.channel.AbstractChannel$AbstractUnsafe.bind(AbstractChannel.java:562)
	at io.netty.channel.DefaultChannelPipeline$HeadContext.bind(DefaultChannelPipeline.java:1334)
	at io.netty.channel.AbstractChannelHandlerContext.invokeBind(AbstractChannelHandlerContext.java:506)
	at io.netty.channel.AbstractChannelHandlerContext.bind(AbstractChannelHandlerContext.java:491)
	at io.netty.channel.DefaultChannelPipeline.bind(DefaultChannelPipeline.java:973)
	at io.netty.channel.AbstractChannel.bind(AbstractChannel.java:260)
	at io.netty.bootstrap.AbstractBootstrap$2.run(AbstractBootstrap.java:356)
	at io.netty.util.concurrent.AbstractEventExecutor.safeExecute(AbstractEventExecutor.java:164)
	at io.netty.util.concurrent.SingleThreadEventExecutor.runAllTasks(SingleThreadEventExecutor.java:472)
	at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:500)
	at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:989)
	at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	at java.base/java.lang.Thread.run(Thread.java:829)
2021-10-12 16:29:18,498 [   2926]  ERROR - llij.ide.plugins.PluginManager - IntelliJ IDEA 2021.2.2  Build #IU-212.5284.40 
2021-10-12 16:29:18,501 [   2929]  ERROR - llij.ide.plugins.PluginManager - JDK: 11.0.12; VM: OpenJDK 64-Bit Server VM; Vendor: JetBrains s.r.o. 
2021-10-12 16:29:18,501 [   2929]  ERROR - llij.ide.plugins.PluginManager - OS: Windows 10 
2021-10-12 16:29:18,502 [   2930]  ERROR - llij.ide.plugins.PluginManager - Last Action:  
```

这个错误发生的环境：win10 21H1  JetBrains 2021.2.2

解决方法，以管理员运行CMD,一次输入以下命令

```powershell
netsh int ipv4 set dynamicport tcp start=49152 num=16383
netsh int ipv4 set dynamicport udp start=49152 num=16383


#If the above doesn't help, please try these commands instead:
net stop winnat
net stop winnat
net start winnat
```



链接：[Revise IDE folders locking mechanism (don't fail startup if all ports in range are taken) : IDEA-238995 (jetbrains.com)](https://youtrack.jetbrains.com/issue/IDEA-238995?_ga=2.55760301.747728973.1634010900-527688314.1632555664)







## 9. 腾讯云服务器初始化配置

```shell
$sudo passwd root
$sudo vi /etc/ssh/sshd_config
```



找到 `#Authentication`，将 `PermitRootLogin` 参数修改为 `yes`。如果 `PermitRootLogin` 参数被注释，请去掉首行的注释符号（`#`）

![img](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/e3d9dd2e362616a8bc25f8793a403cba.png)

找到 `#Authentication`，将 `PasswordAuthentication` 参数修改为 yes。如下图所示：

> 若 `sshd_config` 配置文件中无此配置项，则添加 `PasswordAuthentication yes` 项即可。

![29ebeedf5f50279aa67974784d6140af](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/29ebeedf5f50279aa67974784d6140af.png)



添加用户：

```shell
#删除用户
$deluser --remove-home <你想要删除的用户名字>
```



```shell
$useradd -r -m -s /bin/bash wwx
```

-r：建立系统账号

-m：自动建立用户的登入目录

-s：指定用户登入后所使用的shell

```shell
$passwd wwx
```

增加sudo权限（管理员权限）

```shell
$cd /etc
$chmod +w sudoers
$vim sudoers
```

```shell
wwx ALL=(ALL:ALL) ALL # 这个是新添加的成员

```





## 10. tmux

```shell
tmux new -s foo # 新建名称为 foo 的会话
tmux ls # 列出所有 tmux 会话
tmux a # 恢复至上一次的会话
tmux a -t foo # 恢复名称为 foo 的会话，会话默认名称为数字
tmux kill-session -t foo # 删除名称为 foo 的会话
tmux kill-server # 删除所有的会话
```





## 11. wsl安装桌面环境

参考：[WSL2 Ubuntu GUI 图形用户界面_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1LA411n7BK)

```shell
$sudo apt install xrdp
$sudo apt install xfce4 xfce4-goodies
$echo xfce4-session > ~/.xsession
#启动桌面环境
$ sudo /etc/init.d/xrdp start


```





## 12. Ubuntu添加新用户

```shell
#Step1：添加新用户
$useradd -r -m -s /bin/bash 用户名

#Step2:配置新用户密码
$passwd 用户名

#Step3：给新添加的用户增加ROOT权限
$vim /etc/sudoers
#然后添加：
#用户名 ALL=(ALL:ALL) ALL
#另外，如果直接用useradd添加用户的话，可能出现没有home下的文件夹，以及shell无法自动补全的显现。出现此问题只要修改/etc/passwd下的/bin/sh为/bin/bash即可。
```



## 13. Git 设置/取消代理

设置代理

```shell
git config --global http.proxy 127.0.0.1:41091
git config --global https.proxy 127.0.0.1:41091

git config --global http.proxy 'socks5://127.0.0.1:1020'
git config --global https.proxy 'socks5://127.0.0.1:1020'
```



取消代理

```shell
git config --global --unset http.proxy
git config --global --unset https.proxy
```



