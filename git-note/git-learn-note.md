# git-uer-guide

### basic use:

### git help

- git help -a :全部命令...
  
  ```git
  执行　'git help <command>' 来了解特定子命令
  
  主要的上层命令
     add                  添加文件内容至索引
     am                   应用邮箱格式的系列补丁
     archive              基于一个指定的树创建文件存档
     bisect               通过二分查找定位引入 bug 的提交
     branch               列出、创建或删除分支
     bundle               通过归档移动对象和引用
     checkout             切换分支或恢复工作区文件
     cherry-pick          应用一些现存提交引入的修改
     citool               git-commit 的图形替代界面
     clean                从工作区中删除未跟踪文件
     clone                克隆仓库到一个新目录
     commit               记录变更到仓库
     describe             基于一个现存的引用为一个对象起一个可读的名称
     diff                 显示提交之间、提交和工作区之间等的差异
     fetch                从另外一个仓库下载对象和引用
     format-patch         准备电子邮件提交的补丁
     gc                   清除不必要的文件和优化本地仓库
     gitk                 Git 仓库浏览器
     grep                 输出和模式匹配的行
     gui                  一个便携的 Git 图形客户端
     init                 创建一个空的 Git 仓库或重新初始化一个已存在的仓库
     log                  显示提交日志
     maintenance          运行任务以优化仓库数据
     merge                合并两个或更多开发历史
     mv                   移动或重命名一个文件、目录或符号链接
     notes                添加或检查对象注释
     pull                 获取并整合另外的仓库或一个本地分支
     push                 更新远程引用和相关的对象
     range-diff           比较两个提交范围（如一个分支的两个版本）
     rebase               在另一个分支上重新应用提交
     reset                重置当前 HEAD 到指定状态
     restore              恢复工作区文件
     revert               回退一些现存提交
     rm                   从工作区和索引中删除文件
     shortlog             'git log' 输出摘要
     show                 显示各种类型的对象
     sparse-checkout      初始化及修改稀疏检出
     stash                贮藏脏工作区中的修改
     status               显示工作区状态
     submodule            初始化、更新或检查子模组
     switch               切换分支
     tag                  创建、列出、删除或校验一个 GPG 签名的标签对象
     worktree             管理多个工作区
  
  辅助命令/操作者
     config               获取和设置仓库或者全局选项
     fast-export          Git 数据导出器
     fast-import          Git 快速数据导入器后端
     filter-branch        重写分支
     mergetool            运行合并冲突解决工具以解决合并冲突
     pack-refs            打包头和标签以实现高效的仓库访问
     prune                删除对象库中所有不可达对象
     reflog               管理 reflog 信息
     remote               管理已跟踪仓库
     repack               打包仓库中未打包对象
     replace              创建、列出、删除对象替换引用
  
  辅助命令/询问者
     annotate             使用提交信息注释文件行
     blame                显示文件每一行最后修改的版本和修改者
     bugreport            收集信息以供用户提交错误报告
     count-objects        计算未打包对象的数量和磁盘空间占用
     difftool             使用常见的差异工具显示更改
     fsck                 验证仓库中对象的连通性和有效性
     gitweb               Git web 界面（Git 仓库的 web 前端）
     help                 显示 Git 的帮助信息
     instaweb             在 gitweb 中即时浏览您的工作仓库
     merge-tree           显示三路合并而不动索引
     rerere               重用冲突合并的解决方案记录
     show-branch          显示分支和提交
     verify-commit        检查 GPG 提交签名
     verify-tag           检查标签的 GPG 签名
     whatchanged          显示每一个提交引入的差异日志
  
  与其它系统交互
     archimport           将一个 GNU Arch 仓库导入到 Git
     cvsexportcommit      将一个提交导出到 CVS 检出中
     cvsimport            从另外一个人们爱恨的配置管理系统中拯救您的数据
     cvsserver            Git 的一个 CVS 服务模拟器
     imap-send            从标准输入将一组补丁发送到IMAP文件夹
     p4                   导入和提交到 Perforce 仓库中
     quiltimport          将一个 quilt 补丁集应用到当前分支。
     request-pull         生成待定更改的摘要
     send-email           通过电子邮件发送一组补丁
     svn                  Subersion 仓库和 Git 之间的双向操作
  
  低级命令/操作者
     apply                应用一个补丁到文件和/或索引区
     checkout-index       从索引拷贝文件到工作区
     commit-graph         写入和校验 Git 提交图文件
     commit-tree          创建一个新的提交对象
     hash-object          从一个文件计算对象 ID，并可以创建 blob 数据对象
     index-pack           从一个现存的包存档文件创建包索引
     merge-file           运行一个三路文件合并
     merge-index          对于需要合并的文件执行合并
     mktag                创建一个有额外验证的标签对象
     mktree               基于 ls-tree 的格式化文本创建一个树对象
     multi-pack-index     写入和校验多包索引
     pack-objects         创建对象的存档包
     prune-packed         删除已经在包文件中的多余对象
     read-tree            将树信息读取到索引
     symbolic-ref         读取、修改和删除符号引用
     unpack-objects       从打包文件中解压缩对象
     update-index         将工作区的文件内容注册到索引
     update-ref           安全地更新存储于引用中的对象名称
     write-tree           从当前索引创建一个树对象
  
  低级命令/询问者
     cat-file             提供仓库对象的内容、类型或大小
     cherry               查找尚未应用到上游的提交
     diff-files           比较工作区和索引区中的文件
     diff-index           将一个树和工作区或索引做比较
     diff-tree            比较两个树对象的文件内容和模式
     for-each-ref         对每一个引用输出信息
     for-each-repo        在一个仓库列表上运行 Git 命令
     get-tar-commit-id    从 git-archive 创建的归档文件中提取提交 ID
     ls-files             显示索引和工作区中文件的信息
     ls-remote            显示一个远程仓库的引用
     ls-tree              显示一个树对象的内容
     merge-base           为了合并查找尽可能好的公共祖先提交
     name-rev             查找给定版本的符号名称
     pack-redundant       查找冗余的包文件
     rev-list             按时间顺序列出提交对象
     rev-parse            选出并处理参数
     show-index           显示打包归档索引
     show-ref             显示本地仓库中的引用
     unpack-file          用 blob 数据对象的内容创建一个临时文件
     var                  显示一个Git逻辑变量
     verify-pack          校验打包的Git存仓文件
  ```

- git help -g :手册

- git help <git-commands>

```git
      GIT-COMMIT(1)                                                            Git Manual                                                            GIT-COMMIT(1)

  NAME
         git-commit - Record changes to the repository

  SYNOPSIS
         git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
                    [--dry-run] [(-c | -C | --squash) <commit> | --fixup [(amend|reword):]<commit>)]
                    [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
                    [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
                    [--date=<date>] [--cleanup=<mode>] [--[no-]status]
                    [-i | -o] [--pathspec-from-file=<file> [--pathspec-file-nul]]
                    [(--trailer <token>[(=|:)<value>])...] [-S[<keyid>]]
                    [--] [<pathspec>...]

  DESCRIPTION
         Create a new commit containing the current contents of the index and the given log message describing the changes. The new commit is a direct child
         of HEAD, usually the tip of the current branch, and the branch is updated to point to it (unless no branch is associated with the working tree, in
         which case HEAD is "detached" as described in git-checkout(1)).

         The content to be committed can be specified in several ways:

          1. by using git-add(1) to incrementally "add" changes to the index before using the commit command (Note: even modified files must be "added");

          2. by using git-rm(1) to remove files from the working tree and the index, again before using the commit command;

          3. by listing files as arguments to the commit command (without --interactive or --patch switch), in which case the commit will ignore changes
             staged in the index, and instead record the current content of the listed files (which must already be known to Git);

          4. by using the -a switch with the commit command to automatically "add" changes from all known files (i.e. all files that are already listed in the
             index) and to automatically "rm" files in the index that have been removed from the working tree, and then perform the actual commit;

          5. by using the --interactive or --patch switches with the commit command to decide one by one which files or hunks should be part of the commit in
```

> 阅读 : next; b :periovus ; q : quit;

### git config

- git config 

- git config --global  user.name <xxx>

- git config --global  user.email <xxx>

- git config --list

- git config --help

- git config --unset --global  user.name   ：'delete user.name-config...'

- git config --global color.ui true

- cat ~/.gitconfig  // 查看已经(config)配置的内容

![](/home/administrator/.config/marktext/images/2024-09-16-13-44-38-image.png)

### git init

- git init

- cd ./.git

---

- git status

```git
╭─    ~/my/learnSpace/Study_Notes_Collection/git-note    main +1 !1 ────────────────────────────────────────────────────────────── ✔    13:44:20  
╰─ git status       
位于分支 main
您的分支与上游分支 'origin/main' 一致。

要提交的变更：
（使用 "git restore --staged <文件>..." 以取消暂存）
  修改：     git-note.md

尚未暂存以备提交的变更：
（使用 "git add <文件>..." 更新要提交的内容）
（使用 "git restore <文件>..." 丢弃工作区的改动）
  修改：     git-note.md
```

- git add <file-name>
  
  - git add .  //add all files

- git commit -m 'commit message...'

- git diff <file-name>  // 比较出来的差异是 【工作区】和【暂存区】的比较

### git mv

> shell方式的mv git不能直接识别，会出现原文件删除&新增未跟踪untracked文件如下：

- <mark>git "rename" </mark>

- 1. mv style.css theme.css

- 2. git status
     message: rm style.css

![](/home/administrator/.config/marktext/images/2024-09-16-13-49-19-image.png)

- 3. git rm sysle.css

- 4. git add theme.css
     message: 
        rename: style.css -> theme.css

![](/home/administrator/.config/marktext/images/2024-09-16-13-50-33-image.png)

![](/home/administrator/.config/marktext/images/2024-09-16-13-53-36-image.png)

- 使用 git mv <a.txt> <b.txt>  //a --> b，避免了上述的`git rm ` +  `git add ` 才能实现的` mv ` 过程...

![](/home/administrator/.config/marktext/images/2024-09-16-14-14-19-image.png)

### git diff

- git diff <file-name.xxx>
  
  > 当前文件(workspace)和(storespace)的比较
  
  ```git
  diff --git a/git-note/git-note.md b/git-note/git-note.md
  index 5ab0758..d911f76 100644
  --- a/git-note/git-note.md
  +++ b/git-note/git-note.md
  @@ -180,10 +180,9 @@
                index) and to automatically "rm" files in the index that have been removed from the working tree, and then perform the actual commit;
  
             5. by using the --interactive or --patch switches with the commit command to decide one by one which files or hunks should be part of the commit in
  +```
  
  -  ```
  -
  -  > 阅读 : next; b :periovus ; q : quit;
  +> 阅读 : next; b :periovus ; q : quit;
  
   - git config 
  ```

- git diff  HEAD -- <file-name.xxx>
  
  > 当前文件(workspace)和本地仓库的比较
  
  ```git
  diff --git a/git-note/git-note.md b/git-note/git-note.md
  new file mode 100644
  index 0000000..d911f76
  --- /dev/null
  +++ b/git-note/git-note.md
  @@ -0,0 +1,252 @@
  +# git-uer-guide
  +
  +### basic use:
  +
  +- git help -a :全部命令...
  +  
  +  ```git
  +  执行　'git help <command>' 来了解特定子命令
  +  
  +  主要的上层命令
  +     add                  添加文件内容至索引
  +     am                   应用邮箱格式的系列补丁
  +     archive              基于一个指定的树创建文件存档
  +     bisect               通过二分查找定位引入 bug 的提交
  +     branch               列出、创建或删除分支
  +     bundle               通过归档移动对象和引用
  +     checkout             切换分支或恢复工作区文件
  +     cherry-pick          应用一些现存提交引入的修改
  +     citool               git-commit 的图形替代界面
  +     clean                从工作区中删除未跟踪文件
  +     clone                克隆仓库到一个新目录
  +     commit               记录变更到仓库
  +     describe             基于一个现存的引用为一个对象起一个可读的名称
  ...
  ```

### git rm

- 注意点：只有提交commit后的文件才可被 git rm 

- 如果想 将新修改的文件被 git  rm 时，<mark>需要新提交(git add <>;git commit...)，才可git rm</mark>
  
  ![](/home/administrator/.config/marktext/images/2024-09-16-17-36-45-image.png)
  
  > 修改后，但是还没有提交的文件，是不可以直接被 ` git rm` 的...

### 回退

#### git checkout  //[X]

> git checkout HEAD -- <filename>

#### git revert : 回退提交,可将删除的文件恢复

> git revert  <commit-ID>

#### git reset:

> 修改HEAD指向，后commit，就是在但前HEAD的基础上'垒'

##### --soft

> 不影响工作区和暂存区()，只是回退版本，
> 
> - 此时，还是有未提交的，将此时，还是有未提交的，将工作区和暂存区`git add ; git commit` 就会将新提交接到当前回退的版本上形成新版本
> 
> 

##### --hard

> 工作区和暂存区也跟着回退到指定的版本

##### --mixed(默认选项)

> 将暂存区(add的内容 ) 指定到reset版本上(HEAD^,HEAD^^,...)

##### 

### git branch ： 分支

> git branch <branch-name>

- 切换分支：

> git checkout <branch-name> //[X]：会将分支上还没有提交的内容清空....!!!所以不推荐
> 
> git switch <branch-name> //[switch更合适用于切换]
