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

## 我想要找 某個人 或 某些人 的 Commit

```
$ git log --oneline --author="Sherly"           # 只會顯示這個人的 commit
$ git log --oneline --author="Sherly\|Eddie"    # 使用 | 符號來查找兩個以上
```

## 我想要找 特定 Commit 訊息

```
$ git log --oneline --grep="WTF"
```

## 我想要找到 Commit 的檔案內容有提到 特定字眼

```
$ git log -S "Ruby"     # 訊息有提到 Rudy 這個字的都會被列出
```

## 查詢 某個時間段 的 Commit 歷史

```
$ git log --oneline --since="9am" --until="12am"                       # 可以找出今天早上 9 點到 12 點之間所有的 Commit
$ git log --oneline --since="9am" --until="12am" --after="2017-01"     # 還可以再加一個 after
```

---

## 在 Git 裡刪除檔案

```
$ rm welcome.html         # 這是系統預設的刪除
$ git rm welcome.html     # 用 Git 指令，可以連 Add 的動作一起包含進去

```

## 加上 –cached 參數

```
$ git rm welcome.html --cached
```

-   如果只是「我不是真的想把這個檔案刪掉，只是不想讓這個檔案再被 Git 控管了」的話，可以加上 --cached 參數

## 修改名稱

```
$ mv hello.html world.html         # 系統預設的改名稱
$ git mv hello.html world.html     # 使用 Git 裡面改名稱，效果跟上面刪除一樣
```

---

## 修改 Commit 紀錄

-   要修改 Commit 紀錄有好幾種方法：
    -   使用 git rebase 來修改歷史。
    -   先把 Commit 用 git reset 拆掉，整理後再重新 Commit。
    -   使用 --amend 參數來修改最後一次的 Commit。

## 使用 --amend 參數來進行 Commit

```
$ git log --oneline     # 這是原來的
4879515 WTF
7dbc437 add hello.html
657fce7 add container
```

```
$ git commit --amend -m "Welcome To Facebook"
```

```
$ git log --oneline     # 改過來了
614a90c Welcome To Facebook
7dbc437 add hello.html
657fce7 add container
```

## 使用 rebase 來進行 Commit

```
$ git log --oneline
2bab3e7 add dog 1
ca40fc9 add 2 cats
1de2076 add cat 2
cd82f29 add cat 1
382a2a5 add database settings
bb0c9c2 init commit
```

```
$ git rebase -i bb0c9c2
```

-   -i 參數是指要進入 Rebase 指令的「互動模式」，而後面的 bb0c9c2 是指這次的 Rebase 指令的應用範圍會「從現在到 bb0c9c2 這個 Commit」，也就是最一開始的那個 Commit，這個指令會跳出編輯器。
-   上面的順序，跟 git log 指令的結果是相反的
-   前面的 pick 的意思是「保留這次的 Commit，不做修改」。

```
pick cd82f29 add cat 1
pick 1de2076 add cat 2
```

```
reword cd82f29 add cat 1
reword 1de2076 add cat 2
```

-   說明等一下會修改這兩個 Commit。
-   離開編輯器會馬上再跳出一個，在這裡修改 Commit 的內容，第二個也一樣後存檔離開編輯器。

## 回到 rebase 之前

```
$ git reset ORIG_HEAD --hard
```

---
