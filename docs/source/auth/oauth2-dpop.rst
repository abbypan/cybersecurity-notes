OAuth2 DPoP
================

RFC 9449 OAuth 2.0 Demonstrating Proof of Possession (DPoP)

client 随机生成dpop key pair

client -> authorization server 的access token请求中，包含dpop pubkey，以及dpop signature。

authorization server: 校验dpop signature，将 dpop pubkey 与 access token 关联。

authorization server -> client: type为dpop的access token。

client -> resource server: 以acess token请求resource。

resource server：根据acess token检查是否允许该client访问resource。
- 如果access token为JWT格式，则resource server可校验dpop pubkey已被authorization server认可并签名；
- 如果access token为opaque string，则resource server通过token introspection endpoint查询dpop pubkey详情。




