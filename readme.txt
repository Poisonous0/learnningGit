1.创建版本库
	1）在合适的地方创建一个空目录
		命令：mkdir name
		pwd 命令用于显示当前目录
	2）通过 git init 命令把这个目录变成Git可以管理的仓库
2.提交文件
	1）编写一个文件，一定要放到 name 目录（子目录），因为这是Git仓库，放到其他地方Git找不到
		用命令 git add  把文件添加到仓库
			如：git add readme.txt
	2）用命令git commit 把文件提交到仓库
			如：git commit -m "wrote a readme file"
			
			解释：简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录

	总结：
		添加文件到Git仓库，分两步：

		第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；

		第二步，使用命令git commit，完成。
		
3.修改文件并提交
	1）使用git status命令看看结果
		git status 命令可以让我们时刻掌握仓库当前的状态
	2）用命令 git diff 文件名 查看修改了什么内容
		git diff readme.txt 
	3）提交修改和提交新文件时一样的 
		a）git add 文件名
		b）在执行第二步git commit之前，我们再运行git status看看当前仓库的状态
			git status告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了
		c）git commit -m "提交注释"
		d）提交后，我们再用git status命令看看仓库的当前状态

4.版本回退
		
