由于现有的Drupal 8.x是用PHP 5版本，而我们安装的是PHP 7.x，所以需要修改 .htaccess 文件。

```bash
vim .htaccess
```

```ini
<IfModule mod_php5.c>
```

改为

```ini
<IfModule mod_php7.c>
```

尤其是以下几个文件需要修改

* .htaccess
* sites/default/files/.htaccess