Git 上標籤(Tagging)
=========================

程式的開發幾乎都需要所謂的「版號」，除了開發者之間可用以識別正在開發或維護的程式是否版本相同之外，也可以用版號來表示程式的開發狀態(例如 development, alpha, beta, production/stable 等等)，或者是標示重大版本演進(例如從 1.0 演變為 2.0)。

不同的開發者或者開發團隊，也都會有不同的版號規則。但目前常見的 2 種版號規則如下：

- Date based scheme

- Squence-based scheme

更詳細的說明可以參見：http://en.wikipedia.org/wiki/Software_versioning

上版號的工作，就可以利用 Git 標籤的功能來達成。以下本文將介紹如何使用 Git 的標籤功能。

-------------------------
新增標籤 & 列出標籤
-------------------------

Git 的標籤分為 2 種，其分別為 lightweight 以及 annotated 。

1. lightweight

   lightweight 標籤其實就只是一個指標(pointer)指向特定的 commit 而已。

   新增 lightweight 標籤的方法很簡單：

   ::

        $ git tag my-light-tag

   新增完標籤之後，就可以用以下指令列出標籤。

   ::

        $ git tag # 列出所有標籤


   又或者可以用 ``git show <標籤名>`` 來查看 lightweight 標籤所對應到的 commit 資訊。

   ::

        $ git show my-lightweight-tag


2. annotated

   Git 官方文件推薦使用的標籤。 annotated 標籤則不像 lightweight 標籤，annotated 標籤在建立時也會同時儲存許多資訊，例如上標籤的日期、上標籤者的名字、標籤訊息以及 Checksum 等等，此外還可以加簽(sign)進行驗證。

   annotated 標籤與 lightweight 標籤使用類似，但需要 ``-a <tag name>`` , ``-m <message>`` 2 個參數，例如：

   ::

        $ git tag -a 'my-annotated-tag' -m 'my first annotated tag!'

   新增完 annotated 標籤之後就可以使用， ``git tag -n`` 查看：

   ::

        $ git tag -n
        my-annotated-tag my first annotated tag

預設的標籤是加在當前的 commit，不過也可以為之前的 commit 上標籤，只要加上想加標籤的 commit 編號即可：

::

    $ git tag -a '1.0.dev' 3b7de7f

-------------
推送標籤
-------------

標籤的新增都是在本地端(local)進行，所以其他成員或協作者並無法看到你上的標籤，所以就必須將標籤透過 ``git push <upstream> <tag_name>`` 上傳到遠端 Repository 。

例如：

::

    $ git push origin my-lightweight-tag

如此一來，其他成員就能夠透過遠端 Repository 得到你上的標籤。

此外，如果想一次性地將本地端所有標籤上傳，可以使用以下指令：

::

    $ git push --tags


-------------
刪除標籤
-------------

標籤的刪除十分容易，只要用以下指令即可：

::

    $ git tag -d <tag_name>

該指令只會刪除本地端的標籤。不過如果該標籤已經上傳到遠端 Repository 的話，就得使用另外的指令刪除。

--------------------------------
刪除遠端 Repository 的標籤
--------------------------------

刪除遠端 Repository 的標籤指令為：

::

    $ git push <upstream> :refs/tags/<tag_name>

例如：

::

    $ git push origin :refs/tags/my-lightweight-tag

---------------------
利用標籤切換版本
---------------------

在 Git 中無法利用標籤進行版本切換，但是卻可以利用分支的功能，來達到利用標籤進行版本切換的功能。

利用標籤切換版本的指令為：

::

    $ git checkout -b <new_brance> <tag_name>

例如：

::

    $ git checkout -b 'v2.0' my-v2-tag
