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

```
 yum -y install readline-devel libpng libpng-devel curl curl-devel bzip2 bzip2-devel  libxml2-devel libxml2 openssl openssl-devel gcc
```

 ```
 ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-opcache --enable-fpm --with-fpm-user=www --with-fpm-group=www --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-gettext --enable-mbstring --with-iconv --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-zlib --enable-zip --with-bz2 --with-readline --with-gd
```
>> php 报错 ERROR: [pool www] cannot get uid for user 'www'

```
cd /usr/local/php/etc/php-fpm.d/

mv www.conf.default www.conf

groupadd www

useradd -g www-data www
```


>> configure: error: Please reinstall the libzip distribution

#解决方法：
```
wget https://nih.at/libzip/libzip-1.2.0.tar.gz
tar -zxvf libzip-1.2.0.tar.gz
cd libzip-1.2.0
./configure
make && make install
```

>> configure: error: off_t undefined; check your library configuration

```
#解决方法
# 添加搜索路径到配置文件
echo '/usr/local/lib64
/usr/local/lib
/usr/lib
/usr/lib64'>>/etc/ld.so.conf
# 更新配置
ldconfig -v

```

>> /usr/local/include/zip.h:59:21: fatal error: zipconf.h: No such file or directory

```
#解决方法：手动复制过去
cp /usr/local/lib/libzip/include/zipconf.h /usr/local/include/zipconf.h
```

>> 编译程序时，如果出现类似virtual memory exhausted: Cannot allocate memory的错误时，可以用下面的方法解决。


```
创建swap挂载点

# mkdir /opt/images/

# rm -rf /opt/images/swap

设置挂载swap的大小，64M*32=2GB

# dd if=/dev/zero of=/opt/images/swap bs=64M count=32

# mkswap /opt/images/swap
开启swap

# swapon /opt/images/swap
这个时候，可以执行之前内存不足时的命令了，正常情况下，执行时间会比较长，但是能过去

最后，可以考虑关闭swap并删除挂载文件

# swapoff swap
# rm -f /opt/images/swap

```


```
cp php.ini-development /usr/local/php/etc/php.ini

cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm


service php-fpm start

```
添加 PHP 命令到环境变量
编辑 ~/.bash_profile，将：
```

添加：
PATH=$PATH:$HOME/bin:/usr/local/php/bin
```
配置生效：
```
source ~/.bash_profile
```
* mysql 备份

>> mysql 导出 mysqldump -u root -p databasename | gzip > filename_to_compress.sql.gz

>> mysql 导入 gunzip < filename_to_compress.sql.gz  | mysql -u root -pPassWord databasename 


# nginx 最新版

``` 
vim /etc/yum.repos.d/nginx.repo
```

```python
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
```
```
yum update
```

