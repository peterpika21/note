###### tags: `通用技術`

# GIT指令

## 常用操作

| 指令 | 說明 |
| :--- | :--- |
| git version |查詢版本 |
| git config --list |查詢設定列表|
| git config --global user.name "你的名子" <br> git config --global user.email "你的信箱" | 開發者設置 |
| git init | 建立本地數據庫 |
| git clone 遠端數據庫網址 | 下載專案 |
| git add 檔案名稱 | 檔案加進索引 |
| git add . | 全部檔案加進索引 |
| git status | 查詢狀態 |
| git log | 顯示歷史紀錄 |
| git commit -m "更新訊息" | 將索引提交到數據庫 |
| git reset --hard | 還原工作目錄與索引 |
| git reset HEAD | 全部檔案取消索引 |
| git reset HEAD 檔案名稱 | 單一檔案取消索引 |
| git checkout commit名稱 | 轉跳到某 commit 狀態 |
| git reset --hard "HEAD^" | 刪除最後一次 commit |
| git reset --hard ORIG\_HEAD | 刪錯 commit 還原 |
| git reset --soft "HEAD^" | 刪除最後一次 commit , 但保留異動內容 |
| git commit --amend | commit 後發現有幾個檔案忘了加入進去，想要補內容進去時  |
| git branch | 顯示所有本地分支  |
| git branch 名稱 | 新增分支  |
| git checkout 名稱 | 切換分支  |
| git merge 名稱 | 合併分支 |
| git branch -d 名稱 | 刪除分支  |
| git remote add origin 資料庫網址 | 建立推送的遠端資料庫網址 |
| git push 遠端數據名稱\(通常是origin\) 遠端分支名稱\(master or 其他\) | 推送 |
| git pull | 更新 |

## 暫存

| 指令 | 說明 |
| :--- | :--- |
| git stash | 暫時儲存當前目錄  |
| git stash list | 瀏覽 stash 列表  |
| git stash pop | 還原暫存  |
| git stash drop | 清除最新暫存  |
| git stash clear | 清除全部暫存 |

## 標籤

| 指令 | 說明 |
| :--- | :--- |
| git tag | 查詢標籤 |
| git tag -n | 查詢詳細標籤 |
| git tag -d 標籤名稱 | 刪除標籤  |
| git checkout 標籤名稱 | 移動到標籤版本  |
| git tag 標籤名稱 | 新增輕量標籤  |
| git tag -am "備註內容" 標籤名稱 | 新增標示標籤 |

