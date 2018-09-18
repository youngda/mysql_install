# centos7 Mysql 安装

* 版本选择
mysql yum [仓库地址](http://dev.mysql.com/downloads/repo/yum/)选择合适的版本。
如下： 
> wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm 
* 下载后执行：
> sudo rpm -Uvh mysql80-community-release-el7-1.noarch.rpm
* 安装：
> sudo yum install -y mysql-community-server
* 启动：
> sudo systemctl start mysqld.service
* 查看状态：
> sudo service mysqld status 或 sudo systemctl status mysqld.service
* 获取密码：
> sudo grep 'temporary password' /var/log/mysqld.log
* 修改密码：
> mysql -uroot -p
> ALTER USER 'root'@'localhost' IDENTIFIED BY 'SSssssss22222.';
> 必须含有大写、小写字母、数字、特殊字符

* php 编译

>>  yum -y install readline-devel libpng libpng-devel curl curl-devel bzip2 bzip2-devel  libxml2-devel libxml2

>> ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-opcache --enable-fpm --with-fpm-user=www --with-fpm-group=www --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-gettext --enable-mbstring --with-iconv --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-zlib --enable-zip --with-bz2 --with-readline --with-gd

>> php 报错 ERROR: [pool www] cannot get uid for user 'www'

>> groupadd www-data

>> useradd -g www-data www-data

* mysql 备份

>> mysql 导出 mysqldump -u root -p databasename | gzip > filename_to_compress.sql.gz

>> mysql 导入 gunzip < filename_to_compress.sql.gz  | mysql -u root -pPassWord databasename 
