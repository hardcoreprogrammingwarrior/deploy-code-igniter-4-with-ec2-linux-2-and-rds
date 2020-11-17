
# How to Speedrun Deploy  CodeIgniter 4 with ec2 Linux 2 and RDS 
Watch it here   
CodeIgniter v4.0.4 (PHP v7.4.11) Mysql 8.0  
Since Nov 17, 2020  

---  
```sh
$ sudo yum update -y  
$ sudo amazon-linux-extras enable php7.4 -y  
$ sudo yum clean metadata  
$ sudo yum install  -y php php-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}  
$ sudo yum install  -y git  
$ curl -sS https://getcomposer.org/installer | php  
$ sudo  mv composer.phar /usr/bin/composer  
$ chmod +x /usr/bin/composer  

$ php --version  
```
**Dependencies installation will take some time. After than set proper permissions on files.**  
```sh
$ sudo chown -R ec2-user:apache /var/www  
$ sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;  
$ find /var/www -type f -exec sudo chmod 0664 {} \;  

$ cd /var/www/html  
$ composer create-project codeigniter4/appstarter speedrun
$ cd /var/www/html/speedrun  
$ composer install  
```

**Codeigniter configurations.**  
```sh
$ cp env .env 
$ vim .env
enable CI_ENVIRONMENT = development
$ sudo chown ec2-user:apache -R writable/  
```

**Database configurations.**  
```sh
$ vim .env
enable database.default
#--------------------------------------------------------------------
# DATABASE
#--------------------------------------------------------------------

database.default.hostname = rds.ipaddress.host
database.default.database = ci4
database.default.username = root
database.default.password = root
database.default.DBDriver = MySQL
```

**Set the host:**  
```sh
$ sudo vim /etc/httpd/conf/httpd.conf   
```

**Add the File bottom**  

```blade
<VirtualHost *:80>  
	ServerName codeigniter.example.com  
	DocumentRoot /var/www/html/speedrun/public  
	<Directory /var/www/html/speedrun>  
		AllowOverride All  
	</Directory>  
</VirtualHost>  
```  

```sh
$ sudo systemctl start httpd  
$ sudo systemctl start php-fpm  
$ sudo systemctl enable php-fpm  
```
