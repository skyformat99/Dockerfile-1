Docker Official
===============

docker 官方映像获取和使用配置



## PostgreSQL ##
*运行PostgreSQL容器*

```sh
# 获取映像
docker pull library/postgres:9.6.1
# 创建并启动
docker run --name postgres \
-p 5432:5432 \
-v /data/run/docker/postgres/data:/var/lib/postgresql/data:rw \
-e POSTGRES_PASSWORD=123456 -d postgres:9.6.1
```

## Percona(MySQL) ##
*运行Percona(MySQL)容器*

```sh
# 获取映像
docker pull library/percona:5.7.16
# 创建并启动
docker run --name percona \
-p 3306:3306 \
-v /data/run/docker/percona/data:/var/lib/mysql:rw \
-e MYSQL_ROOT_PASSWORD=123456 -d percona:5.7.16
```

## Tomcat ##
*运行Tomcat容器*

```sh
# 获取映像
docker pull library/tomcat:8-jre8
# 创建并启动
docker run --name tomcat8_jre8 \
-p 8080:8080 \
-v /data/run/docker/tomcat8_jre8/conf/server.xml:/usr/local/tomcat/conf/server.xml \
-v /data/run/docker/tomcat8_jre8/webapps:/usr/local/tomcat/webapps:rw \
-d tomcat:8-jre8
```

*Tomcat主机配置*

```sh
<Engine name="Catalina">
      <Host name="openeasy.net" appBase="webapps" autodeploy="true" unpackWARs="true">
        <Context path="" docBase="openeasy"  debug="0" reloadable="true" />
      </Host>

</Engine>
```

*Tomcat设置JAVA参数*

```sh
JAVA_OPTS="-server  -Dfile.encoding=UTF-8 -Duser.timezone=GMT+08 -Xss2m -Xms1024m -Xmx1024m"
```

## Redis ##
*运行Redis容器*

```sh
# 获取映像
docker pull redis:3.2.6
# 创建并启动
docker run --name redis \
-p 6379:6379 \
-v /data/run/docker/redis/data:/data:rw \
-d redis:3.2.6 redis-server --appendonly yes
```