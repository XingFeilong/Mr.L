# Zabbix+[**grafana-zabbix**](https://github.com/alexanderzobnin/grafana-zabbix)+[**grafana**](https://github.com/grafana/grafana)组合

zabbix web默认用户名:Admin    密码:zabbix

配置文件目录: /usr/share/zabbix/include

# 配置：

Zabbix完整的监控配置流程可以简单描述为：

**Host groups（主机组）→Hosts（主机）→Applications（监控项组）→Items（监控项）→Triggers（触发器）→Event（事件）→Actions（处理动作）→User groups（用户组）→Users（用户）→Medias（告警方式）→Audit（日志审计）**

# 安装：

## For Debian / Ubuntu

## Supported versions

* Debian 7 \(codename: wheezy\)

* Debian 8 \(codename: jessie\)

* Ubuntu 14.04 LTS \(codename: trusty\)

### 1. Installing repository configuration package

```
wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+trusty_all.deb

 sudo dpkg --install zabbix-release_3.2-1+trusty_all.deb

 sudo apt-get update
```

### 2. Installing packages

```
- apt-get install zabbix-server-mysql zabbix-frontend-php

 // Administrator 'root' password: 26554422
```

### 3. Creating initial database

```
- zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uroot -p26554422
```

OR

参考 [https://www.zabbix.com/documentation/3.2/manual/appendix/install/db\_scripts](https://www.zabbix.com/documentation/3.2/manual/appendix/install/db_scripts)

下载sql文件 [http://www.zabbix.com/developers.php](http://www.zabbix.com/developers.php)

```
shell> mysql -uroot -p<password>

mysql> create database zabbix character set utf8 collate utf8_bin;

mysql> grant all privileges on zabbix.* to zabbix@localhost identified by '26554422';

mysql> quit;

shell> cd database/mysql

shell> mysql -uzabbix -p<password> zabbix < schema.sql

# stop here if you are creating database for Zabbix proxy

shell> mysql -uzabbix -p<password> zabbix < images.sql

shell> mysql -uzabbix -p<password> zabbix < data.sql
```

### 4. Common configuration

##### 1. Database configuration for Zabbix server

```
>> vi /etc/zabbix/zabbix_server.conf



 DBHost=localhost

 DBName=zabbix

 DBUser=zabbix

 DBPassword=26554422
```

##### 2. PHP configuration for Zabbix frontend

> NOTE: /etc/apache2/conf.d/zabbix.conf and /etc/zabbix/apache.conf are the same file.

```
DBHost=localhost

DBName=zabbix

DBUser=zabbix

DBPassword=zabbix
```

### 5. Starting Zabbix server process

* After database is installed and zabbix\_server.conf file is configured, you may start Zabbix server process.

```
>> service zabbix-server start
```

* As frontend configuration is done, you need to restart Apache web server.

```
>> service apache2 restart
```

### 6. Install Agent

```
sudo apt-get -y install zabbix-agent

//Now agent is ready to be started by:

 sudo service zabbix-agent start
```

---

# Q&A

### 一. 错误描述

"You are not able to choose some of the languages, because locales for them are not installed on the web server."

### 解决办法

原因是找不到语言包

$ sudo dpkg-reconfigure locales  \#查看系统已经安装的语言包

$ sudo vim /usr/share/zabbix/include/locales.inc.php   \#找到源码文件

在文件中找到函数"getLocales\(\)"

'en\_GB' =&gt; array\('name' =&gt; \_\('English \(en\_GB\)'\),        'display' =&gt; true\),

可以把你不需要的语言设置为false,有些版本默认不支持中文,可以找到'zh\_CN'这一行把flase改为true

最后保存文件,重启apache后问题解决

---

官网：[http://www.zabbix.com/](http://www.zabbix.com/)

[Ubuntu 14.04 x64 安装 Zabbix 3.2.0 Server端+多系统Agent端部署: ](https://www.deamwork.com/archives/zabbix3.orz6)[https://www.deamwork.com/archives/zabbix3.orz6](https://www.deamwork.com/archives/zabbix3.orz6)

**Zabbix企业级分布式监控系统：**[https://github.com/itnihao/zabbix-book](https://github.com/itnihao/zabbix-book)

详解zabbix安装部署\(Server端篇\)：[http://blog.chinaunix.net/uid-25266990-id-3380929.html](http://blog.chinaunix.net/uid-25266990-id-3380929.html)

zabbix监控系统客户端安装: [http://blog.chinaunix.net/uid-25266990-id-3387002.html](http://blog.chinaunix.net/uid-25266990-id-3387002.html)

运维监控篇\(2\)\_Zabbix简单的性能调优：[https://www.xiaomastack.com/2014/10/10/zabbix02/](https://www.xiaomastack.com/2014/10/10/zabbix02/)

zabbix3.2.0实现微信企业号报警: [http://www.jianshu.com/p/09a5a21b6b47](http://www.jianshu.com/p/09a5a21b6b47)

Zabbix-3.0.3 实现微信（WeChat）告警: [http://www.oschina.net/news/75588/zabbix-3.0.3](http://www.oschina.net/news/75588/zabbix-3.0.3)

Zabbix 短信报警配置: [http://soft.dog/2016/01/19/zabbix-sms-notification/](http://soft.dog/2016/01/19/zabbix-sms-notification/)

Grafana2.6+Zabbix3.0监控系统搭建：https://docs.20150509.cn/2016/03/04/Grafana-2-6-Zabbix-3-0-%E7%9B%91%E6%8E%A7%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA/



