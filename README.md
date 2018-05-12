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
