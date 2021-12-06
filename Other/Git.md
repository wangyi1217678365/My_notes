<style>
    @import url('../index.css')
</style>
1. [安装教程](https://www.cnblogs.com/hdlan/p/14395189.html)   
2. [生成本地ssh秘钥](https://juejin.cn/post/6974184935804534815)
3. [git平台托管，添加秘钥](https://juejin.cn/post/6974184935804534815)
4. 全局配置 Git 的用户名和邮箱
```
    git conifg --global user.name "XXX"
    git config --global user.email "XXX@XX.com"
```
### Git 的工作区域和流程
![git的四个工作区跟一些常见操作](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ec7210655b4d4fc4afcd1466d9aa2343~tplv-k3u1fbpfcp-watermark.awebp)   
- <i class="danger">Workspace：</i>工作区，就是平时进行开发改动的地方，是当前看到最新的内容，在开发的过程也就是对工作区的操作
- <i class="danger">Index：</i>暂存区，当执行 git add 的命令后，工作区的文件就会被移入暂存区，暂存区标记了当前工作区中那些内容是被 Git 管理的，当完成某个需求或者功能后需要提交代码，第一步就是通过 git add 先提交到暂存区。
- <i class="danger">Repository：</i>本地仓库，位于自己的电脑上，通过 git commit 提交暂存区的内容，会进入本地仓库。
- <i class="danger">Remote：</i>远程仓库，用来托管代码的服务器，远程仓库的内容能够被分布在多个地点的处于协作关系的本地仓库修改，本地仓库修改完代码后通过 git push 命令同步代码到远程仓库。
- 一般来说，Git 的工作流程分为以下几步
<div class="bg">

    1.在工作区开发，添加，修改文件。
    2.将修改后的文件放入暂存区。
    3.将暂存区域的文件提交到本地仓库。
    4.将本地仓库的修改推送到远程仓库。
</div>

### Git 基本操作
#### git add（添加到暂存区）
```
    git add XXX  // 添加某个文件到暂存区，后面可以跟多个文件，已空格区分
    git add .  // 添加当前文件的所有更改到暂存区
    git reset HEAD  // 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了 
    git reset HEAD XXX/XXX/XXX.java  // 就是对某个文件进行撤销了
```
#### git commit （添加暂存区的内容到本地仓库，并记录）
````
    git commit  // 提交暂存的更改，会调用vim编辑器进行信息编写。窗口最一开始打开时是不能输入的。按一下 i 就可以开始输入我们的提交信息。输入完成后先按一下Esc，退出输入状态。w  保存命令。将我们输入的提交信息保存。q  退出窗口。
    git commit -m '备注提交记录'  // 提交暂存的更改，并记录下备注
    git commit -am '备注提交记录'  // 等同于 git add . && git commit -m
    git commit --amend  // 对最近一次的提交的信息进行修改,此操作会修改commit的hash值
    git reset --soft HEAD^  // 撤销这次的commit
        // --soft 不删除工作空间的改动代码 ，撤销commit到暂存区
        // --hard 删除工作空间的改动代码，撤销commit且撤销add（删除！！！）
````
#### git pull
```
    git pull <远程主机名> <远程分支名>:<本地分支名>  // 从远程仓库拉取代码并合并到本地，可简写为 git pull 等同于 git fetch && git merge 
    git pull --rebase <远程主机名> <远程分支名>:<本地分支名>  // 使用rebase的模式进行合并
```
#### git fetch
与 git pull 不同的是 git fetch 操作仅仅只会拉取远程的更改，不会自动进行 merge 操作。对你当前的代码没有影响
```
    git fetch <远程主机名> <分支名>  // 获取远程仓库特定分支的更新
    git fetch --all  // 获取远程仓库所有分支的更新
```
#### git branch
```
    git branch <branch-name>  // 建本地分支，但不切换
    git branch  // 查看本地分支
    git branch -r  // 查看远程分支
    git branch -a  // 看本地和远程分支
    git branch -D <branch-nane> // 删除本地分支
    git branch -m <old-branch-name> <new-branch-name>  // 重新命名分支
```
### 工作中使用 Git 解决问题的场景
#### git rebase 让你的提交记录更加清晰可读
<p class="bg">rebase 翻译为变基，他的作用和 merge 很相似，用于把一个分支的修改合并到当前分支上。但是大部分情况下，rebase 的过程中会产生冲突的，此时，就需要手动解决冲突，然后使用依次 git add  、git rebase --continue  的方式来处理冲突，完成 rebase 的过程，如果不想要某次 rebase 的结果，那么需要使用 git rebase --skip  来跳过这次 rebase 操作。</p>

**git merge 和 git rebase 的区别**
1. 不同于 git rebase 的是，git merge 在不是 fast-forward（快速合并）的情况下，会产生一条额外的合并记录，类似 Merge branch 'xxx' into 'xxx' 的一条提交信息。
2. 在解决冲突的时候，用 merge 只需要解决一次冲突即可，简单粗暴，而用 rebase 的时候 ，需要依次解决每次的冲突，才可以提交。
3. git rebase可以用来合并本地的提交记录
```
    git rebase -i <base-commit> // 参数 base-commit 就是指明操作的基点提交对象，基于这个基点进行 rebase 的操作
```
<p class="bg">特别注意，只能在自己使用的 feature 分支上进行 rebase 操作，不允许在集成分支上进行 rebase，因为这种操作会修改集成分支的历史记录。</p>


#### 使用 git cherry-pick 获取指定的 commit
<p class="bg">git cherry-pick 可以理解为”挑拣”提交，和 merge 合并一个分支的所有提交不同的是，它会获取某一个分支的单笔提交，并作为一个新的提交引入到你当前分支上。当我们需要在本地合入其他分支的提交时，如果我们不想对整个分支进行合并，而是只想将某一次提交合入到本地当前分支上，那么就要使用 git cherry-pick 了。
如下场景，以下有三条分支，feature/cherry-pick1 和 feature/cherry-pick2 都是基于 master 检出的两条功能性分支，对应的分支 log 记录如下</p>

![分支1](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/66e205eb421841bfa476167e6fd7c581~tplv-k3u1fbpfcp-watermark.awebp)

![分支1](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c5743a18a0c24b3aa085db3d7a620742~tplv-k3u1fbpfcp-watermark.awebp)   
现在 master 只需要 feature/cherry-pick1 和 feature/cherry-pick2 有关 change 的修改，并不关心有关 fix 内容的修改。此时就可以用 cherry-pick 指令了。    
语法： 
```
git cherry-pick [commit-hash]; 如果需要多个 cherry-pick 需要同步到目标分支，可以简写为 git cherry-pick <first-commit-id>...<last-commit-id>
```
<p class="bg">commit-hash 表示的是某次 commit 的 hash 值。现在，依次执行以下两条指令 git cherry-pick e0bb7f3、git cherry-pick c9a3101，过程中，如果出现冲突，解决冲突后 进行 git add ，接着执行 git cherry-pick --continue
所有。商业转载请联系作者获得授权，非商业转载请注明出处。</p>   


#### 使用 git revert 回滚某次的提交
<p class="bg">git revert 撤销某次操作，此操作不会修改原本的提交记录，而是会新增一条提交记录来抵消某次操作。</p>    
语法： <i class="danger">git revert <commit-id> </i>针对普通 commit \\ <i class="danger">git revert <commit-id> -m </i>针对 merge 的 commit

- git revert 也可以回滚多次的提交
<p class="bg">语法：git revert [commit-id1] [commit-id2] ... 注意这是一个前开后闭区间，即不包括 commit1 ，但包括 commit2 。</p>

- git reset 跟 git revert 的区别
  1. git revert 会新建一条 commit 信息，来撤回之前的修改。git reset 会直接将提交记录退回到指定的 commit 上。
<p class="bg">对于个人的分支而言，可以使用 git reset 来回退历史记录，之后使用 git push --force 进行推送到远程，但是如果是在多人协作的集成分支上，不推荐直接使用 git reset 命令，而是使用更加安全的 git revert 命令进行撤回提交。这样，提交的历史记录不会被抹去，可以安全的进行撤回。</p>

#### 使用 git stash 来暂存文件
<p class="bg">会有这么一个场景，现在你正在用你的分支上开发新功能。这时，生产环境上出现了一个 bug 需要紧急修复，但是你这部分代码还没开发完，不想提交，怎么办？这个时候可以用 git stash 命令先把工作区已经修改的文件暂存起来，然后创建切换到 hotfix 分支上进行 bug 的修复，修复完成后，切换回 feature 分支，从堆栈中恢复刚刚保存的内容。
</p>

基本命令如下：
```
    git stash //把本地的改动暂存起来
    git stash save "message" 执行存储时，添加备注，方便查找。
    git stash pop // 应用最近一次暂存的修改，并删除暂存的记录
    git stash apply  // 应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即 stash@{0}，如果要使用其他个，git stash apply stash@{$num} 。
    git stash list // 查看 stash 有哪些存储
    git stash clear // 删除所有缓存的 stash
```

#### 不同的工作区域撤销更改
1. git checkout -- <filename>：撤销工作区文件的修改。如果要撤回多个文件的修改，文件之间使用空格隔开