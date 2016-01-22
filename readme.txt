一.创建版本库
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
二.时光机穿梭
	
	1.版本回退
		1）用 git log 命令显示从最近到最远的提交日志，如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：
			$ git log --pretty=oneline
			---------------------------------------------------------------------
			628e40d94e7989f67cb1438ca184b7ab0d7911d5 git使用说明				 
			4d5be73d0778f54967d03d6d9d947a9af242f016 git使用说明
			6e5bbfa8479399e721c602efb05dc42513f603f9 修改了文件
			833e6fb8ac19e174f487734b6ea7c3fb4f51a2bf 添加一条数据
			167c57cbc0f4c6e5a2de5b57f76a52ca41968c90 wrote a readme file
			----------------------------------------------------------------------
			需要友情提示的是，你看到的一大串类似3628164...882e1e0的是commit id（版本号），和SVN不一样，Git的commit id不是1，2，3……递增的数字，
			而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的commit id和我的肯定不一样，以你自己的为准。为什么commit id
			需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，
			那肯定就冲突了。

			每提交一个新版本，实际上Git就会把它们自动串成一条时间线。如果使用可视化工具查看Git历史，就可以更清楚地看到提交历史的时间线

		2）Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，
			上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
			
			
			git reset --hard HEAD^  退回到上一个版本
			cat readme.txt    查看文件的内容
			
			用git log再看看现在版本库的状态，最新的那个版本  已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？
			办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是3628164...，于是就可以指定回到未来的某个版本：
			
			git reset --hard 3628164
			
			现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？

			在Git中，总是有后悔药可以吃的。当你用$ git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。
			Git提供了一个命令git reflog用来记录你的每一次命令：
			
			$ git reflog
			
			
		小结：
			HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

			穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

			要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
			
	2.工作区和暂存区
		前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

		第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

		第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

		因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

		你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
		
		
-----------------------------------------------2016/01/21--------------------------------------------

	3.管理修改
		Git跟踪并管理的是修改，而非文件。
		git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别。
		
		小结

			现在，你又理解了Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中。
			
			git diff：是查看working tree与index file的差别的。
			git diff --cached：是查看index file与commit的差别的。
			git diff HEAD：是查看working tree和commit的差别的。（你一定没有忘记，HEAD代表的是最近的一次commit的信息）
	4.撤销修改
		1）git checkout -- file可以丢弃工作区的修改：
		
			命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
			
			一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
			
			一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
			
			总之，就是让这个文件回到最近一次git commit或git add时的状态。
		
			注：git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。
		
		2）用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区
		
		小结：
			场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

			场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

			场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
			
			1.撤销工作区的修改符合就近原则：

				工作区 < - 缓冲区 <- 版本库

				撤销工作区修改，如果缓冲区有该文件，则checkout -- file 会将缓冲区的内容覆盖到工作区，此时工作区和缓冲区文件内容相同，因此工作区是干净的，并没有未add的文件。

				撤销工作区修改，如果缓冲区没有该文件，则checkout -- file命令会继续向上找，找到版本库中的该文件，此时使用版本库中的文件覆盖工作区，工作区不干净，因为有未提交的文件（从版本库中覆盖修改的文件）。

			2.撤销缓冲区

				撤销缓冲区，使用git rest HEAD -- file ，直接丢弃缓冲区中的相应内容，只剩下工作区的版本。缓冲区中内容并没有覆盖到工作区，而是直接清除。
				
	5.删除文件

----------------------------------------2016/01/22--------------------------------------------

三、远程仓库
	1.创建远程仓库
		第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。
			如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
				$ ssh-keygen -t rsa -C "youremail@example.com"
				你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

				如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

		第2步：登陆GitHub，打开"Account settings"，"SSH Keys"页面：

				然后，点"Add SSH Key"，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
	2.添加远程库
		第1步：登陆GitHub,在右上角找到"Create a new repo"按钮，创建一个新的仓库
				在Repository name填入  testgit  ，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库
				目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
				现在，我们根据GitHub的提示，在本地的 testgit  仓库下运行命令：
				
				$ git remote add origin git@github.com:Poisonous0/testgit.git
				
				添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。、
				
		第2步：就可以把本地库的所有内容推送到远程库上：
			$ git push -u origin master
			
			把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
			由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
			推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：
			从现在起，只要本地作了提交，就可以通过命令：

			$ git push origin master
			
			把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！
			
		小结

			要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

			关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

			此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
			
			
			**************************************************************************************
			git remote rm origin  删除已经创建的远程仓库名字
			
			
			
四、分支管理
	1.创建与合并
	
		小结

			Git鼓励大量使用分支：

			查看分支：git branch

			创建分支：git branch <name>

			切换分支：git checkout <name>

			创建+切换分支：git checkout -b <name>

			合并某分支到当前分支：git merge <name>

			删除分支：git branch -d <name>

			
			存在问题:
				对文件进行修改之后，在dev分支上不进行add和commit，此时切回到master，如果安装楼上的说法，会更新到master上一次commit的，那么应该添加的内容应该是没有的

	2.解决冲突
		
		小结
		当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
		
		用带参数的git log也可以看到分支的合并情况：

		$ git log --graph --pretty=oneline --abbrev-commit
		
	3.分支管理策略
		合并分为两种： 第一种是 fast forward 强制合并  git merge
					   第二种是禁用fast forward  git merge --no-ff -m "提示" 分支name 
		
	4.bug分支
		git 是一个免费的软件
