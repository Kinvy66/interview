## Git的基本使用

#### 基本命令

```bash
git init      #初始化当前文件夹作为git库
git clone     #复制远程的库
#版本管理
git add        #将工作区的文件放入暂存区，后面加文件名或文件
git commit    #将暂存区的文件提交到库中，会弹出编辑器提示输入提交信息，适用于提交信息比较多的情况
git commit -m '提交信息'  #提交文件到库中，并注明提价信息，适用提交信息比较简短的情况
git rm        #删除文件
git rm --cached <file>... #从暂存区删除文件
#查看信息
git help    #帮助
git    log        #查看日志
git log -n    #查看最近n条的日志
git log --pretty=oneline    #一行简略的显示日志
git    diff    #比较不同
git status    #查看文件修改状态
git config --list    #查看配置信息
#远程协作
git pull
git push
#git配置
git config --global        #使用全局配置文件
git config --system        #使用系统的配置文件
git config --local        #使用当前仓库的配置文件
git config --global user.name 'naem'      #配置全局用户名
git config --global user.email 'email'  #配置全局邮箱


#删除文件
git rm filename        #删除工作区的文件，
                    #完成了两件事，1.删除一个文件 2.将删除的文件纳入到暂存区
#从已提交的文件中恢复删除的文件
git reset HEAD filename     #将待删除的文件从暂存区恢复到工作区
git checkout -- filename #把之前的修改丢弃，用 rm 删除的文件只要这个命令就可以恢复

#文件重命名
git mv oldname newname  #执行这个命令后文件就会放入到暂存区，和mv的区别与 git rm和rm的区别一样


#修改上次提交的消息
git commit --amend -m '修改的消息内容'




#.gitignore文件
#在目录下添加一个.gitignore文件
#文件内容就是需要忽略文件的文件名
#git上传是就会忽略这些文件
#文件支持通配符和正则表达式
```

#### 分支相关命令

```bash
git branch        #查看当前所有的分支
git branch new_branch    #创建新的分支（new_branch）
                        #创建时会把当前分支的文件复制到新分支
git checkout new_branch        #切换到new_branc分支
git checkout -            #切换到之前的分支

git branc -m old_branch new_branch  #更改分支的名字

#在新的分支上创建新文件，然后提交到git中
#在切换到分支中是不会有刚刚创建的文件的

git branch -d new_branch    #删除new_branch分支，没有合并分支无法删除
                            #使用下面的命令可以强制删除
git branch -D new_branch


git checkout -b new_branch    #创建new_branch分支，并切换到新分支
git merge new_branch  #将new_branch分支的修改内容合并到当前所在的分支

git branch -v    #查看当前分支最近一次提交的信息
```

#### git远程操作

```bash
push     推送
pull    拉取，同时执行合并merge
pull == fetch + merge



#推送到GitHub

#git remote 远程
#origin,表示用origin代替后面的地址
git remote add origin https://github.com/Kinvy66/NanoClock.git

git push -u origin master     #将本地的推送到远程，同时本地的master和远程的关联 

#执行上述操作后，再次提交
git push

git remote show    #列出远程仓库的别名
git remote show    origin #列出origin的详细信息



#ssh
which ssh-keygen #查看keygen的目录
ssh-keygen        #生成keygen


#执行步骤
git remote add origin ssh地址
git push -u origin master
ssh-keygen
#将公钥配置到GitHub上
git push -u origin master

#拉起本地不存的远程分支并创建一个本地分支
git checkout -b 本地分支名 origin/远程分支名

```

#### 版本回退

**方法一：git reset**

1. 查看提交历史，确定回退版本
   
   ```bash
   git log
   ```

2. 回退到某个版本
   
   ```bash
   git reset --hard [版本号，只需前几个字符就可以]
   ```

3. 强制提交到远程
   
   ```bash
   git push -f -u origin master
   ```

注意：回退之后强制提交到远程，回退版本之后的提交历史会没有的。

**方法二：git revert**

git revert 撤销 某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销作为一次最新的提交

```bash
 git revert HEAD                #撤销前一次 commit
 git revert HEAD^               #撤销前前一次 commit
 git revert commit （比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）#撤销指定的版本，撤销也会作为一次提交进行保存。
```

git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去，版本会递增，不影响之前提交的内容



**丢弃所有更改**

没有 `add`

```shell
$ git checkout .
```





#### 其他

使用 `git add` 后出现下面警告

> warning: LF will be replaced by CRLF in text.txt.
>  The file will have its original line endings in your working directory
>  警告：在xxx.xx文件中LF将被CRLF替换。
> 在你的工作区（working directory）里，这个文件将会保持它原本的换行符。（line ending:行尾，换行）

CR：Carriage Return 回车\r
LF：Line Feed  换行 \n

CRLF：Carriage Return Line Feed 回车换行\r\n

出现原因：
这是因为在Windows中的换行符为CRLF，而在Linux中的换行符为LF。在git创建的项目中换行符为LF，而gits是linux环境，Git自作聪明的“换行符自动转换”功能会自动进行转换，然后系统会提示LF将被转换为CRLF。
解决的办法很简单，禁止git的自动转换即可。

#### 生成密钥

1. 查看是否有密钥：
   
   ```shell
   $ cd ~/.ssh        #没有的话，不会有这个文件
   ```

2. 生成密钥
   
   ```shell
   $ ssh-keygen -t rsa
   ```

3. 最后得到了两个文件：id_rsa和id_rsa.pub
   
   ```shell
   $ cat ~/.ssh/id_rsa.pub
   ```
   
   将这个文件内容复制到GitHub



#### 配置代理

```shell

git config --global http.proxy http://127.0.0.1:1080

git config --global https.proxy http://127.0.0.1:1080

git config --global --unset http.proxy

git config --global --unset https.proxy
```



删除跟踪

```bash
git rm --cached filename
```





```cmake
cmake_minimum_required(VERSION 3.16)
project(LeetCode)

set(CMAKE_CXX_STANDARD 11)

#add_executable(LeetCode main.cpp 001TwoSum.cpp)
# 遍历项目根目录下所有的 .cpp 文件
file (GLOB files *.cpp)
foreach (file ${files})
    string(REGEX REPLACE ".+/(.+)\\..*" "\\1" exe ${file})
    add_executable (${exe} ${file})
    message (\ \ \ \ --\ src/${exe}.cpp\ will\ be\ compiled\ to\ bin/${exe})
endforeach ()
```

