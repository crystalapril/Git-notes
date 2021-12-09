# git push origin  --force

    1 如果在本地库操作失误，并且已经推送到了远程库，想要删除本地库和远程库的commit记录要怎么处理
    
    - 通过找到想要退回到的那个commit_id
    >>git log --oneline
    - 将本地代码变成某个提交记录时刻的代码
    >>git reset --hard commit_id
    - 推送到服务器，一定要加 --force 参数 "master":对应的分支即可
    >>git push origin HEAD:master --force
    
