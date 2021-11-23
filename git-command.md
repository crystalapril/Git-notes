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
                
                       暂存区    工作区              
              本地库
                
            - mixed
              在本地库移动HEAD指针
              重置暂存区
              >>git reset --mixed f17479c
              >>git reflog
              >>git log --oneline
              >>git status    
              
                                 工作区
              本地库    暂存区
              
            - hard
              在本地库移动HEAD指针
              重置暂存区
              重置工作区
    
    1.2.5 删除文件并找回    
    1.2.5.1 删除文件    
    >>git rm --cached backup.py    
    rm 'backup.py'
    >>git status    
    On branch master
    changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout --<file>..." to discard changes in working directory)
            deleted: backup.py   
    # 提交至缓存区
    >>git add backup.py   # 把删除 backup.py 的操作add给缓存区
    >>git commit -m "delete backup.py" backup.py  # 提交至本地库    
     1 file changed, 120 deletions(-)
     delete mode 100644 backup_address.py
    >>git status 
    On branch master
    nothing to commit, working tree clean
    >>git reflog
    5e2ffe0 (HEAD->master) HEAD@{0}: commit: delete backup.py  
    
    1.2.5.2 找回文件
    前提：删除前，文件存在时的状态提交到了本地库
    操作：git reset --hard [指针位置]
         - 删除操作已经提交到本地库：指针位置指向历史记录
         - 删除操作尚未提交到本地库：指针位置使用HEAD
    eg1.     
    >>git reset --hard 567f4e1
    HEAD is now at 567f4e1 backup.py  
    >>cat backup.py  
    eg2.
    >>git reset --hard HEAD
    HEAD is now at 567f4e1 backup.py  
    
    1.2.6 比较文件差异
    git diff [文件名]
    - 将工作区中的文件和暂存区进行比较
    git diff [本地库中历史版本] [文件名]
    - 将工作区中的文件和本地库历史记录比较
    git diff
    - 不带文件名比较多隔文件
    
    eg1.
    >>git diff backup.py
    diff --git a/backup.py b/backup.py
    index 063c5ed..e5d5097 100644
    --- a/backup.py
    +++ b/backup.py
    @@ -27,19 +27,32 @@ c = Conflux(provider)  # 可能会出现 warning，不用管

    -amount_in = int(5*10**18)
    +amounts = [int(5*10**18),int(10*10**18),int(15*10**18)]^M

    -def opportunity(path,amount_in):
    -    b = backup_fun.cal_amount_out(path,amount_in)
    -    if b < amount_in:
    +def opportunity(path,amounts):^M
    +    differences = []^M
    +    for amount_in in amounts:^M
    
    eg2.
    >>git diff HEAD backup.py
    >>git diff HEAD^ backup.py
    
    eg3.
    >>git diff
    >>git diff HEAD 

    
    1.3 分支管理
    
    1.3.1 什么是分支
    
    master ---------------------------------- master1.1 ------- master1.2
                                                |                   | 
    feature1 ----- featuer1.1 ---- feature1.2----                   |
                                                                    |
    feature2 ----------------feature2.1 -------------------------feature2.2    
    
    1.3.2 分支操作
    - 创建分支
      git branch [分支名]
      
    - 查看分支
      git branch -v
      
    - 切换分支
      git checkout [分支名]
      
    - 合并分支
      第一步：切换到接受修改的分支（被合并，增加新内容）
      git checkout [被合并分支名]      
      第二步：执行merge 命令
      git merge    [有新内容分支名]
      
    - 解决冲突
      - 冲突的表现：
      ...
      <<<<<<< HEAD
      code        edit by branch    # 当前分支内容
      ============
      code        edit by master    # 另一分支内容
      >>>>>>> master
      ...
      - 冲突的解决：
      第一步：编辑文件，删除特殊符号
      第二步：把文件修改到满意的程度，保存退出
      第三步：git add [文件名]
      第四步：git commit -m "日志信息"   # 注意：此时 commit 一定不能带具体文件名        
    
    eg.
    >>git branch -v   # 查看分支
    * master 1644b18 commit the 2nd version of backup.py which amount=[2,4,6,8],increased by linear    
    >>git branch test_branch
    >>git branch -v   # *表示目前位于哪个分支
    * master      1644b18 commit the 2nd version of backup.py, increased by linear
      test_branch 1644b18 commit the 2nd version of backup.py, increased by linear    
    >>git checkout test_branch
    Switched to branch 'test_branch'
    >>git branch -v
      master      1644b18 commit the 2nd version of backup.py,increased by linear
    * test_branch 1644b18 commit the 2nd version of backup.py,increased by linear
    
    >>vi backup.py    # 修改文件
    >>git add backup.py
    >>git commit -m "test function of branch" backup.py
    >>test function of branch    
    [test_branch 850a4c5] test function of branch
     1 file changed, 1 insertion(+)
    >>git branch -v
      master      1644b18 commit the 2nd version of backup.py which amount=[2,4,6,8],increased by linear
    * test_branch 850a4c5 test function of branch
    >>git checkout master
    Switched to branch 'master'
    >>git merge test_branch    # 合并分支，发现出现冲突
    Auto-merging backup.py
    CONFLICT (content):Merge conflict in backup.py
    Automatic merge failed; fix conflic and then commit the result.
    
    >>vi backup.py    # 解决冲突
    >>git status    
    On branch test_branch
    You have unmerged paths.
        (fix conflicts and run "git commit")
        (use "git merger --abort" to abort the merge)
     Unmerged paths:
        (use "git add <file>..." to mark resolution)
            both modified: backup.py
     no changes added to commit (use "git add" and/or "git commit -a")
     >>git add backup.py
     >>git status
     On branch test_branch
     All conflicts fixed but you are still merging.
        (use "git commit" to conclude merge)
     Changes to be commited:
        modified: backup.py
     >>git commit -m "resolve conflict"   # 注意：此处不要接文件名
     >>git status
     On branch test_branch
     nothing to commit, working tree clean           

    1.4 git基本原理
    1.4.1 哈希
    哈希是一系列的加密算法，具有以下几个共同特点：
    - 不管输入数据量有多大，输入同一个哈希算法，得到的加密结果长度固定
    - 哈希算法确定，输入数据确定，输出数据能保证不变
    - 哈希算法不可逆
    
    git底层采用的是sha1 算法
    原始文件 ----> sha1 ----> 哈希值  （明文 ---> 算法 ---> 密文）    
    
    1.4.2 git保存版本的机制   
    - 集中式版本控制工具的文件管理机制 
      以文件变更列表的方式储存信息。
      这类系统将保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异
      v1          v2           v3           v4          v5
      file A ---> A1 ---------------------> A2
      file B -----------------------------> B1 -------> B2
      file C ---> C1 --------> C2 --------------------> C3
      
    - git 文件管理机制
      git把数据看作是小型文件系统的一组快照
      每次提交更新时，git都会对当前的全部文件制作一个快照并保存这个快照的索引。
      如果文件没有修改，git不再重新储存该文件，而是只保留一个链接指向之前储存的文件。
      v1          v2          v3          v4          v5
      file A      A1          A1          A2          A2
      file B      B           B           B1          B2
      file C      C1          C2          C2          C3
      
    - git 文件的提交对象    
    98ca9           ---->  92ec2     (后面三个分支)
    commit   size          tree     size
    tree     92ec2         blob      5b1d3  README     ---->  5b1d3 (blob size  testing library)
    author   april         blob      911e7  LICENSE    ---->  911e7 (blob size  the MIT license)
    commiter april         blob      cba8a  test.rb    ---->  cba8a (blob size  test case)
    
    - git 提交对象及其父对象形成的链条
    98ca9           <----  34ac2           <----  f30ab 
    commit   size          commit   size          commit   size       
    tree     92ec2         tree     184ca         tree     0de24   
    parent                 parent   98ca9         parent   34ac2
    author   april         author   april         author   april      
    commiter april         commiter april         commiter april  
    "the intial commit"    "fixed bug"            "add feature"
       |                       |                      |
    snapshot A             snapshot B             snapshot C
    
    1.4.3 git 分支管理机制
    git的分支管理，主要是指针的移动
    
    （1）创建分支
                                   HEAD 
                                    |
                                  master
                                    |
    98ca9   <----  34ac2   <----  f30ab 
                                    |
                                 testing   
   
    （2）把head 指向分支
                                  master
                                    |
    98ca9   <----  34ac2   <----  f30ab 
                                    |
                                 testing
                                    |
                                   HEAD
                                   
    （3）在分支上做修改，并提交
                                   HEAD 
                                    |
                                  master
                                    |
    98ca9   <----  34ac2   <----  f30ab  <---- 87ab2
                                                 |
                                              testing
                                              
    
    2 远程库操作
    2.1 在github上创建远程仓库
    - 创建仓库，给仓库命名，选择public/private
    - 复制仓库地址，有https/ssh
    
    2.2 仓库远程地址    
    - 查看当前所有远程地址及其别名
      git remote -v    
    - 添加远程地址
      git remote 仓库地址名 仓库地址  
    eg.
    >>git remote first_repo https://github.com/crystalapril/Linux-notes.git
    >>git remote -v
    first_repo https://github.com/crystalapril/Linux-notes.git (fetch)
    first_repo https://github.com/crystalapril/Linux-notes.git (push)
    
    2.3 推送到远程仓库
    git push [仓库地址名] [分支名]
    >>git push first_repo master
    
    2.4 克隆仓库到本地
    git clone [远程仓库地址]
    >>git clone https://github.com/crystalapril/Linux-notes.git
    
    克隆的三个效果：
    - 完整的把远程库下载到本地
    - 创建远程地址别名
    - 初始化本地库
    
    2.5 添加合作开发者
    setting ———— collaborators ———— add collaborators  ———— 等待对方 accept invitation
    
    合作开发者可以接受邀请，然后把仓库 clone 到本地，修改后再 push 到远程仓库
    但是 push 之后不一定会被创始人接受，还需要创始人审核代码，pull下来
    
    2.6 拉取远程仓库的更新信息
    2.6.1 git pull    # 当与其他分支没有没有冲突时，用 pull 最方便
    pull = fetch + merge
    
    git pull [远程仓库地址名] [远程仓库分支]
    >>git pull first_repo master 
    
    2.6.2 git fetch    # 如果可能出现冲突，最好先 fetch 下来，然后检查修改之后，再 merge
    git fetch [远程仓库地址名] [远程仓库分支]
    >>git fetch first_repo master   # 纯读取数据，不需要登录
    >>git checkout first_repo/master
    >>vi backup.py    
    
    2.6.3 git merge
    git merge [远程仓库地址名/远程仓库分支]
    >>git merge first_repo/master
    >>vi backup.py
    >>git commit -m "test pull" backup.py
    >>git push first_repo master
    
    2.6.4 解决冲突
    - 如果不是基于 github 远程库最新版所作的修改，不能推送，必须先拉取
    - 拉取下来后，如果进入冲突状态，则按照“分支冲突解决”操作解决即可
    
    
    2.7 与外部团队协同开发
         | --------------外部团队 ----------------- | ---------本团队 -----------|
    重点：fork --- clone --- push --- pull request --- 代码审核 --- merge --- pull
    
    操作步骤：
    - 第一步：先对对方的远程库进行fork，到自己的远程库
    - 第二步：git clone 到本地
    - 第三步：vi file ，修改文件
    - 第四步：git commit ，提交到本地库
    - 第五步：git push 到远程库
    - 第六步：点击 pull request ，向创始团队提交拉取的申请
      pull request --- new pull request ---create pull request
    - 第七步：创始团队点击 pull request，并审核代码，还可以comment进行对话
    - 第八步：创始团队审核无误后，点击 merge pull request 合并代码，在 confirm merge 的框里填写本次操作的信息
    - 第九步：创始团队再将远程库修改拉取到本地，git pull first_repo/master
    
    2.8 通过ssh来登录
    
    
    
