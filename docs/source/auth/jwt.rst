JWT
==========================================================

RFC7515 ~ RFC7519
--------------------

`5 Easy Steps to Understanding JSON Web Tokens (JWT) <https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec>`_

`JWT Introduction <https://jwt.io/introduction/>`_

`jwt-token-encrypt <https://www.npmjs.com/package/jwt-token-encrypt>`_

`JWT: The Complete Guide to JSON Web Tokens <https://blog.angular-university.io/angular-jwt/>`_

本质上是以json形式传输的bear token，可以在 **Authorization: Bearer <token>** 头部传输，此时由于不在cookie中，可以天然避开cors问题。

由 header.payload.signature 三部分组成。其中，header, payload默认是json格式，base64url编码。

signature 由 header.payload 加secret 组合计算hmac256得到。

此时，server可以快速校验payload里的用户信息的合法性，授权资源。缓解传统的session/cookie查找资源问题。

signature也可以使用rsa/ecdsa等签名算法生成。

如果payload中有部分敏感内容被加密，则server需要有对应的解密配置。

优点在于server端非常容易对token做轮转，json内容简单，且避开了cookie的cors等问题。缺点在于提高了对secret的安全性依赖。


RFC9493
---------

identifier format

    {
      "format": "account",
      "uri": "acct:example.user@service.example.com"
    }

