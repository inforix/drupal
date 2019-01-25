
# 1. 安装 PHP 7.2

1. 首先从webtatic网站安装预编译好的PHP 7.2。
```bash
yum -y install epel-release
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum -y install php72w php72w-opcache php72w-mbstring php72w-gd php72w-xml php72w-pear  php72w-mysql php72w-pdo mod_php72w
```

# 2. 安装 Composer

`建议直接访问 https://getcomposer.org/download 获取以下代码的最新版。`

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --install-dir=/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

# 3. 安装 Apache HTTPD 服务
```bash
yum -y install httpd mod_ssl
systemctl enable httpd
systemctl start httpd
```

配置防火墙
```bash
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
```

# 4. 安装并配置 Mariadb 
```bash
yum -y install mariadb-server mariadb
systemctl enable mariadb
systemctl start mariadb
```

安装完成后，运行 mysql_secure_installation 进行安全处理。
所有提示均选择默认的Y。

# 5. 创建站点数据库

```bash
mysqladmin -p create drupal
mysql -u root -p drupal
grant all privileges on drupal.* to drupal@localhost identified by 'PASSWORD';
flush privileges;
exit
```

# 6. 安装 Drupal 8
```bash
cd /var/www/html
chcon -R -t httpd_sys_content_rw_t /var/www/html/
composer create-project drupal-composer/drupal-project:8.x-dev ./ --stability dev --no-interaction
mkdir -p config/sync
chown -R apache:apache *
```

# 7. 修改httpd.conf配置文件

修改默认web路径为 /var/www/html/web/
修改允许覆盖
```ini
AllowOverride = All
```

```bash
systemctl restart httpd
```

# 8. 访问站点进行安装。