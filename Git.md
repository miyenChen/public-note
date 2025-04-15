---
date created: 2025/02/24
date modified: 2025/04/15
tags:
- git
---

## 基本觀念
- 實作新功能時，一定要在分支上進行
- [Git Flow](https://enginebai.medium.com/git-flow-60b9466e9942)
###### 圖形化工具
- Sourcetree


![[Git commit type]]


## 新建
### 當前目錄新建 git庫
```git
git init
```
## 暫存檔案
遇到事情需要臨時切換分支，或是還沒有做一個段落不想要git commit留下紀錄時
### 先確保文件能全部暫存
如果有新文件還沒被追蹤過  (git stash後工作目錄還有殘留的文件時)
```git
git add .
```
### 儲存
#### ==基本儲存==
```git
git stash
```
#### 儲存+自訂訊息
```git
git stash save "Your message here"
```
### 查看存储的暂存
```git
git stash list
```
### 取出
#### ==取出並刪除紀錄(推薦)==
```git
git stash pop
```
#### 取出但不刪除
```git
git stash apply
```
### 刪除暫存
```git
git stash drop
```
## 本地
### LF / CRLF 轉換的錯誤訊息 - `LF will be replaced by CRLF`

```git
<!--統一換行符號為 LF--> 
git config --global core.eol lf 

<!--自動轉換開關關閉，避免轉換失敗不能進行提交--> 
git config --global core.autocrlf false 

<!--禁止混用 LF 和 CRLF --> 
git config --global core.safecrlf true
```

或加入Git 配置文件
```
# Set the default behavior, in case people don't have core.autocrlf set. 
* text eol=lf
```

改VScode設定
>setting > files:Eol  

### 列出所有被忽略&未被跟蹤的檔案
```git
git ls-files --others --ignored --exclude-standard
```
- 可以用來確認放置API KEY等重要資料的文件不被追蹤

## 查看
### 查看目前檔案更改狀態
```git
git status
```
### 查看當前分支的歷史訊息
```git
git log
```
用`Q`離開
## 遠端
### 拉取遠端的變化和本地分支合併
```
git pull
```
### 確認本地分支配對的預設遠端分支
```git
git config --get branch.main.merge
```
- 確認本地 `main` 這個分支配對到遠端哪一個分支
- 返回 `refs/heads/main` 代表默認配段的遠端分支是 `main`
- 返回 `refs/heads/其他分支` ，代表本地分支連接的遠端分支不同名稱，可[更改](#推送到遠端分支，並建立/更改連結)
### ### 推送到遠端分支，並建立/更改連結
[設定upstream的影響](https://stackoverflow.com/questions/37770467/why-do-i-have-to-git-push-set-upstream-origin-branch)
> **使用情景**
> - 已有本地分支，但還沒跟遠端建立連結，想以後推送都不用指定遠程分支時
> - 已有本地分支，想和遠端已有的分支建立連結時
> - 本地分支和遠端分支名稱不一致 > 改成一致
> 	`git config --get branch.main.merge` 執行時return  `refs/heads/其他分支`
> 	代表連接的分支不同名，而你希望是同名分支

**本地的 main 分支推送到遠端的同名分支**
```git
git push --set-upstream origin main
```
- 跟`git push -u origin main:main`功能相同，寫法不同

**本地的 `local_branch`推送到遠端的 remote_branch，同時建立連結** 
以下兩種方法==如果兩者名稱不同，會導致本地和遠端不同分支名稱建立連結==
- `-u`是`--set-upstream`的簡化名稱，本質上是一樣功能
```git
git push -u origin main:main
```
- git push 遠端名稱 本地分支名稱 : 遠端分支名稱
```git
git push --set-upstream origin local_branch:remote_branch
```


## 撤銷(退回)、覆蓋
### 期間的檔案更改保留，單純撤回commit
**1. 撤銷最新的提交(回到上次提交)**
```git
git reset HEAD~1
```
**2. 回到指定的版本**
```git
// 退回到 <38e7e30> 這個 commit
git reset <38e7e30 >  
```
### 目前檔案不保留，回到此版本的狀態
**1. 撤銷最新的提交(回到上次提交)，檔案也完全回到此版本**
```git
git reset --hard HEAD~1
```
**2. 回到指定的版本，檔案也完全回到此版本**
```git
// 退回到 <38e7e30> 這個 commit，檔案也完全回到此版本
git reset --hard <38e7e30 >  
```
### 覆蓋遠程上的提交
```git
git push origin +HEAD
```
> 注意!! ==+HEAD== 會強制覆蓋遠程倉庫的歷史紀錄，須謹慎避免影響其他協作者

###  強制 push 本地端 branch 到遠端
```git
git push origin <branch> --force
```

## Gitignore 忽略檔案
- 忽略放置API KEY、安裝檔的資料夾
