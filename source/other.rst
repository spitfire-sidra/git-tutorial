其他
==============


========================
git archive 打包程式
========================

有時候我們會需要把整個程式碼打包成一個壓縮檔，同時把裡面的 ``.git`` 資料夾所儲存的版本控制歷程給移除，可以用以下指令進行打包： ::

    $ git archive <branch> --format=<zip, tar> --output=<output path>

例如： ::

    $ git archive master --format=zip --output=./app_v1.20150621.zip

    $ git archive master --format=tar --output=./app_v1.20150621.tar

此外，還有把當前分支最新的版本打包更快方式： ::

    $ git archive -o latest.zip HEAD


=======================
git 產生 patch 檔
=======================

雖然在版本控制環境下，應該會很少會需要用到 patch 的功能，但有時候會需要把版本間的差異記錄下來，作為回復之用或者給上述移除版本控制歷程的程式緊急更新之用，就會需要用到 patch 的功能。

最簡單的 patch 功能只要利用 git diff 即可產生 patch 檔： ::

    $ git diff <branch/commit id>...<branch/commit id> > diff.patch

例如跟遠端的 master 比較差異後產生 patch 檔： ::

    $ git diff origin/master...HEAD > diff.patch

例如產生當前 unstaged 的 patch 檔： ::

    $ git diff > diff.patch

例如產生 staged 的 patch 檔： ::

    $ git diff --staged > diff.patch

套用 patch 檔指令或者可使用 ``patch`` 指令： ::

    $ git apply diff.patch
    or
    $ patch -p0 < diff.patch # 回復可以用 patch -R -p0 < diff.patch

套用 patch 檔之後，仍需要進行 add, commit 等動作， git 並不會自動幫我們 commit 。


第 2 種產生 patch 檔的功能是用 ``git format-patch`` ，只要指定從哪個 commit id 之後產生 patch 即可，執行之後就會按照順序將一個個 commit 之間的差異產生 patch 檔，不過這種方式所產生的 patch 檔只有 git 能夠使用，必須注意： ::

    $ git format-patch <commit id>

例如產生從 commit id **13e50ace92** 開始產生 patch 檔 ： ::

    $ git format-patch 13e50ace92
    0001-added-conflict.rst.patch
    0002-fixed-syntax.patch
    0003-added-a-new-chapter.patch

執行結果就是按照次序產生一個個 patch 檔。

如果只是需要該 commit id 之後的特定幾個 patch 檔，則可以加上數字來限制： ::

    $ git format-patch 13e50ace92 -1
    0001-added-conflict.rst.patch

而 2 個 commit id 之間的 patch 檔則可以用： ::

    $ git format-patch <commit id 1> <commit id 2>

套用 patch 檔則是使用 ``git am`` ： ::

套用所有 patch 檔： ::

    $ git am *.patch

套用單個 patch 檔： ::

    $ git am 0001-added-conflict.rst.patch

如果套用 patch 時有遇到 conflict 或是用以下幾個指令來解決：

解決 conflict 後 ::

    $ git am --continue

或者不想解決 conflict 想直接跳過這個 patch ： ::

    $ git am --skip

又或者想乾脆放棄，直接回復原本的狀態： ::

    $ git am --abort
