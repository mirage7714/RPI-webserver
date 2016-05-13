測試在Raspberry pi 3 上安裝LAMP(Linux, Apache2, MySQL, PHP)，設置的步驟如下：
1.先更新一下系統軟體
sudo apt-get update
2. 安裝apache2 server還有php
sudo apt-get install apache2 php5 libapache2-mod-php5
3. 安裝MySQL
sudo apt-get install mysql-server mysql-client php5-mysql
過程中會詢問MySQL管理員root的密碼(要輸入兩次)
4. 測試用root帳號登入MySQL
mysql -u root -p
5. 安裝phpmyadmin
sudo apt-get install phpmyadmin
過程中會詢問要不要設定dbconfig-common，選yes之後會詢問MySQL的管理員密碼，輸入之後就可以安裝好了
6. 將phpmyadmin加入apache2服務中
sudo nano /etc/apache2/apache2.conf
在最後面加入下面這行：
Include /etc/myadmin/apache.conf
7.重新啟動apache2服務
sudo /etc/init.d/apache2 restart
8. 更改放網頁的/var/www權限
sudo chown -R pi /var/www
更改首頁路徑：
1.在/var/www/html內先建立.htaccess檔案
nano /var/www/html/.htaccess
DirectoryIndex index.html
更多詳細設定參考以下網頁：
http://support.unethost.com/knowledgebase.php?action=displayarticle&id=29
2. 更改apache2設定檔
sudo nano /etc/apache2/apache2.conf
加入以下幾行：
<Directory /var/www/html>
AllowOverride All
</Directory>
3.重新啟動apache2服務
sudo service apache2 restart

架設LEMP(Linux, Nginx, MySQL, PHP)
1.架設Niginx(用root)
apt-get install nginx
打開http://IP or DNS確認能不能看到Nginx首頁
2.安裝php5
apt-get install php5-fpm php-mysql
設定php5-fpm
nano /etc/php5/fpm/php.ini
cgi.fix_pathinfo=0
3.設定nginx.conf
nano /etc/nginx/nginx.conf
worker_process 4;
keepalive_timeout 2;
nano /etc/nginx/sites-available/default
找到server{ 把註解都拿掉


有關DNS服務:
目前使用no-ip提供的dns
1.安裝no-ip duc(使用root)
cd /usr/local/src
wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
tar -xvf noip-duc-linux-tar.gz
cd noip-2.1.9-1/
make install
中間會要求輸入帳號密碼還有設定更新期間，建議改為五分鐘
2.建立查詢外部IP檔案
建立shell script方便之後用crontab進行排成
sudo nano ipcheck.sh
加入以下內容:
date > ip.txt
curl icanhazip.com >> ip.txt
存檔後修改權限
sudo chmod 777 ipcheck.sh
測試執行
bash ipcheck.sh
cat ip.txt
如果有資料就完成了
3.自動上傳外部IP到dropbox當備份
先下載dropbox-upload-master(https://github.com/andreafabrizi/Dropbox-Uploader)
用ftp傳到需要上傳IP資料的主機
先更改dropbox-uploader.sh的權限
chmod 777 dropbox-uploader.sh 
執行dropbox-uploader.sh
bash dropbox-uploader.sh
這邊要輸入app key還有app secret 要從dropbox developer console那邊取得
最後是確認dropbox讀取的資料範圍
測試上傳檔案
./dropbox-uploader.sh upload ip.txt /
4.將查詢IP跟上傳建立排程
將原本的ipcheck.sh加入最後一行
./dropbox-uploader.sh upload ip.txt /
建立排程
crontab -e
定時每天12點上傳
0 12 * * * bash ipcheck.sh
檢查是否有建立排程
crontab -l
有剛剛的資料就完成




