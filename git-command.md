# git command （git命令行操作）

### git 简介

    1 git的优势
    - 大部分操作在本地完成，不需要联网
    - 完整性保证
    - 尽可能添加数据，而不是删除或修改数据
    - 分支操作快捷流畅
    - 与linux命令全面兼容
    
    2 git结构
             git commit    git add
       本地库 <---- 暂存区  <---- 工作区
    （历史版本）  （临时储存）   （写代码）
    
    3 git和代码托管中心
    局域网环境下：
    - gitlab 服务器
    外网环境下：
    - github
    - 码云

### git命令行操作

    1 本地库操作
    
    1.1 本地库初始化
    
    1.1.1 git init
    >>git init    
    >>ll -a   # 必须要用 -a, 才能显示隐藏文件
    total 0
    drwxrwxrwx 1 root root 0 Nov 15 09:54 .
    drwxrwxrwx 1 root root 0 Nov 15 09:54 ..
    drwxrwxrwx 1 root root 0 Nov 15 15:48 .git
    >>ls -l | less  # 可以用 less 翻页查看    
    >>ll .git/
    total 6
    -rwxrwxrwx 1 root root   23 Nov 15 15:51 HEAD
    -rwxrwxrwx 1 root root  130 Nov 15 15:51 config
    -rwxrwxrwx 1 root root   73 Nov 15 15:51 description
    drwxrwxrwx 1 root root 4096 Nov 15 15:51 hooks
    drwxrwxrwx 1 root root    0 Nov 15 15:51 info
    drwxrwxrwx 1 root root    0 Nov 15 15:51 objects
    drwxrwxrwx 1 root root    0 Nov 15 15:51 refs
    
    注意：.git目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改
    
    1.1.2 设置签名
    形式：
        用户名： april
        email 地址：aprilzhangpku@qq.com
    作用：区分不同开发人员的身份
    辨析：这里设置的签名和登录远程库（代码托管中心）的账号、密码没有任何关系
    命令：
        项目级别/仓库级别：仅在当前本地库范围内有效
        >>git config user.name april
        >>git config user.email april@qq.com        
        >>cat .git/config        # 信息保存位置： ./.git/config 文件
        [core]
                repositoryformatversion = 0
                filemode = false
                bare = false
                logallrefupdates = true
                symlinks = false
                ignorecase = true
        [user]
                name = crystal
                email = aprilzhangpku@qq.com
                
        系统用户级别：登录当前操作系统的用户范围
        >>git config --global user.name april
        >>git config --global user.email april@qq.com
        >>cd ~   
        >>pwd
        /home/crystal
        >>cat .gitconfig    # 信息保存位置：~/.gitconfig 文件        
        [user]
                name = crystalapril
                email = aprilzhangpku@qq.com
                     
        级别优先级：
        - 就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名
        - 如果只有系统用户级别的签名，就以系统用户级别的签名为准
        - 二者都没有不允许
    
    
    1.2 基本操作
    
    1.2.1 状态查看操作   
    >>git status
    On branch master
    No commits yet
    nothing to commit (create/copy files and use "git add" to track)
    
    1.2.2 添加操作
    git add [file name]   # 将工作区的“新建/修改”添加到暂存区    
    
    1.2.3 提交操作
    git commit -m "commit message" [file name]    # 将暂存区的内容提交到本地库    
    
    1.2.4 查看历史记录操作
    1.2.4.1 git log
            >>git log        # 多屏显示控制方式：空格向下翻页，b 向上翻页 ， q 退出    
            commit 48117dc87dcffd9332bf57ebe1c5c3c62eaf3a68 (HEAD -> master)
            Author: crystalapril <aprilzhangpku@qq.com>
            Date:   Tue Nov 16 17:50:06 2021 +0800

                commit backup.py rectified in 20211004,amount increased of linear trend,eg. [2 4 6 8]

            commit f17479c1926d83fa1019fa3f0eda0a86afa73c3f
            Author: crystalapril <aprilzhangpku@qq.com>
            Date:   Tue Nov 16 17:44:02 2021 +0800

                commit backup_address created in 20210926
    
            >>git log --pretty=oneline            
            48117dc87dcffd9332bf57ebe1c5c3c62eaf3a68 (HEAD -> master) commit backup.py rectified in 20211004
                    ,amount increased of linear trend,eg. [2 4 6 8]
            f17479c1926d83fa1019fa3f0eda0a86afa73c3f commit backup_address created in 20210926
    
            >>git log --oneline   # 是缩短的简写，包括hash码也是前7位
            48117dc (HEAD -> master) commit backup.py rectified in 20211004,amount increased of linear trend,eg. [2 4 6 8]
            f17479c commit backup_address created in 20210926
    
    1.2.4.2 git reflog
            >>git reflog            
            1644b18 (HEAD -> master) HEAD@{0}: commit: commit the 2nd version of backup.py which amount=[2,4,6,8]
            67416b8 HEAD@{1}: commit: commit the 1st version of backup.py,which amount is fixed,eg.amount=5
            48117dc HEAD@{2}: commit: commit backup.py rectified in 20211004,amount increased of linear trend,eg. [2 4 6 8]
            # HEAD@{移动到当前版本需要多少步}   
    
    1.2.4.3 head 前进后退
            - 本质
              指针在前后索引上的移动  
              
            - 基于索引值操作[推荐] 
            git reset --hard [局部索引值]           
            eg.
            >>git reset --hard 67416b8
            HEAD is now at 67416b8 commit the 1st version of backup.py,which amount is fixed,eg.amount=5
            >>git reflog
            67416b8 (HEAD -> master) HEAD@{0}: reset: moving to 67416b8
            1644b18 HEAD@{1}: commit: commit the 2nd version of backup.py which amount=[2,4,6,8],increased by linear
            67416b8 (HEAD -> master) HEAD@{2}: commit: commit the 1st version of backup.py,which amount is fixed,eg.amount=5
            >>cat backup.py   # 就可以看到67416b8 这个节点保存的 backup.py 的内容   
            
            - 使用^符号
            git reset --hard HEAD^  # 过河卒子，只能后退，往回找，不能前进
            eg.
            >>git reset --hard HEAD^            
            HEAD is now at 48117dc commit backup.py rectified in 20211004,amount increased of linear trend,eg. [2 4 6 8]  
            >>git reset --hard HEAD^^^  # 一个^表示后退一步，n个^表示后退n步
            
            - 使用~符号
            git reset --hard HEAD~n  # 只能后退，n表示后退n步
            >>git reset --hard HEAD~1            
            HEAD is now at f17479c commit backup_address created in 20210926     
    
    1.2.4.4 reset命令的三个参数
            - soft
              仅仅在本地库移动HEAD指针     
                >>git reset --soft 48117dc
                >>git reflog
                48117dc (HEAD -> master) HEAD@{0}: reset: moving to 48117dc
                1644b18 HEAD@{1}: reset: moving to 1644b18
                f17479c HEAD@{2}: reset: moving to f17479c
                f17479c HEAD@{3}: reset: moving to HEAD~1
                48117dc (HEAD -> master) HEAD@{4}: reset: moving to 48117dc
                >>git status
                
            - mixed
              在本地库移动HEAD指针
              重置暂存区
              >>git reset --mixed f17479c
              >>git reflog
              >>git log --oneline
              >>git status              
              
            - hard
              在本地库移动HEAD指针
              重置暂存区
              重置工作区
              
    1.2.4.5 
    
    
    1.2.5 删除文件并找回
    >>git rm --cached backup.py    
    rm 'backup.py'

    
    1.2.6 比较文件差异
   

    
    1.3 分支管理
    
    2 远程库操作
