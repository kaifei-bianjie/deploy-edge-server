# 迁移旧的合约数据到新的合约数据

## 前置操作

- 通过bsn门户网站上传新的智能合约（略，不需要操作）
- 使用 opb-sdk-java 调用刚刚上传智能合约生成部门和 HashAdmin 账号

```xml
<!-- https://mvnrepository.com/artifact/io.github.bianjieai/opb-sdk -->
<dependency>
    <groupId>io.github.bianjieai</groupId>
    <artifactId>opb-sdk</artifactId>
    <version>0.1.4</version>
</dependency>
```

- 初始化开放联盟链客户端

```java
        KeyManager km = KeyManagerFactory.createDefault();
        FileInputStream is = new FileInputStream("该ca Keystore的文件路径");
        km.recoverFromCA(is,"该ca Keystore所对应的密码");
        km.recover(mnemonic);

        String nodeUri = "nodeUri参考edge-server中的配置";
        String grpcAddr = "https://opbningxia.bsngate.com:18603";
        String chainId = "wenchangchain";
        ClientConfig clientConfig = new ClientConfig(nodeUri, grpcAddr, chainId);
        OpbConfig opbConfig = new OpbConfig("projectID", "projectKey", "");

        IritaClient client = new IritaClient(clientConfig, opbConfig, km);
        comGovClient = client.getComGovClient();
```

- 调用智能合约创建一个部门

```java
        // 使用地址为 iaa14dx54hq68tewhjngkxm6z7356lmj6rfxur4n3g 的发送交易
        final String publicKey = "iaa14dx54hq68tewhjngkxm6z7356lmj6rfxur4n3g";
        final String department = "区块链证照";

        try {
            comGovClient.addDepartment(department, publicKey, txBaseTx);
        } catch (ContractException e) {
            // you can use log to record
            e.printStackTrace();
        }
```

- 调用智能合约创建一个Hash管理员

```java
        // hash 管理员的地址，也是edge-server中从caKeystore恢复出来的地址
        String hashAdminAddr = "iaa16prjgsyg5m0jg0zmk75r4uzymy8w77d6vtxdyv";

        try {
            comGovClient.addMember(hashAdminAddr, Role.HASH_ADMIN, txBaseTx);
        } catch (ContractException e) {
            e.printStackTrace();
        }
```

## 1 停止旧的 edge-server

## 2 将所有之前发送到链上的 doc 表中的交易状态设置为空

```sql
UPDATE doc 
SET tx_hash = NULL, tx_status = NULL, err_log = NULL
```

## 3 application.yml新增 grpcAddr 字段

详见README.md配置文件实例

## 4 启动新的 edge-server