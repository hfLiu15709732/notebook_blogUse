- [ ] git初始化
- [ ] 相关linux命令



- [ ] git的基本工作流程与原理



- [ ] 推送至暂存区 git add  git add .  git add --all 

- [ ] 暂存区回退值工作区 git rm --cached 1.txx

- [ ] 查看暂存区 git status

  

- [ ] 提交至历史区 git commit -m "第一个版本"

- [ ] 查看历史区 git log

  

- [ ] 版本穿梭1：版本回退（会直接更新工作区）

- [ ] 退一步：git reset --head HEAD^

- [ ] 退两步：git reset --head HEAD~2

- [ ] 查看历史区所有操作记录 git reflog

- [ ] 回退或返回目标版本：git reset --hard （版本号） 版本号在reflog中查就行

  

- [ ] 版本软回退（回退至暂存区） hard改成soft

- [ ] 软回退一般用于 commit注释写错或者历史区的信息需要修复填充

- [ ] git revert也是回退，只不过reset是指针向前移，revert是新增加一条回退记录，没有hard/soft之分，都是默认hard



- [ ] 查看分支
- [ ] 分支建立的原因
- [ ] 添加分支
- [ ] 切换分支
- [ ] 切换后，再增添文件，再切回原分支，文件是没有的
- [ ] 分支合并
- [ ] 分支删除





- [ ] 可能冲突1：两分支都修改同一个文件，在合并时需要手动合并，保留修改之后，在git add git commit一下就可以合并成功了



- [ ] 团队合作初步
- [ ]    建立远程仓库
- [ ] 提前设置授权签约（按照他给个配置username和email）
- [ ] 添加默认地址   
- [ ] git remote add origin https://gitee.com/hfliu15709732/aabb.git
- [ ] 查看推送地址  git remote -v
- [ ] 删除目标地址  git remote remove origin 
- [ ] 文件推送 git push origin master:master 将本地的maste推到远程的master
- [ ] 也可以选择加上-u为默认操作
- [ ] 例如：git push -u origin master:master 
- [ ] 以后直接git push就行了
- [ ] 不成文规定：尽量不要在远程仓库修改（会造成本地与远程版本不同）
- [ ] 冲突2：当出现版本不一致，本地和远程都改了，pull的使用就会触发自动合并失败
- [ ] 解决方案：先pull回来那会报错，到vscode中配置选项，就解决后在git add git commit 最后git push（这样本地和远程都统一解决了）
- [ ] 拉取更新代码 git pull origin master（在本地会自动合并）





- [ ] 团队协同冲突
- [ ] 克隆项目 git clone
- [ ] 冲突：远程版本更加的新（双方修改的是不同文件的时候）：（同事先push了 所以自己先pull一下，在push）
- [ ] 冲突2：远程版本更加的新（双方修改的是同一个文件）：（同事先push了自己要修改的文件）
- [ ]  所以自己先pull一下（会报错，在到vscode配置一下，在 add   commit   push一下就可以了）





- [ ] 团队分支管理
- [ ] $ git push origin login:login
- [ ] git pull origin login（本地只会显示远程login分支）
- [ ] git checkout login（自动在本地创建login分支并切入）
- [ ] 以后每次在拉取更新的分支 login 需要先切到 login 在 git pull origin login



- [ ] 不小心将不该传到远程的分支传到远程，删除远程分支的方法  git push origin   :bug
- [ ] 讲一个空推到原先的要删除的分支即可
- [ ] 注意！ 克隆是只有主分支的，没有其他分支！！！！！



- [ ] 跨平台合作开发
- [ ] 先用fork将别人的项目查过来（会在自己gitee平台创建一份她的项目），
- [ ] 就可以对这个在自己平台的项目 add commit push pull了
- [ ] 修改好了后，将最终的项目向对方发起pull request
- [ ] 对方批准后，就可以对两项目执行merge合并了