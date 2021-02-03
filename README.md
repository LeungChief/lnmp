## 软件版本说明
1. nginx1.18.0 stable version
2. php7.3.24
3. mysql8.0 MySQL Community Server - GPL
4. redis4.0.6
5. swoole 4.6.1

## 使用说明
#### 1. 拉取镜像
`docker pull cheifleung/lnmp`
#### 2. 运行
    docker run -dit \
    -p 3306:3306 -p 80:80  -p 6379:6379 -p 9000:9000 \
    -v /usr/local/nginx_conf:/nginx_conf \
    -v /usr/local/mysql_data:/mysql_data \
    -v /usr/local/www:/www \
    --privileged=true --name=lnmp \
    cheifleung/lnmp:1.0.0


#### 3. 进入容器
`docker exec -it 容器ID bash `

#### 4. 初始化软件

    # 获取Mysql初始密码
	cat /var/log/mysqld.log | grep password 
    mysql -uroot -p初始密码 
    # 修改密码策略
    set global validate_password.policy=0;
    set global validate_password.length=1;
    ALTER USER "root"@"localhost" IDENTIFIED  BY "你的密码";
	# 配置远程访问（这里建议新建一个账号）
	# 切换数据库
	use mysql;
	create user  'admin'@'%' identified by 'Admin123';
	grant all on *.* to 'admin'@'%' ;
	flush privileges;
	# 修改admin认证模式及密码
	ALTER USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY 'Admin123';
	flush privileges;
	
	# 重启mysql
	systemctl restart mysqld
	
	# 到这里mysql配置完毕

#### 查看状态
    systemctl status nginx
    systemctl status mysqld
    systemctl status php-fpm
    systemctl status redis
    
#### 挂载目录

`/www web项目存放的目录
/mysql_data 数据库存放的目录
/nginx_conf nginx扩展配置文件存放目录
`