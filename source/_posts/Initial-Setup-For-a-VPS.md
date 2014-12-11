title: VPS极简初始化配置
date: 2014-12-11 16:55:45
tags:
    - linux
categories:
    - buzz
---
最近电信网络半瘫，一直在寻觅网速过得去的VPS。之前的DigitalOcean无论哪个节点都卡成翔了，刚换搬瓦工，一次性买了一年。好久不弄，基本的设置都忘了，有些一时没想到，有些忘记了还要问Google。记录下来，以防下回再“搬家”。
<!-- more -->
##换OS为Ubuntu
对，有些服务器商默认会给你一个CentOS，对我这种初级用户来说用啥都差不多，用Ubuntu纯碎是情怀。

##更改root密码
默认密码太难记，换成自己的比较方便，然后就SSH走起。
```
sudo passwd root
```

##更新软件源
不是所有的服务器商都会保证一个up-to-date的镜像的，搬瓦工就是！
```
sudo apt-get update
```

##安装Shadowsocks
[Shadowsocks on Github](https://github.com/clowwindy/shadowsocks)
Across the Great Firewall, we can reach every corner in the world.
祝病魔早日战胜方校长。

##安装LAMP，开启必备组件
某些服务商会在新建的时候提供组件选择，比如DigitalOcean。没有的话，就要自己安装了：
```
sudo apt-get install lamp-server^
```
以上默认不开mod_rewrite，要自己开启：
```
sudo a2enmod rewrite
```
另外还要安装下Mcrypt，比如要连接微信公众平台就是需要此组件：
```
sudo apt-get install php5-mcrypt
```
重启apache生效：
```
sudo service apache2 restart
```

##设置apache虚拟主机
1、复制一个站点配置文件
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```
2、编辑新站点配置文件
```
sudo vi /etc/apache2/sites-available/example.com.conf
```
修改配置信息：
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/html/example
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
启用新站点：
```
sudo a2ensite example.com.conf```

重启apache生效：
```
sudo service apache2 restart
```

##屏蔽根目录
有时候只是放了个二级域名而已，根目录不喜欢被访问，只想放条信息，修改000-default.conf：
```
sudo vi /etc/apache2/sites-available/000-default.conf
```
添加一行：
```
ServerName 服务器的IP地址
```

配置完毕，够用就行。