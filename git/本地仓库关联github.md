### 

### ...或者在命令行窗口创建一个新的仓库

```
echo "# demo-data_transfer" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/lyPersonalGit/demo-data_transfer.git
git push -u origin main
```



### ...或者从命令行窗口推送一个已有的仓库

```
git remote add origin https://github.com/lyPersonalGit/demo-data_transfer.git
git branch -M main
git push -u origin main
```