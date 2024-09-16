# git-uer-guide
### basic use:
- git help -a :全部命令...
- git help -g :手册
  - git help <git-commands>
  > f : next; b :periovus ; q : quit;

- git config 
  - git config --global  user.name <xxx>
  - git config --global  user.email <xxx>

  - git config --list

  - git config --help
  - git config --unset --global  user.name   ：'delete user.name-config...'

  - git config --global color.ui true

  - cat ~/.gitconfig  // 查看已经(config)配置的内容



- git init
  - cd ./.git

- git status

- git add <file-name>
  - git add .  //add all files
   
- git commit -m 'commit message...'

- git diff <file-name>  // 比较出来的差异是 【工作区】和【暂存区】的比较

- git "rename" 
	- 1. mv style.css theme.css
	- 2. git status
		message: rm style.css
			 untracked theme.css
	- 3. git rm sysle.css
	- 4. git add theme.css
		message: 
			rename: style.css -> theme.css






