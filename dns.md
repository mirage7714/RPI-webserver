##有關DNS服務:

目前使用no-ip提供的dns

1-1 安裝no-ip duc(使用root)

```
cd /usr/local/src
wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
tar -xvf noip-duc-linux-tar.gz
cd noip-2.1.9-1/
make install
```

中間會要求輸入帳號密碼還有設定更新期間，建議改為五分鐘

1-2 建立查詢外部IP檔案

為了避免IP沒有更新造成無法連上伺服器，在這邊建立一個shell script方便之後用crontab進行排成

`sudo nano ipcheck.sh`

加入以下內容:

```
date > ip.txt
curl icanhazip.com >> ip.txt
```

存檔後修改權限，再測試執行

```
sudo chmod 777 ipcheck.sh
bash ipcheck.sh
cat ip.txt
```
如果有資料就完成了

1-3自動上傳外部IP到dropbox當備份

先下載dropbox-upload-master(https://github.com/andreafabrizi/Dropbox-Uploader)

用ftp傳到需要上傳IP資料的主機

先更改dropbox-uploader.sh的權限，再執行dropbox-uploader.sh
```
chmod 777 dropbox-uploader.sh
bash dropbox-uploader.sh
```
這邊要輸入app key還有app secret 要從dropbox developer console那邊取得

最後是確認dropbox讀取的資料範圍

測試上傳檔案

`./dropbox-uploader.sh upload ip.txt /`

1-4 將查詢IP跟上傳建立排程

將原本的ipcheck.sh加入最後一行

`./dropbox-uploader.sh upload ip.txt /`

建立排程

`crontab -e`

定時每小時上傳

`0 * * * * bash ipcheck.sh`

檢查是否有建立排程，有剛剛的資料就完成

`crontab -l`


