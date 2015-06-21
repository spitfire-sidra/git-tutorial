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
