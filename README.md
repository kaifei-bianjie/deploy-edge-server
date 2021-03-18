# 部署edge-server

## 1 查看当前项目的 edge-server.jar

## 2 服务器上安装 open-jdk11

## 3 运行启动命令

my.yml：外部的springboot配置文件

```shell
java -jar edge-server.jar --spring.config.additional-location=/Users/zaizai/deploy/my.yml 
```

外部配置文件e.g

```yaml
server:
  port: 8088

# 配置数据库连接
spring:
  jpa:
    hibernate:
      ddl-auto: update
  datasource:
    url: jdbc:mysql://localhost:3306/edge-server?useSSL=false
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

# 配置irita-sdk
irita:
  sdk:
    caKeystore: /Users/zaizai/deploy/cb.JKS  # hash管理员ca私钥的绝对路径
    password: xxx
    opbUri: xxx
    chainId: xxx
    projectId: xxx
    projectKey: xxx
    contractAddr: xxx

# pdf图片插入的位置
pdf:
  mosaic:
    location: 0

# 摘要过期时间
digest:
  expireTime: 60000 #60000: 1min 6000000: 100min

qrcode: # 二维码的长和宽，单位（px）不用填
  width: 200
  height: 200
```

## 4 表authorization 需要插入数据(app_key and app_secret)

e.g 插入一行 app_key=root,app_secret=123456

```sql
INSERT INTO authorization (app_key, app_secret, create_at, status, update_at)
VALUES ('test', '123456', null, null, null);
```

# 使用 docker 部署

```Dockerfile
FROM openjdk:11

ARG environment
ENV springEnv = $environment

ADD ./target/edge-server-0.1.jar edge-server.jar
ENTRYPOINT java -jar edge-server.jar $springEnv
```

编写docker-compose.yml，当然也可以使用docker run 加参数运行

```yaml
version: "3.7"

services:
  edge-server:
    build:
      context: ./
      args:
        environment: --spring.config.additional-location=/app/my.yml
    ports:
      - 8080:8080
    volumes:
      - /Users/zaizai/deploy:/app
```

my.yml 如下

```yaml
server:
  port: 8088

# 配置数据库连接
spring:
  jpa:
    hibernate:
      ddl-auto: update
  datasource:
    url: jdbc:mysql://xxxx:3306/edge-server?useSSL=false
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

# 配置irita-sdk
irita:
  sdk:
    caKeystore: app/cb.JKS
    password: xxx
    opbUri: xxx
    chainId: xxx
    projectId: xxx
    projectKey: xxx
    contractAddr: xxx

# pdf图片插入的位置
pdf:
  mosaic:
    location: 0

# 摘要过期时间
digest:
  expireTime: 60000 #60000: 1min 6000000: 100min

qrcode: # 二维码的长和宽，单位（px）不用填
  width: 200
  height: 200
```