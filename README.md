# Git 基本指令筆記

## 初始化

```
$ cd /tmp                  # 切換至 /tmp 目錄
$ mkdir git-practice       # 建立 git-practice 目錄
$ cd git-practice          # 切換至 git-practice 目錄
$ git init                 # 讓 Git 對這個目錄開始進行版控
```

## 取消 GIT 控制

-   只要把那個 `.git 目錄` 移除，Git 就對這個目錄失去控制權了。

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

## 檢視特定檔案的歷史紀錄

```
$ git log -p welcome.html     # log 後面直接接檔名，-p 會再更詳細列出資訊
```

## 檢視某個檔案的某一行是誰寫的

```
$ git blame welcome.html             # 會在這行程式碼前面出現識別代號
$ git blame -L 5,10 welcome.html     # -L 可以控制顯示的行列數
```

---

## 在 Git 裡刪除檔案

```
$ rm welcome.html         # 這是系統預設的刪除
$ git rm welcome.html     # 用 Git 指令，可以連 Add 的動作一起包含進去

```

## 加上 `--cached` 參數

```
$ git rm welcome.html --cached
```

-   如果只是「我不是真的想把這個檔案刪掉，只是不想讓這個檔案再被 Git 控管了」的話，可以加上 `--cached` 參數

## 修改名稱

```
$ mv hello.html world.html         # 系統預設的改名稱
$ git mv hello.html world.html     # 使用 Git 裡面改名稱，效果跟上面刪除一樣
```

---

## 修改 Commit 內容敘述

-   要修改 Commit 內容敘述有好幾種方法：
    1.  使用 `git rebase` 來修改。
    2.  先把 Commit 用 `git reset` 拆掉，整理後再重新 Commit。
    3.  使用 `--amend` 參數來修改最後一次的 Commit。

## 使用 `--amend` 參數來修改 Commit 內容敘述

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

## 使用 `rebase` 來修改 Commit 內容敘述

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

-   `-i` 參數是指要進入 `Rebase` 指令的「互動模式」，而後面的 bb0c9c2 是指這次的 `Rebase` 指令的應用範圍會「從現在到 bb0c9c2 這個 Commit」，也就是最一開始的那個 Commit，這個指令會跳出編輯器。
-   上面的順序，跟 `git log` 指令的結果是相反的
-   前面的 `pick` 的意思是「保留這次的 Commit，不做修改」。

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

## 回到 `rebase` 之前

```
$ git reset ORIG_HEAD --hard
```

## 追加檔案到最近一次的 Commit

-   如果不想因此再發一次 Commit，想把這個檔案併入最近一次 Commit，可以這樣做：
    1. 使用 `git reset` 把最後一次的 Commit 拆掉，加入新檔案後再重新 Commit。
    2. 使用 `--amend` 參數進行 Commit。

例如，我有一個檔案叫做 cinderella.html，想把它加到最後一次的 Commit 裡，假設它剛加進來，它的狀態還是 `Untracked`，流程上一樣，還是使用 `git add` 先把檔案加到暫存區，接著在 Commit 的時候加上 `--amend` 參數

```
$ git commit --amend --no-edit      # --no-edit 參數的意思是指「我不要編輯 Commit 訊息」
```

---

## 回復被刪除的檔案或目錄

```
$ git checkout welcome.html     # 回復特定檔案到上一次 Commit 的狀態
$ git checkout .                # 回復所有被刪除的檔案到上一次 Commit 的狀態
```

-   當使用 git checkout 分支名稱 的時候，Git 會切換到指定的分支，但如果後面接的是檔名或路徑，Git 則不會切換分支，而是把檔案從 .git 目錄裡拉一份到目前的工作目錄。

```
$ git checkout HEAD~2 welcome.html     # 這樣會回復到上上一次 Commit 的狀態
$ git checkout HEAD~2 .
```

## 回復檔案狀態到之前的版本

-   `Reset` 這個英文單字的翻譯是「重新設定」，用中文來說比較像是「前往」或「變成」，指令應該要解讀成「我要前往前幾個 Commit 之前的狀態」或是「我要變成前幾個 Commit 之前的狀態」。

```
$ git reset e12d8ef^     # e12d8ef 是指在 e12d8ef 這個 Commit 的前一次
$ git reset e12d8ef^^    # 前兩次
$ git reset e12d8ef~5    # 前五次
```

-   而且因為剛好 `HEAD` 跟 `master` 目前都是指向 `e12d8ef` 這個 Commit，而且 `e12d8ef` 這個數字也不好記，所以上面這行，通常也會改寫成：

```
$ git reset master^
$ git reset HEAD^
```

-   也可以直接指名某個版本:

```
$ git reset 版本號
```

-   git reset 指令可以搭配參數使用，常見到的三種參數，分別是 `--mixed`、`--soft` 以及 `--hard`，這會決定拆出來的檔案回留在哪裡:

|    Soft    |  Mix(預設)   |   Hard   |
| :--------: | :----------: | :------: |
| 丟回暫存區 | 丟回工作目錄 | 直接丟掉 |

## 又想要回到後面的版本

-   先使用 `git reflog` 或 `git log -g` 來查看最近的紀錄， 當 `HEAD` 有移動的時候（例如切換分支或是 `reset` 都會造成 `HEAD` 移動），Git 就會在 `Reflog` 裡記上一筆。
-   如果想要退回剛剛 `Reset` 的這個步驟，只要 `Reset` 回到一開始那個 Commit 的 `e12d8ef` 就行了：

```
$ git reset e12d8ef --hard
```

## 參考

[為你自己學 Git](https://gitbook.tw/ "link")
