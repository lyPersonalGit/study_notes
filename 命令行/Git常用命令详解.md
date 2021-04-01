 

| **命令**                                  | 描述                           |
| ----------------------------------------- | ------------------------------ |
| **git init**                              | 初始化一个git版本库            |
| **git add** <file>                        | 将工作区的某个文件添加到暂存区 |
| **git commit -m**  <message>              | 将暂存区提交，生成一个版本     |
| **git status**                            | 查看当前目录下所有文件的状态   |
| **git log**                               | 查看当前版本库历史记录         |
| **git reflog**                            | 查看当前版本库操作记录         |
| **git reset --hard**  <version>           | 回退到指定版本                 |
| **git** **checkout** -- <file>            | 撤销工作区某个文件的修改       |
| **git reset HEAD** <file>                 | 撤销暂存区某个文件的修改       |
| **git remote add origin** <地址>/名称.git | 将本地版本库创建到远程版本库   |
| **git clone** <地址>                      | 将远程版本库克隆到本地         |
| **git push** <地址> <branch>              | 从远程版本库拉取到本地版本库   |
| **git pull** <地址> <branch>              | 将本地版本库推送到远程版本库   |

 

使用Git来管理本地文件资源 

**git init**

 

从远程库中克隆项目到本地

**git** **clone** <url>

 

执行以上的命令后，目录下出现一个.git文件夹，这代表改目录已被git管理起来。

 

该目录作为你的工作区使用。

 

在该目录下修改的某个文件会存放在工作区，还不能进入git的管理，需要执行

**git add** <filename>

 

以添加到暂存区，也可以执行

**git** **add .**

 

将工作区的所有文件添加到暂存区

之后就可以执行提交命令，将暂存区的改动生成为一个版本

**git commit -m** <message>

 

查看当前目录下版本记录

**git log**

 

回退到指定版本

**git reset --hard** <version>