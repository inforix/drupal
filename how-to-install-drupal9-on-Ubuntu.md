
# 1. 安装 PHP 7.2

```bash
sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-intl php7.2-mysql php7.2-cli php7.2-zip php7.2-curl mod_ssl
```
配置时区
```bash
sed -i 's/;date.timezone =/date.timezone = Asia\/Shanghai/g' /etc/php/7.2/apache2/php.ini
```

# 2. 安装 Composer

`建议直接访问 https://getcomposer.org/download 获取以下代码的最新版。`

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --install-dir=/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

# 3. 安装配置防火墙

```bash
sudo apt install firewalld
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

# 4. 安装并配置 Mariadb 
```bash
sudo apt-get install mariadb-server mariadb-client
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

安装完成后，运行 mysql_secure_installation 进行安全处理。
所有提示均选择默认的Y。

# 5. 创建站点数据库

```bash
sudo mysqladmin -p create drupal
sudo mysql -u root -p
mysql> grant all privileges on drupal.* to drupal@localhost identified by 'PASSWORD';
mysql> flush privileges;
mysql> exit;
```

# 6. 安装 Drupal 8
```bash
sudo composer create-project drupal-composer/drupal-project:8.x-dev /var/www/html/ --stability dev --no-interaction
mkdir -p /var/www/html/config/sync/
chown -R www-data:www-data /var/www/html/*
```

# 7. 修改000-default.conf配置文件

修改默认web路径为 /var/www/html/web/

修改允许覆盖

```ini
AllowOverride = All
```

重启Apache服务
```bash
systemctl restart apache2.service
```

# 8. 访问站点进行安装。