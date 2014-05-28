Git基本設定
===========

========================
First thing first.
========================

利用Git進行版本控制之前，需要設定 ``user.name`` 及 ``user.email`` 。主要原因在於能夠透過這兩個設定了解哪些更動是由哪位成員所做出的，並且提供email的聯絡方式，使得團隊成員彼此間能夠確保至少有一個聯絡的管道。

設定 ``user.name`` 及 ``user.email`` 的指令如下所示︰ ::

	$ git config --global user.name "Your Name"
	$ git config --global user.email you@example.com

#. 上述指令加上 ``--global`` 代表全域設定
#. 每一位成員送出新版本(push)時，就是依照上述兩個設定紀錄於Git的日誌檔中

設定 ``user.name`` 及 ``user.email`` 之後，可以使用 ``git config --list`` 查看設定確認是否有設定成功。 ::

	$ git config --list

上述指令顯示結果如下︰ ::

	user.email=vans@gmail.com
	user.name=vans
	core.repositoryformatversion=0
	core.filemode=true
	core.bare=false
	core.logallrefupdates=true
	core.ignorecase=true
	core.precomposeunicode=false

而上述的設定的內容，其實是存在使用者的家目錄下的 ``.gitconfig`` 檔案內( ``~/.gitconfig`` )，因此直接更動這個檔案也可以更改設定。

===============================
設定預設的推送(Push)模式
===============================

從Git推送(Push)模式目前有 ``nothing`` , ``matching`` , ``upstream`` , ``simple`` , ``current`` ，共5種。如果不指定的話，就可能就會顯示以下類似的訊息(依Git版本而異)。 ::

	warning: push.default is unset; its implicit value is changing in
	Git 2.0 from 'matching' to 'simple'. To squelch this message
	and maintain the current behavior after the default changes, use:

	  git config --global push.default matching

	To squelch this message and adopt the new behavior now, use:

	  git config --global push.default simple

	See 'git help config' and search for 'push.default' for further information.
	(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
	'current' instead of 'simple' if you sometimes use older versions of Git)

那麼要選擇哪一種Push模式？
各位可以依照需求設定，詳細的各種模式說明可以使用以下指令查詢 ``push.default`` 的部份： ::

	$ git help config

但一般建議各位使用 ``current`` 或 ``simple`` 即可，使用 ``current`` 模式代表只將目前所在的分支推送至程式碼儲存庫(repository)。相較於其他的推送模式而言，較為安全與單純，設定使用 ``current`` 推送模式可以使用以下指令： ::
	
	$ git config --global push.default current

=======================
設定不需版本控制的檔案 
=======================

進行系統程式開發時，不免會遇到一些檔案需要針對自己的系統環境進行設定，例如設定資料庫帳號密碼，或是使用 ``vim`` 可能會留下 ``.swp`` 暫存檔，這些可能具敏感機密的檔案如果不在版本控制系統中設定忽略的規則，就可能會造成資安事件。

因此，團隊成員之間必須在專案開始前，討論並設定必須將哪些檔案排除在版本控制之外。Git版本控制系統則提供 ``gitignore`` 的設定，可以將指定的檔案排除版本控制的範圍。

有2種方式設定 ``gitignore`` ，一是於專案資料夾內新增 ``.gitignore`` ，二是於使用者家目錄下新增 ``.gitignore`` ，建議2種方式都使用，原因在於讓被包含在專案的 ``.gitignore`` 只需設定專案內部版本控制需要忽略的檔案，使得其他團隊成員就算忘記建立 ``.gitignore`` 也能夠保有一定程度的安全性；而使用者家目錄下的 ``.gitignore`` 則是可依照各人開發習慣以及作業系統的不同進行設定，而不會影響團隊其他成員的 ``.gitignore`` 。

以下是在使用者家目錄下新增MAC OS X ``.gitignore`` 的指令及 ``.gitignore`` 檔案內容 [#f1]_ 。  

::

	$ cd ~
	$ git config --global core.excludesfile ~/.gitignore #設定讀取使用者家目錄下的.gitignore
	$ touch .gitignore
	$ vim .gitignore

MAC OS X 可適用的 ``.gitignore`` 檔案內容 ::
	
	# Compiled source 
	*.com
	*.class
	*.dll
	*.exe
	*.o
	*.so

	# Packages
	# it's better to unpack these files and commit the raw source
	# git has its own built in compression methods
	*.7z
	*.dmg
	*.gz
	*.iso
	*.jar
	*.rar
	*.tar
	*.zip

	# Logs and databases
	*.log
	*.sql
	*.sqlite

	# OS generated files
	.DS_Store
	.DS_Store?
	._*
	.Spotlight-V100
	.Trashes
	ehthumbs.db
	Thumbs.db

	# Editor generated files
	*.swp
	*~

#. ``.gitignore`` 內的設定支援 ``glob`` 語法(簡化過的正規表示式)
#. ``.gitignore`` 也可設定子目錄下的忽略規則，如 ``output/*.*``

==================================
設定Git預設所使用的diff演算法
==================================

Git也使用了多種不同的diff演算法，用來進行不同版本間的程式碼或文字內容的比對，目前有 ``patience`` , ``minimal`` , ``histogram`` , ``myers`` 4種diff演算法，預設使用 ``myers`` 演算法進行比對，此種演算法是使用貪婪(greedy)的方式進行比對，因此有時候比對結果會與我們預期的結果有些出入，建議可以使用 ``patience`` 演算法做為預設的diff演算法。

設定預設diff演算法之指令為： ::

	$ git config --global diff.algorithm patience	

===================================
設定好用的Git指令縮寫
===================================

有些常用的Git指令可以利用別名(alias)的功能變成縮寫，可以有效增加工作效率，以下是一些常用的指令別名的設定： ::

	$ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.st status
	$ git config --global alias.br branch
	
此外，有很多網路資源也各自整理了不少常用的指令縮寫，也建議各位可以針對需求增加。
以下列出一些相對重要的指令別名設定：

* 設定檔案層級的更改忽略(將以下設定寫入至 ``.gitconfig`` 的 ``[alias]`` 區塊內) 
		
	::

		[alias]
			assume = update-index --assume-unchanged
			unassume = update-index --no-assume-unchanged
			assumed = "!git ls-files -v | grep ^h | cut -c 3-"
			unassumeall = "!git assumed | xargs git update-index --no-assume-unchanged"
			assumeall = "!git st -s | awk {'print $2'} | xargs git assume"

	上述設定的簡要說明如下：

	``.gitignore`` 設定檔的功能在於將檔案排除在版本控制的範疇之外，但是有些應用程式的開發不免需要設定檔，設定檔就不太適合列入 ``.gitignore`` 中，因為我們也需要將設定檔的預設項目列入到版本控制之中，但是每個團隊成員經常在開發過程也會針對各自的任務不同而變更設定檔，如果這些變更也被一併提交推送到程式儲存庫中，就可能變更了預設的設定檔的狀態，也可能造成其他成員在合併設定檔時的衝突。

	針對這個問題，我們可以使用 ``git update-index`` 來變更設定檔的狀態，這也是我們設定 ``assume`` , ``unassume`` 等別名的原因。

	例如將設定檔的狀態從「已更改(modified)」變更為「未更改」： 
	
	::

		$ git update --assume-unchanged your_config
		$ git assume your_config #此指令與上一指令相同

* 設定快照(snapshot)功能(將以下設定寫入至 ``.gitconfig`` 的 ``[alias]`` 區塊內) 

	::

		[alias]
			snapshot = !git stash save "snapshot: $(date)" && git stash apply "stash@{0}"

	這個快照功能實際上是使用 ``git stash`` 功能來達成的。簡單而言，就是將目前分支的所有變更以時間為名稱存一份起來，如此一來，就等於是達成了快照功能，你就能夠使用 ``git stash apply "快照名稱"`` 來回復到你所想要回到的那個時間點。

	可以使用以下指令進行快照、列出所有快照、回復快照： ::

		$ git snapshot #快照
		$ git stash list #列出快照
		stash@{0}: On master: Thu Feb 13 14:28:38 CST 2014
		stash@{1}: On master: Thu Feb 12 11:20:58 CST 2014
		$ git stash apply stash@{1} #回復到2014/2/12所做的快照

以上就是2個相對重要的功能，如果不懂的話，先有個認識即可，本文會在後續章節中再做一次介紹。

====
其他
====

* 設定偏好的文字編輯器 ::

	$ git config --global core.editor "vim"

* 開啟Git文字輸出以彩色形式輸出(Git預設輸出是沒有顏色的，我們可以讓Git在輸出時加上顏色讓我們更容易閱讀) ::

	$ git config --global color.ui true
	$ git config --global color.status always

* 客製化提交變更說明 ::

	$ vim your_project/.git/COMMIT_EDITMSG

	# 建議格式

	subject line

	what happened

	[ticket: X]
	# Please enter the commit message for your changes. Lines starting
	# with '#' will be ignored, and an empty message aborts the commit.
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	# modified:   lib/test.rb
	#

.. rubric:: Footnotes

.. [#f1] `Ignoring files <https://help.github.com/articles/ignoring-files>`_ , https://help.github.com/articles/ignoring-files
