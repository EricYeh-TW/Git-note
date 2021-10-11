# Git 基本指令筆記

## 初始化

```
$ cd /tmp                  # 切換至 /tmp 目錄
$ mkdir git-practice       # 建立 git-practice 目錄
$ cd git-practice          # 切換至 git-practice 目錄
$ git init                 # 讓 Git 對這個目錄開始進行版控
```

## 取消 GIT 控制

> ### 只要把那個 **.git 目錄** 移除，Git 就對這個目錄失去控制權了。

---

## 查看 Git 狀態

```
$ git status
```

## 將更改過的檔案加入 Git 的暫存區

```
$ git add filename.html     # 將特定檔案加入
$ git add *.html            # 將所有html檔案加入
$ git add .                 # 將所有檔案加入
```

## 將更改過的檔案正式加入 Git

```
$ git commit -m "init commit"       # init commit 是要說明的內容，中英文都可以
```

-   Git 每次的 Commit 都只會處理暫存區（Staging Area）裡的內容，也就是說，如果在執行 git commit 指令的時候，**那些還沒被加到暫存區裡的檔案，就不會被 Commit 到儲存庫裡**。

## 縮短兩段式流程的方法

```
$ git commit -a -m "init commit"     # 直接跳過 add 的程序
```

-   這個指令只對已經存在 Repository 的檔案有效，**對還是新加入的檔案（也就是 Untracked file）是無效的**。

---

## 檢視之前 Commit 的紀錄

```
$ git log                       # 顯示詳細記錄，越新的資訊會在越上面
$ git log --oneline --graph     # 輸出的結果更為精簡，可以一次看到更多的 Commit
```

## 我想要找某個人或某些人的 Commit

```
$ git log --oneline --author="Sherly"           # 只會顯示這個人的 commit
$ git log --oneline --author="Sherly\|Eddie"    # 使用 | 符號來查找兩個以上
```
