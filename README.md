# PACC PA2

PACC 后端的密码哈希实现：自研 Argon2id（RFC 9106 v1.3）加 Blake2b，只用 JDK 标准库，不引任何第三方依赖。

后端的密码哈希原先走 BouncyCastle 的 `Argon2BytesGenerator`（`bcprov-jdk18on` 约 5MB）。这个模块把它换掉：相同参数下输出与 BouncyCastle 逐字节一致，存量密码哈希不用重置。

母仓库是 [EPOTATOTV/PACC4_0](https://github.com/EPOTATOTV/PACC4_0)，本仓库是它在 `ptv-backend/src/main/java/com/potatotv/pa2` 下的独立镜像，两边内容保持一致。

## 用法

```java
Argon2 argon2 = new Argon2(iterations, memoryKib, parallelism, hashLength);
byte[] hash = argon2.hash(password, salt);
```

只提供 Argon2id（y=2）与版本 1.3，Argon2d / Argon2i 及旧版本不实现。

## 构建与测试

```bash
mvn -B test
```

`Argon2Test` 钉的是 BouncyCastle 1.86 生成的基准向量，用来确认替换后逐字节一致。