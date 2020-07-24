## docker-compose 一键部署zabbix-proxy,zabbix-agent
基于官方文档，增加了固定IP配置  
zabbix-server：172.16.239.4  
zabbix-web-apache-mysql:172.16.239.3  
zabbix-server-agent: 172.16.239.5  
mysql-server: 172.16.239.2

### 网络配置
zbx\_net\_backend: 内部访问使用，避免将数据库端口暴露到外网  
zbx\_net\_frontend: web外部访问使用

### 环境配置文件
路径：zabbix-server/common/env/

mysql：env_mysql  
  
```
MYSQL_ROOT_PASSWORD=rootpassword
MYSQL_DATABASE=zabbix
MYSQL_USER=zabbix
MYSQL_PASSWORD=zabbix.server
```

zabbix-server: env_server  
  
```
DB_SERVER_HOST=mysql-server  
MYSQL_DATABASE=zabbix  
MYSQL_USER=zabbix
MYSQL_PASSWORD=zabbix.server
MYSQL_ROOT_PASSWORD=rootpassword
ZBX_LOGTYPE=file
ZBX_LOG_FILE=/tmp/ser.log
```

zabbix-web-apache-mysql: env_web  
  
```
DB_SERVER_HOST=mysql-server
MYSQL_DATABASE=zabbix
MYSQL_USER=zabbix
MYSQL_PASSWORD=zabbix.server
MYSQL_ROOT_PASSWORD=rootpassword
ZBX_SERVER_HOST=zabbix-server
PHP_TZ=Asia/Shanghai
```

zabbix-server-agent: env_agent  
  
```
ZBX_SERVER_HOST=172.16.239.4
ZBX_SERVER_PORT=10051
ZBX_LISTENPORT=10050
```

### 使用方法  

```
mkdir ~/zabbix-server
cd ~/zabbix-server
git clone https://github.com/yogi401/zabbix-server-docker.git
cd zabbix-server-docker
docker-compose up -d
```

