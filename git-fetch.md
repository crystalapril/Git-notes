# git fetch or pull

### git fetch or pull

    1 从远程库 git fetch/pull 可能会遇到一些问题：    
    >>git fetch cfx_backup_ssh master    
    From github.com:crystalapril/cfx_backup
     * branch            master     -> FETCH_HEAD
       413bab6..d3e6f7e  master     -> cfx_backup_ssh/master    
    >>git branch -a    # 查看现有分支，发现出现了remotes/cfx_backup_ssh/master，这个是远程分支在本地的一个copy，远程跟踪分支
    * master
      remotes/cfx_backup_ssh/master
    >>git checkout remotes/cfx_backup_ssh/master   
    error: The following untracked working tree files would be overwritten by checkout:
            backup_address.py
            backup_fun.py
    Please move or remove them before you switch branches.
    Aborting       
    # 切换到该远程分支时出现失败，原因是因为本地有未add，commit的文件，提示要求先删除那些文件
    >>git log --oneline
    413bab6 (HEAD -> master) modify backup.py in 20211108 with caculate time: @make_elapsed

    2 解决办法：先把本地文件copy到另外的文件夹，再使用 git clean
    >>git clean -d -f   # 强制清除未被追踪的文件，d是delete，f是force    
    >>git checkout remotes/cfx_backup_ssh/master  # 再切换    
    Note: switching to 'remotes/cfx_backup_ssh/master'.
    >>git log --oneline
    d3e6f7e (HEAD, cfx_backup_ssh/master) modify backup_address in 20211106 
    >>git checkout -f remotes/cfx_backup_ssh/master   # 也可以切换的同时，强行清除，一样会使得未追踪文件被删除

    3 合并分支 git merge
    >>git checkout master     # 切换到master
    Previous HEAD position was d3e6f7e modify backup_address in 20211106 
    Switched to branch 'master'
    >>git branch -a
    * master
      remotes/cfx_backup_ssh/master
    >>git merge remotes/cfx_backup_ssh/master    # 在master上合并远程分支
    Updating 413bab6..d3e6f7e
    Fast-forward
    >>git log --oneline             # 发现本地master已经有了远程分支上的最新记录
    d3e6f7e (HEAD -> master, cfx_backup_ssh/master) modify backup_address in 20211106

    4 删除本地的远程跟踪分支 git branch 
    - 如果想要删除本地的远程跟踪分支可以使用 git branch，但是一般不推荐这么做，它有助于跟踪对远程主分支的更改
    - 这个只是删除本地的cfx_backup_ssh/master ，不会影响github上的远程分支
    
    >>git branch --remotes --delete cfx_backup_ssh/master  # git branch -r -d cfx_backup_ssh/master 或许也可以
    Deleted remote-tracking branch cfx_backup_ssh/master (was d3e6f7e).    
    >>git branch -a
    * master
    
    '''
    以下方法可能也行，待验证：
        git branch -d remotes/origin/master
        git remote prune origin
        git fetch -p
        git fetch origin master 
    '''
    
    5 删除远程分支
    典型的情况是，如果你有一个远程特性分支，你已经完成了，你从远程删除它
    >>git push origin --delete feature_branch # 或 git push origin :feature_branch
    
    然后更新的本地存储库引用，以“修剪”过时的远程跟踪分支
    >>git fetch origin --prune   # 或 git fetch origin -p    
        
    当然，想要删除远程库的 master 也是可以的
    >>git push origin --delete master  # 或 git push origin :master

    
    

