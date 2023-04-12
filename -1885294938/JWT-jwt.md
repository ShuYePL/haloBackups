---
title: JWT
date: 2023-04-03 22:33:20.912
updated: 2023-04-03 22:33:20.912
url: /archives/jwt
categories: 
- JavaWeb
tags: 
---

# 1. JWT简介

## 1.1 官网

[JSON Web Tokens - jwt.io](https://jwt.io/)

JWT : JSON Web Token，看最后一个单词Token，令牌，就是一种认证的东西，JWT使用JSON的形式在Web应用中的前后端之间传输信息，这种传输信息的形式是安全的，因为JWT可以进行数字签名验证，以及对数据进行加密。

## 1.2 传统认证方式

session保存用户登录信息的方式在单体项目中用得比较多，但在现在的需求之下显得有点无力了，session在当前的应用中有几个缺点

- session信息保存在服务端内存中，对服务器资源压力较大
- session在做集群服务的时候需要处理共享问题
- session保存一份信息（sessionid）在客户端，每次访问服务端都要携带session信息，有被窃取篡改的风险
- session信息携带的特征内容不够多，扩展力较差

## 1.3 JWT认证方式

1. 客户端发送HTTP POST请求，通过表单的形式将用户的用户名和密码发送到后端，建议使用HTTPS发送。
2. 服务端根据传过来的表单信息进行认证，认证通过之后将用户的id等信息加入到JWT的payload中，将其与头部进行Base64编码，并拼接上千名，生成JWT。
3. 将JWT发送到客户端中进行本地保存，如localStorage或者是sessionStorage，退出登录是删除即可。
4. 以后每次发送请求的时候都带上JWT，放在请求头上的Authorization位，解决XSS和XSRF问题。
5. 服务端对JWT进行拦截认证，认证通过了就放行，否则拦截。

## 1.4 JWT的优势

1. 简洁：数据量小，传输速度快。
2. 自包含：payload中包含了用户的一些信息，避免多次查询数据库。
3. 跨语言：Token是以JSON加密的形式保存的。
4. 不需要在服务端保存会话信息，适用于分布式服务。  

# 2. JWT结构

## 2.1 令牌组成

由三部分组成：head.payload.singurater，即标头、有效载荷、签名。标头和有效载荷会进行Base64编码。

## 2.2 标头

记录令牌的类型和使用的签名算法。

## 2.3 有效载荷

实体的有效信息等。

## 2.4 签名

保证内容没被修改过。

# 3. 使用JWT

## 3.1 引入依赖

```xml
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>4.4.0</version>
</dependency>
```

## 3.2 生成token

```java
@Test
public void testGetJWT() {
    Map<String, Object> map = new HashMap<>();

    Calendar instance = Calendar.getInstance();
    instance.add(Calendar.SECOND, 20000);
    String token = JWT.create()
        .withHeader(map)    // header
        .withClaim("userId", 21)  // payload
        .withClaim("username", "xiaochen")
        .withExpiresAt(instance.getTime())    // 指定过期时间
        .sign(Algorithm.HMAC256("WIWUEUE@#$!#%$")); // 签名
    System.out.println(token);
}
```

## 3.3 根据令牌和签名解析数据

~~~java
@Test
public void testValidateJWT() {
    String token = "上面生成的token";
    // 获取验证对象，秘钥要和之前加密的秘钥一样
    JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("WIWUEUE@#$!#%$")).build();
    DecodedJWT verify = jwtVerifier.verify(token);
    System.out.println(verify.getClaim("userId"));
    System.out.println(verify.getClaim("username"));
}
~~~

## 3.4 可能会抛出的异常

1. SignatureVerificationException 签名不一致异常
2. TokenExpiredException 令牌过期异常
3. AlgorithmMismatchException 算法不匹配异常
4. InvalidClaimException 失效的payload异常

---

参考资料：

- [【编程不良人】JWT认证原理、流程整合springboot实战应用,前后端分离认证的解决方案!_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1i54y1m7cP/?spm_id_from=333.337.search-card.all.click&vd_source=3b9bc9314c9590a1d18a84ef490fb982)