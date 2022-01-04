### Git全局设置

```
git config --global user.name "lyPersonalGitee"
git config --global user.email "qianse163ly@163.com"
```



### ...或者在命令行窗口创建一个新的仓库

```
mkdir data_transfer
cd data_transfer
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/lyPersonalGitee/data_transfer.git
git push -u origin master
```



### ...或者从命令行窗口推送一个已有的仓库

```
cd existing_git_repo
git remote add origin https://gitee.com/lyPersonalGitee/data_transfer.git
git branch -M master
git push -u origin master
```