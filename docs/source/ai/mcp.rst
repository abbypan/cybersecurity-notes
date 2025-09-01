mcp 
=====

https://modelcontextprotocol.io/docs/learn/architecture

core components
------------------

application 中manage多个mcp client

每个mcp client连接对应的mcp server


authorization
----------------

https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization

强制 oauth2.1

要求 dynamic client registration (rfc7591)，mcp client动态获取client id & client credential

mcp server 即 resource server，通过well-known url声明自身能力

Authorization Flow Steps
-----------------------------

mcp client <->  mcp server :  获取 mcp server 的 metadata

mcp client: 从 metadata 解析出 authorization server


mcp client <-> authorization server: 请求实施注册，获得client credentials

mcp client -> browser : 构造PKCE请求（含code verifier），拉起browser

browser <-> authorization server: 用户登录并获得authorization code

browser -> mcp client: authorization code

mcp client -> authorization server: 以authorization code + code challenge请求对应resource的access token

authorization server: 颁发对应access token


mcp client -> mcp server: 以access token请求对应resource


bcp
-------

Confused Deputy Problem: mcp proxy使用static client ID，无dynamic registration，browser侧不做user人工确认，攻击者(evil client)可能构造恶意redirect URI，欺骗用户点击登录并授权，最终获取用户的access token。


分析
-----

proxy场景需要强信任。

直连场景主要依赖oauth2.1。

