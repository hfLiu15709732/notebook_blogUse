# Git使用全面解析（前端视角）



## 第一部分：初识Git：

### 1.1仓库初始化：

> 需要注意的是：**本操作的执行是初始化了==本地仓库==**，远程仓库需要在托管平台建立



```apl
# 初始化本地仓库
- git init



# 完整的执行与反馈如下：
- mkdir test
- cd test/
- git init
- Initialized empty Git repository in /Users/test/.git/
#  初始化空 Git 仓库完毕。
```





### 1.2:基本的linux指令：

| 指令代码 | 指令执行                     |
| -------- | ---------------------------- |
| cd       | 切换目录                     |
| mkdir    | 创建目录                     |
| rmdir    | 删除目录                     |
| cp       | 复制文件或目录               |
| mv       | 移动文件或目录               |
| ls       | 列出文件夹内文件             |
| ls -a    | 列出所有文件（包括隐藏文件） |
| alias    | 定义别名                     |
|          |                              |



> **掌握这些后，对于之后的工作可以更加高效**





## 第二部分：Git的工作原理与流程：

### 2.1流程图：

![](https://blog-use-1316646528.cos.ap-nanjing.myqcloud.com/git%E5%85%A8%E8%A7%A3%E6%9E%90/1.png)



> **注意：本地仓库也可被叫做本地历史区**





### 2.2流程简述：

- 1
- 2
- 3
- 4
- 5
- 6
- 7



## 第三部分：工作流程的具体操作：

### 3.1代码解析：

```apl
# 1.将本地工作区目标推送至本地暂存区
- git add xxx.txt

# 2.将本地工作区所有产生更改的文件推送至本地暂存区（git会自动检测文件更改）
- git add --all  或
- git add .

# 3.查看当前本地暂存区状态
- git status   # 会显示具体暂存区的文件和已更改但为add的文件

# 4.暂存区文件回退至工作区
- git rm --cached xxx.txt



# 5.将暂存区的文件提交至本地仓库
- git commit -m "要写的注释信息"
- git commit -m "A功能完成"


# 6.检查本地仓库当前信息：
- git log

# 7.版本回退（硬回退）
- git reset --head HEAD^  # 退一步

- git reset --head HEAD~2 # 退两步

- git reset --head HEAD~10 # 退10步 以此类推

#  8.注意：硬回退会直接将工作区内容直接回退至当时的位置


# 9.查看历史区所有操作记录（包括回退信息和每次的版本id） 
- git reflog


# 10.根据版本id进行相应的回退（解决回退错误，重新在回退到错误回退之前） 
- git reset --hard 版本id

# 11.版本软回退（原版本文件会回退到暂存区，不会直接更新工作区）
- git reset --soft 版本id
注意：软回退一步与多部，也是直接将hard转为soft即可

# 12.revert新增记录式回退（reset是指针向前移，revert是新增加一条回退记录，没有hard/soft之分，都是默认hard）
- git revert HEAD
注意：上述的语句已经是回退一步了，与reset不同，reset这样是不会退




```



## 第四部分: 初认分支管理与分支操作：

### 4.1分支管理：

### 4.2分支操作：

```apl
#  分支查看：
- git branch


#  添加分支：
- git branch testList
注意：testList是分支名称，尽量不要起中文，有可能有问题

#  切换分支：
- git checkout testList
注意：切换后，增添文件；在切换回原分支，文件是没有的，进而实现了分支之间的工作互不干扰

#  分支合并（需要先切回到主分支或目标分支在做合并）
- git merge testList


#  分支删除
- git branch -d testList
注意：在切换回分支之前，必选保证当前分支的工作已经提交至版本库！！！


```





### 4.3分支冲突：

> **造成原因：**在合并之前，两分支对同一个文件进行了修改

> **出现了该问题，相应区域会爆出需要手动合并的提示（下面是解决的步骤）**



1. 保证两分支最后提交的文件是没问题的
2. 在编译器选择将要保留的地方
3. 再一次将文件提交至暂存区（**重新add xxx.txt一下**)
4. 再一次将文件提交至本地版本库（**再一次commit -m “冲突解决”**）
5. 到此，**两分支已经成功合并**



## 第五部分：团队协作

### 5.1初识团队协作：

#### 5.1.1 基础仓库准备：

1. **建立远程仓库（gitee/github/局域网内的gitlab）**

2. **提前设置授权签约（仓库创建后平台给出的email和username，配置到本地仓库即可）**

3. 添加默认仓库地址 

   ```java
   git remote add origin      //https://gitee.com/hfliu15709732/aabb.git
   //将origin代替https://gitee.com/hfliu15709732/aabb.git这个仓库地址
       
   
   ```



#### 5.1.2 其他相关操作：

```ABAP
git clone xxx.com
//项目克隆


git remote -v
//查看推送地址


git remote remove origin
//删除目标地址


git push origin master:master    //将本地的maste推到远程的master
//文件推送


//也可以选择加上-u为默认操作例如：
git push -u origin master:master 
//这样以后直接git push就行了



//拉取更新代码 
git pull origin master//（在本地会自动合并）


//不成文规定：尽量不要在远程仓库修改（会造成本地与远程版本不同）!!!

```



#### 5.1.3远程与本地仓库不一致造成的冲突：

> **原因：本地和远程都改了信息，pull或push的使用就会触发自动合并失败**



**解决方案：**先pull回来那会报错，到vscode中配置选项，就解决后在git add  &&  git commit 最后git push（这样本地和远程都统一解决了）



### 5.2团队协作冲突（版本不一致造成）

#### 5.2.1冲突发生在不同文件：



**原因：**同事先于你push文件，远程仓库版本高于你的本地版本

**解决方案：**自己先pull更新一遍本地仓库，在push一下即可



#### 5.2.2冲突发生在同一文件：



**原因：**同事先于你push文件，远程仓库版本高于你的本地版本（且两者修改均在一个文件中）

**解决方案：**所以自己先pull一下（会报错，在到vscode配置一下，在 add  commit  push一下就可以了）

### 5.3团队分支管理





### 5.4跨团队协作





## 补充部分1：

### 1.1项目对接：commit规范：

> **不同的项目平台会根据不同，协作时需要先提前确定好，前端的项目一般会写在README.md文件中，**
>
> **==这里给出一个曾经自己在某团队一次项目中的相应文档，也是目前团队项目主流使用的标准==**

```apl
#  commit 的几种类型选项，如下：一般来说至少需要一个类型；不同类型分号隔开。  
例如： git commit -m ci: "增加提交规范"       

- feat: 新功能
- fix: Bug 修复
- docs: 文档更新
- style: 样式修改
- refactor: 代码重构
- perf: 性能优化
- test: 测试更新
- build: 构建系统或者包依赖更新
- ci: CI 配置，脚本文件等更新
- chore: 非 src 或者 测试文件的更新
- revert: commit 回退  
```

