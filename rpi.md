���զbRaspberry pi 3 �W�w��LAMP(Linux, Apache2, MySQL, PHP)�A�]�m���B�J�p�U�G
1.����s�@�U�t�γn��
sudo apt-get update
2. �w��apache2 server�٦�php
sudo apt-get install apache2 php5 libapache2-mod-php5
3. �w��MySQL
sudo apt-get install mysql-server mysql-client php5-mysql
�L�{���|�߰�MySQL�޲z��root���K�X(�n��J�⦸)
4. ���ե�root�b���n�JMySQL
mysql -u root -p
5. �w��phpmyadmin
sudo apt-get install phpmyadmin
�L�{���|�߰ݭn���n�]�wdbconfig-common�A��yes����|�߰�MySQL���޲z���K�X�A��J����N�i�H�w�˦n�F
6. �Nphpmyadmin�[�Japache2�A�Ȥ�
sudo nano /etc/apache2/apache2.conf
�b�̫᭱�[�J�U���o��G
Include /etc/myadmin/apache.conf
7.���s�Ұ�apache2�A��
sudo /etc/init.d/apache2 restart
8. ���������/var/www�v��
sudo chown -R pi /var/www
��ﭺ�����|�G
1.�b/var/www/html�����إ�.htaccess�ɮ�
nano /var/www/html/.htaccess
DirectoryIndex index.html
��h�Բӳ]�w�ѦҥH�U�����G
http://support.unethost.com/knowledgebase.php?action=displayarticle&id=29
2. ���apache2�]�w��
sudo nano /etc/apache2/apache2.conf
�[�J�H�U�X��G
<Directory /var/www/html>
AllowOverride All
</Directory>
3.���s�Ұ�apache2�A��
sudo service apache2 restart

�[�]LEMP(Linux, Nginx, MySQL, PHP)
1.�[�]Niginx(��root)
apt-get install nginx
���}http://IP or DNS�T�{�ण��ݨ�Nginx����
2.�w��php5
apt-get install php5-fpm php-mysql
�]�wphp5-fpm
nano /etc/php5/fpm/php.ini
cgi.fix_pathinfo=0
3.�]�wnginx.conf
nano /etc/nginx/nginx.conf
worker_process 4;
keepalive_timeout 2;
nano /etc/nginx/sites-available/default
���server{ ����ѳ�����


����DNS�A��:
�ثe�ϥ�no-ip���Ѫ�dns
1.�w��no-ip duc(�ϥ�root)
cd /usr/local/src
wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
tar -xvf noip-duc-linux-tar.gz
cd noip-2.1.9-1/
make install
�����|�n�D��J�b���K�X�٦��]�w��s�����A��ĳ�אּ������
2.�إ߬d�ߥ~��IP�ɮ�
�إ�shell script��K�����crontab�i��Ʀ�
sudo nano ipcheck.sh
�[�J�H�U���e:
date > ip.txt
curl icanhazip.com >> ip.txt
�s�ɫ�ק��v��
sudo chmod 777 ipcheck.sh
���հ���
bash ipcheck.sh
cat ip.txt
�p�G����ƴN�����F
3.�۰ʤW�ǥ~��IP��dropbox��ƥ�
���U��dropbox-upload-master(https://github.com/andreafabrizi/Dropbox-Uploader)
��ftp�Ǩ�ݭn�W��IP��ƪ��D��
�����dropbox-uploader.sh���v��
chmod 777 dropbox-uploader.sh 
����dropbox-uploader.sh
bash dropbox-uploader.sh
�o��n��Japp key�٦�app secret �n�qdropbox developer console������o
�̫�O�T�{dropboxŪ������ƽd��
���դW���ɮ�
./dropbox-uploader.sh upload ip.txt /
4.�N�d��IP��W�ǫإ߱Ƶ{
�N�쥻��ipcheck.sh�[�J�̫�@��
./dropbox-uploader.sh upload ip.txt /
�إ߱Ƶ{
crontab -e
�w�ɨC��12�I�W��
0 12 * * * bash ipcheck.sh
�ˬd�O�_���إ߱Ƶ{
crontab -l
����誺��ƴN����




