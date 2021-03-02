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
    caKeystore: /Users/zaizai/deploy/cb.JKS  # ca私钥的绝对路径
    password: 123456
    opbUri: https://opbningxia.bsngate.com:18602
    chainId: wenchangchain
    projectId: 7b3c53beda5c48c6b07d98804e156389
    projectKey: 7a3b5660c0ae47e2be4f309050c1d304
    contractAddr: iaa1cr8ard7tpvzf3g8n5llegc0fd92uuxeeuzt4s6
  
# pdf图片插入的位置
pdf:
  mosaic:
    location: 0
  
# 摘要过期时间
digest:
  expireTime: 60000 #60000: 1min 6000000: 100min
```

## 4 表authorization 需要插入数据(app_key and app_secret)