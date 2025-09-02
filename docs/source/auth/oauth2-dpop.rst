OAuth2 DPoP
================

RFC 9449 OAuth 2.0 Demonstrating Proof of Possession (DPoP)

注意dpop主要防access token盗用，不保护http payload。

acess token request 
---------------------

client: 随机生成dpop key pair

client -> authorization server 的access token请求中，包含dpop pubkey，以及dpop signature。

authorization server: 校验dpop signature，将 dpop pubkey 与 access token 关联。

authorization server -> client: type为dpop的access token。


resource request
--------------------

client -> resource server: 以该acess token请求resource，http header中包含:

- Authorization: DPoP <access token>

- DPoP: <DPoP Proof>

该DPoP Proof声明了此次请求的 { Target URI (htu), HTTP method (htm), Timestamp (iat), Unique identifier (jti)}, 以 dpop private key签名。

resource server：检查acess token

- 如果access token为JWT格式，则resource server可校验access token的合法性，并通过cnf.jkt对dpop pubkey的digest进行比较，确认dpop pubkey已被authorization server认可。

- 如果access token为opaque string，则resource server通过token introspection endpoint查询dpop pubkey详情。

resource server: 基于dpop pubkey，检查dpop proof。

resource server: 最终决策是否允许该client访问resource。
