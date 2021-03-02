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
```

## 4 表authorization 需要插入数据(app_key and app_secret)