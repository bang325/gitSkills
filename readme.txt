learnGit 2019/11/25

基本命
初始化一个Git仓库，使用git init命令。
添加文件到Git仓库，分两步：
使用命令git add <file>，注意，可反复多次使用，添加多个文件；
使用命令git commit -m <message>，完成。
git commit -m "备注"

时光机穿梭
要随时掌握工作区的状态，使用git status命令。
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。


版本回退
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
git reset --hard HEAD^
用HEAD表示当前版本，也就是最新的提交1094adb...上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

工作区和暂存区
git add命令实际上就是把要工作区中的文件提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。


管理修改
为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

第一次修改 -> git add -> 第二次修改 -> git commit
你看，我们前面讲了，Git管理的是修改，当你用git add命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
第一次修改 -> git add -> 第二次修改 -> git add -> git commit


撤销修改
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

删除文件
通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：$ rm test.txt
工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

远程仓库
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

从远程库克隆
 git clone git@github.com:bang325/gitSkills.git
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

