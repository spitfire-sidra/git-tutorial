Git flow 分支策略
==========================

在本文中，我們依據 `A successful Git branching model <http://nvie.com/posts/a-successful-git-branching-model/>`_
一文展示一種開發時可以應用的分支策略模型，不論是大型專案或者小型應用程式，此模型皆可套用於其中，並享受到導入此分支策略模型所帶來的成功。在文章裡有關專案(project)的任何細節將不被談論，只談論分支(branching)的策略及發佈(release)的管理。下圖即是整個分支策略的概觀。後續小節將敘述相關分支策略的細節。

.. figure:: images/git-branching-model.png
    :align: center

    Git branching model [Ref1]_

=========================
主分支(Master)
=========================

Git branching model 一圖中最主要的分支有兩大個，主分支(master)與開發分支(develop)。這兩個分支原則上一直都存在於版本控制的repository中，而且各司其職。以下將分別介紹兩大分支：

- 主分支(master)：處於production-ready的狀態，換句話說，即是該版的source
  code是可運行的、符合專案需求的、設計良好的、穩定的、可維護的、可擴展的及已文件化的。 [Ref3]_

- 開發分支(develop)：下次發佈版本的最新狀態。從主要分支分出來。有些開發者也稱開發分支為整合分支，自動化測試所根據的程式碼(source code)即是以此分支上的版本為基準來進行測試。

當開發分支上的source code達到一定穩定程度，並準備發佈時，即可將該分支上的變更合併回主要分支，並標上發佈的版本號。這就是所謂的新產品發佈。以下將逐一探討實作細節。

.. [Ref3] Define “production-ready”, http://programmers.stackexchange.com/questions/61726/define-production-ready


================================================
支援分支(feature, release, hotfix)
================================================

除了主分支，我們也會使用支援分支來幫助專案開發。支援分支可讓整個團隊更容易管理新功能的開發、產品發佈分支或是快速修改一些bug。支援分支與主要分支最大的差別在於，支援分支在支援任務結束後就會移除，而主要分支則是始終存在。

本文中我們所使用到的支援分支有：功能分支(feature)、發佈分支(release)、修補分支(hotfix)。每一個支援分支都有其特殊支援目的，並嚴格遵循分支與合併的規定。例如：功能分支(feature)只能從開發分支(develop)分離出來，也只能與開發分支(develop)合併。


功能分支 (feature branch)
------------------------------------------------

功能分支原則：

- 從開發分支分離。
- 合併回開發分支。
- 分支命名規則：除了master, develop, release-*, or hotfix-* 以外的功能名稱都行。

功能分支顧名思義就是開發新功能的分支。只要新功能未開發完成，功能分支就會持續存在直到開發完成並合併回開發分支或直到放棄開發此新功能。另外，此分支通常只會存在於該功能的開發者的本機端的儲存庫(local repository)，遠端的中央儲存庫(remote repository 或者 remote origin)是不會有的。

開功能分支詳細步驟：
		
1. 開「功能」分支 ::

		# 從開發分支開一名為「myfeature」的分支，開完後切換到myfeature分支
		$ git checkout -b myfeature develop

#. 將已開發完成之功能分支合併回develop分支 ::

		# 切換至開發分支
		$ git checkout develop
		
		# 將myfeature分支合併到開發分支
		$ git merge --no-ff myfeature
		
		# 刪除myfeature分支
		$ git branch -d myfeature
		
		# 將開發分支push到遠端的origin
		$ git push origin develop
		
``--no-ff`` 參數可以保存myfeaute分支上的歷史記錄，讓開發者可以更了解開發的來龍去脈。詳情如下圖所示：

.. figure:: images/merge-without-ff.png
    :align: center

    ``--no-ff`` 參數示意圖 [Ref1]_

右圖中是沒有加入 ``--no-ff`` 的情況，所以無法看見哪些提交(commit)是專門因為開發新功能而產生的，若有追蹤程式碼的需求時，開發者必須得要自行閱讀所有的log訊息才能知道問題也許出在哪一個版本，相較之下較為不便。

發佈分支 (release branch)
--------------------------------------------------

發佈分支原則：

- 從開發分支分離。
- 合併回開發分支或主要分支。
- 分支命名規則：release-*
	
發佈分支是為了新版本發佈而存在的，換言之，可以說是將程式碼與主分支(master)合併前的過渡階段，可以想像成在發佈新版本之前所必須進行的相關手續。例如在此分支必須進行一些整合性測試，並進行小bug修復及增加一些metadata(例如版本號或是build日期等)。有了發佈分支，開發分支就可以只需專注接收功能分支(feature)所合併進來的功能即可。

一般而言，當產品開發到一個階段時，發佈產品的需求就會自然顯露出。此時，即是開發佈分支的關鍵時刻。在此階段，所有為了這一次發佈所開發的新功能就需要合併到開發分支(develop)，接著再由開發分支(develop)分出發佈分支，開始進行相關發佈前的手續。

制定版本號碼的最佳時機是在開發佈分支時。直到發佈分支之前，開發分支上有許多「為了下一次發佈」所合併的新功能，但對於版本號是0.3或1.0尚無法確認。而發佈分支便是為了確認版本號等其他事情而開啟的，版本號碼的制定從開分支時就需制定且遵循專案版本號制定規則。

發佈分支詳細步驟：

1. 開「發佈」分支 ::
		
		# 從開發分支開一支名為「release-1.2」的分支，開完後切換到release-1.2分支。
		$ git checkout -b release-1.2 develop

#. 制定版本號 ::
		
		# commit 一個版本，commit 訊息為「版本號跳躍至1.2」
		$ git commit -a -m "Bumped version number to 1.2"
		
#. 將已制定好metadata或已修復小bug的發佈分支合併到主要分支 ::

		# 切換至主要分支
		$ git checkout master
		
		# 將release-1.2分支合併到主要分支
		$ git merge --no-ff release-1.2
		
		# 上tag
		$ git tag -a 1.2

#. 將已制定好metadata或已修復小bug的發佈分支合併回開發分支 ::

		# 切換至開發分支
		$ git checkout develop
		
		# 將release-1.2分支合併回開發分支
		$ git merge --no-ff release-1.2
		
#. 刪除release-1.2分支 ::

		$ git branch -d release-1.2


修補程式分支 (hotfix)
----------------------------------------------

修補程式分支原則：

- 從主分支分離。
- 合併回開發分支或主分支。
- 分支命名規則：hotfix-*

修補程式分支的誕生通常是因為出現了一個急需在短時間內修復的bug，無法等到下一次發佈時才修復，例如與伺服器間的通訊有bug，需要馬上進行更新修正。此分支通常從主分支某個已標上tag的commit分出，bug經過修復後，可合併到開發分支，或者是合併回主分支，並標上另一版本號的tag。開此分支的目的在於開發分支上的成員可以繼續原本的工作，而bug修復工作也同時由另外的成員來執行，使得對彼此間的影響降到最低。

開修補程式分支詳細步驟：

1. 開「修補程式」分支 (hotfix branch) ::

		# 從主要分支開一支名為「hotfix-1.2.1」的分支，開完後切換到hotfix-1.2.1分支。
		$ git checkout -b hotfix-1.2.1 master
		
2. 制定版本號 ::
		
		# commit 一個版本，commit 訊息為「版本號跳躍至1.2.1」
		$ git commit -a -m "Bumped version number to 1.2.1"
		
3. 修正bug並commit一版 ::

		$ git commit -m "Fixed severe production problem"
		
4. 將修正完bug的修補程式分支合併回主分支 ::

		# 切換至主要分支
		$ git checkout master
		
		# 將hotfix-1.2.1分支合併到主要分支
		$ git merge --no-ff hotfix-1.2.1
		
		# 上tag
		$ git tag -a 1.2.1
		
5. 將修復完成的修補程式分支合併回開發分支 ::

		# 切換至開發分支
		$ git checkout develop
		
		# 將hotfix-1.2.1分支合併回開發分支
		$ git merge --no-ff hotfix-1.2.1
		
需要特別注意的是，若修補程式分支與發佈分支同時存在。則當bug修補已完成時，就不是合併回開發分支，而是發佈分支。修補程式就會在從未來發佈分支合併回開發分支時一併將bug修補完。

6. 刪除hotfix-1.2.1分支 ::

		$ git branch -d hotfix-1.2.1


以上就是 Git flow 分支策略的詳細說明。當然，本文所解說的也只是一種策略的框架，不一定要百分之百依循，也可以隨自己的喜好化用出不同的策略模型。
