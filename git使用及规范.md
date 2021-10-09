## 1 常用命令

```c++
git config --global username xxx
git config --global useremail xxx

git init
git status
    
git add xxx
git add -u          					 //modified and deleted
git add .          						 //new and modified
git add -A         						 //all
    
    
git commit -m "message" (filename)xxx   ???
git commit --amend -m "修改的内容"         //修改commit注释
    
git push -u (origin)仓库名称 (master)分支  //-u只有第一次添加
git pull
    
git relog
git log
 
git reset --hard (版本号)xxx  			   //--head同时操作暂存区和工作区
git reset HEAD^              			 //上一版本
    
git branch 分支名						   //创建分支
git branch -v						     //查看分支
git checkout 分支名					   //切换分支
git merge 分支名					   	   //把指定分支合并到当前分支
    
git remote -v                            //查看所有远程仓库别名
git remote add 别名 远程地址               //起别名
git clone                                //克隆到本地
git push 别名 分支                        //推送本地分支上的内容克隆到本地???
git pull 别名 分支                        //远程拉取并与本地分支合并
```

关于合并的冲突：合并时，**同文件同位置有不同的修改**



















