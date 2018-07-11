---
title: JWT 丨 JSON Web Tokens 丨 java-jwt
keywords: [JWT,jwt java,java-jwt]
date: 2017/8/17 17:44
tags: [java,jwt]
categories: java
---
## JWT简介
JWT(json web token)是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准。

JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源。比如用在用户登录上。
## JWT生成的token
欲加密的字符：hisen
加密后的字符：(分三段)
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXlsb2FkIjoiXCJoaXNlblwiIiwiZXhwIjoxNTAyOTY0Mjk2fQ.gzg4JEm8Z-GoU9eNaNll9I1wQQ0cEAbZC9OBUjAAQqI
```
### JWT的构成
<!--more-->
#### header
完整的头部，json格式：
1. 声明类型，这里是jwt
2. 声明加密的算法 通常直接使用 HMAC SHA256
```
{
  'typ': 'JWT',
  'alg': 'HS256'
}
```
然后对头部进行base64加密，构成第一段：
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
#### playload
载荷就是存放有效信息的地方。这个名字像是特指飞机上承载的货品，这些有效信息包含三个部分
1. 标准中注册的声明
2. 公共的声明
3. 私有的声明
##### 标准中注册的声明 (建议但不强制使用)
1. iss: jwt签发者
2. sub: jwt所面向的用户
3. aud: 接收jwt的一方
4. exp: jwt的过期时间，这个过期时间必须要大于签发时间
5. nbf: 定义在什么时间之前，该jwt都是不可用的.
6. iat: jwt的签发时间
7. jti: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。
##### 公共的声明
公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密.
##### 私有的声明
私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，意味着该部分信息可以归类为明文信息。
##### 定义一个payload
```
{
  "payload": "hisen"
}
```
然后对头部进行base64加密，构成第二段：
```
eyJwYXlsb2FkIjoiXCJoaXNlblwiIiwiZXhwIjoxNTAyOTY0Mjk2fQ
```
#### signature
jwt的第三部分是一个签证信息，这个签证信息由三部分组成
1. header (base64后的)
2. payload (base64后的)
3. secret
这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串，然后通过header中声明的加密方式进行加盐secret组合加密，然后就构成了jwt的第三段：
```
gzg4JEm8Z-GoU9eNaNll9I1wQQ0cEAbZC9OBUjAAQqI
```
**密钥secret是保存在服务端的，服务端会根据这个密钥进行生成token和验证，所以需要保护好。**

## java实现方式
### maven依赖
```
<dependency>
      <groupId>com.auth0</groupId>
      <artifactId>java-jwt</artifactId>
      <version>3.2.0</version>
</dependency>
```
### 生成token代码(加密)
```
// 加密，传入一个对象和有效期
public static <T> String sign(T object, long maxAge)
    throws UnsupportedEncodingException {
  Map<String, Object> map = new HashMap<String, Object>();
  String jsonString = JSON.toJSONString(object);
  map.put("alg", "HS256");
  map.put("typ", "JWT");
  long exp = System.currentTimeMillis() + maxAge;
  String token = JWT.create()
      .withHeader(map)//header
      .withClaim(PAYLOAD, jsonString)//存放的内容 json
      .withClaim(EXP, new DateTime(exp).toDate())//超时时间
      .sign(Algorithm.HMAC256(SECRET));//密钥
  return token;
}
```
### 解析token(解密)
```
// 解密，传入一个加密后的token字符串和解密后的类型
public static <T> T unsign(String token, Class<T> classT) throws UnsupportedEncodingException {
  JWTVerifier verifier = JWT.require(Algorithm.HMAC256(SECRET)).build();
  DecodedJWT jwt = verifier.verify(token);
  Map<String, Claim> claims = jwt.getClaims();
  if (claims.containsKey(EXP) && claims.containsKey(PAYLOAD)) {
    long tokenTime = claims.get(EXP).asDate().getTime();
    long now = new Date().getTime();
    // 判断令牌是否已经超时
    if (tokenTime > now) {
      String json = claims.get(PAYLOAD).asString();
      // 把json转回对象，返回
      return JSON.parseObject(json, classT);
    }
  }
  return null;
}
```
### 完整代码 & 测试代码
[基于JWT token认证 | JSON Web Tokens | java-jwt 3.2.0 | demo](https://github.com/hisen-yuan/IDEAPractice/tree/master/src/main/java/com/hisen/jars/jwt)
## JWT总结
1. 因为json的通用性，所以JWT是可以进行跨语言支持的，像JAVA,JavaScript,NodeJS,PHP等很多语言都可以使用。
2. payload部分，JWT可以在自身存储一些其他业务逻辑所必要的非敏感信息。
3. 便于传输，jwt的构成非常简单，字节占用很小，所以它是非常便于传输的。它不需要在服务端保存会话信息, 所以它易于应用的扩展
4. 点击登陆，如果帐号密码校验通过，给用户生成一个token返回，然后用户每次登陆都带上这个token即可(放在header？)

JWT官方网站：[https://jwt.io/](https://jwt.io/)
