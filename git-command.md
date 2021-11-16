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
    
    1.2.4.2 
    
    
    1.2.5 删除文件并找回
    
    1.2.6 比较文件差异
   

    
    1.3 分支管理
    
    2 远程库操作
