---
title: ContOS7 + Nginx1.2 + PHP7.1 + MySQL5.7
date: '2017/08/15 17:00:00'
tag: 'Linux'
---

## 环境准备 ##
```
    # 设置ssh登录服务器
    ssh root@你的ip # 然后输入密码登录
    tail ~/.ssh/id_rsa.pub  # 本机运行，拿到你的公钥
    vi authorized_keys # 服务器端运行，把上一步得到的公钥添加到此文件
```

## 常用命令 ##
```
    # 查看已启动的服务列表
    systemctl list-unit-files|grep enabled 

    # Nginx stop|restart
    sudo systemctl start nginx

    # Mysql stop|restart
    sudo systemctl start mysqld

    # 启动php-fpm stop|restart
    sudo systemctl start php-fpm
```

## 添加CentOS用户 ##
```
    useradd username # 添加用户
    passwd username # 更改用户密码
    visudo # 用vi打开/etc/sudoers
    username ALL=(ALL:ALL) ALL # 找到 root    ALL=(ALL:ALL) ALL ，下面添加此行内容
```


## 更新资源包 ##
```
    yum upgrade  # 使用root权限更新资源包
    su username # 切换新建的CentOS用户
    sudo yum -y install git # 安装Git
```

## 安装Nginx ##
```
    # 新建文件，写入如下五行代码。 安装新的Nginx源
    sudo vi /etc/yum.repos.d/nginx.repo 

    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/7/$basearch/
    gpgcheck=0
    enabled=1

    # 保存后nginx.repo后执行
    sudo yum install -y nginx   # 安装Nginx
    sudo systemctl start nginx  # 启动nginx
    sudo systemctl enable nginx # 使用systemctl设置开机启动
```

## 安装PHP ##
```
    # 安装PHP源
    sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

    # 安装PHP和PHP-FPM
    sudo yum install -y php71w php71w-cli php71w-fpm

    # 安装相关扩展，具体根据自己实际情况安装
    sudo yum -y install php71w-mbstring php71w-common
    sudo yum -y install php71w-gd php71w-mcrypt
    sudo yum -y install php71w-mysql php71w-xml
    sudo yum -y install php71w-soap php71w-xmlrpc

    # 隐藏PHP版本号
    sudo vi /etc/php.ini
    expose_php = On 改为 Off
```

## 安装MySQL5.7 ##
```
    # 安装MySQL源
    sudo yum install -y https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

    # 安装MySQL
    sudo yum install -y mysql-community-server

    # 启动MySQL
    sudo systemctl start mysqld

    # 设置开机启动
    sudo systemctl enable mysqld

    # 找到随机生成的密码
    vi /var/log/mysqld.log # 找到 A temporary password is generated for root@localhost: cS3.8,,)-y5=

    # 执行下面的命令，设置MySQL(需要上面的密码)
    mysql_secure_installation # 配置 MySQL
```

## 启动PHP-FPM ##
```
    # 启动PHP-FPM
    sudo systemctl start php-fpm
    # 设置开机启动
    sudo systemctl enable php-fpm
```

摘自：https://blog.11010.net/index.php/archives/28/