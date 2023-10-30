## 1 MYSQL
    安装MYSQL并配置字符集 安装完成后创建数据库wordpress
    yum install -y wget
    wget https://repo.mysql.com//mysql80-community-release-el7-10.noarch.rpm
    yum install -y yum-utils
    yum localinstall -y mysql80-community-release-el7-10.noarch.rpm 
    yum-config-manager --enable mysql57-community
    yum-config-manager --disable mysql80-community
    yum repolist enabled | grep mysql
    yum install -y mysql-community-server
    systemctl start mysqld
    systemctl status mysqld
    ss -natl |grep 3306
## 2 防火墙
    防火墙开放端口
    firewall-cmd --permanent --add-port=3306/tcp
    firewall-cmd --permanent --add-port=443/tcp
    firewall-cmd --permanent --add-port=80/tcp
    firewall-cmd --reload
    firewall-cmd --list-all
    grep "password" /var/log/mysqld.log 
    mysql -uroot -p'xeRZv7EBfH,s'
    vi /etc/my.cnf
    systemctl status mysqld
## 3 关闭selinux
    vi /etc/selinux/config 
    reboot 
    getenforce 
## 4 NGINX
* ### 4.1 安装
        vi /etc/yum.repos.d/nginx.repo
        yum-config-manager --enable nginx-stable
        yum install -y  nginx
        systemctl start nginx
        systemctl enable nginx
        systemctl status nginx
* ###  4.2  配置NGINX PHP
        location / {
            root   /usr/share/nginx/html;
        #增加index.php
            index  index.php index.html index.htm;
        }
        #启用PHP
        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  /usr/share/nginx/html/$fastcgi_script_name;
            include        fastcgi_params;
        }
## 5 安装PHP7.4 
    yum install -y epel-release
    yum --enablerepo = remi install php74-php
    rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
    yum --enablerepo=remi install php74-php
    yum --enablerepo=remi install php74-php php74-php-gd php74-php-xml php74-php-sockets php74-php-session php74-php-snmp php74-php-mysql php74-php-fpm
    php74 -v
    #启动php-fpm
    systemctl start php74-php-fpm
    systemctl status php74-php-fpm
    systemctl enable php74-php-fpm
    #修改用户和组为nobody
    vi /etc/opt/remi/php74/php-fpm.d/www.conf 

    #修改内容
    user = nobody
    group = nobody

    #查看端口
    ss -natl |grep 9000
    ps aux |grep php-fpm
## 6 运行向导
    网站首页https://domain或者https://domain/readme.html里面有安装向导连接

