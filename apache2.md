#測試在Raspberry pi 3 上安裝LAMP(Linux, Apache2, MySQL, PHP)，設置的步驟如下：
 
 1-1 先更新一下系統軟體

`sudo apt-get update`

 1-2 安裝apache2 server還有php

`sudo apt-get install apache2 php5 libapache2-mod-php5`

 1-3 安裝MySQL
 
`sudo apt-get install mysql-server mysql-client php5-mysql`

過程中會詢問MySQL管理員root的密碼(要輸入兩次)

 1-4 測試用root帳號登入MySQL
 
`mysql -u root -p`

 1-5 安裝phpmyadmin
 
`sudo apt-get install phpmyadmin`

過程中會詢問要不要設定dbconfig-common，選yes之後會詢問MySQL的管理員密碼，輸入之後就可以安裝好了

1-6 將phpmyadmin加入apache2服務中

`sudo nano /etc/apache2/apache2.conf`

在最後面加入下面這行：
`Include /etc/myadmin/apache.conf`

1-7 重新啟動apache2服務

`sudo /etc/init.d/apache2 restart`

1-8 更改放網頁的/var/www權限

`sudo chown -R pi /var/www`

###更改首頁路徑：
2-1在/var/www/html內先建立.htaccess檔案

`nano /var/www/html/.htaccess`

在.htaccess內加入:

`DirectoryIndex index.html`

更多詳細設定參考以下網頁：
http://support.unethost.com/knowledgebase.php?action=displayarticle&id=29

2-2 更改apache2設定檔

`sudo nano /etc/apache2/apache2.conf`

加入以下幾行：
```
<Directory /var/www/html>
AllowOverride All
</Directory>
```
2-3 重新啟動apache2服務

`sudo service apache2 restart`




