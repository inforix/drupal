本案例的有关设定：

**OS**：CentOS 7.x x64 或 Ubuntu 18.04

**Drupal Root**：/var/www/html/

**数据库**： 系统自带MariaDB

**WEB服务器**：Apache，不适用virtual hosts，直接配置DocumentRoot为Drupal Root

**Drupal Version**：8.x Latest Version

# 1. CentOS 7操作系统安装
建议最小化安装64位CentOS 7 操作系统，同时选择 _Development Tools_ 工具包。
安装完成后，运行
```bash
yum -y install wget
yum -y update
```

配置SELINUX
修改SELINUX为permissive模式。

```bash
sed -i 's/SELINUX=enforcing/SELINUX=permissive/g'  /etc/selinux/config
```

修改该配置需要重启操作系统。也可以暂时运行一下命令生效：

```bash
setenforce 0
```

# 2. Ubuntu 18.04操作系统安装
安装64位Ubuntu 18.04 操作系统，安装完成后，执行软件升级，运行以下命令:
```bash
sudo apt update && sudo apt upgrade
```