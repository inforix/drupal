本案例的有关设定：

**OS**：CentOS 7.1 x64

**Drupal Root**：/var/www/html/

**数据库**： 系统自带MariaDB

**WEB服务器**：Apache，不适用virtual hosts，直接配置DocumentRoot为Drupal Root

**Drupal Version**：8.x Latest Version

# 1. CentOS 7.1操作系统安装
建议最小化安装64位CentOS 7.1 操作系统，同时选择_Development Tools_工具包。
安装完成后，运行
```bash
yum -y install wget open-vm-tools
yum -y update
```

# 2. 配置防火墙
考虑到系统的安全性，防火墙不能关闭，必须按照要求配置。
配置命令如下：
```bash
firewall-cmd --permanent --add-port=http/tcp
firewall-cmd --reload
```

# 3. 配置SELINUX
修改SELINUX为permissive模式。

```bash
sed -i 's/SELINUX=enforcing/SELINUX=permissive/g'  /etc/selinux/config
```

修改该配置需要重启操作系统。也可以暂时运行一下命令生效：

```bash
setenforce 0
```
