OAuth 2.1 
===============

`OAuth 2.1 <https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/>`_

role
--------

client, resource owner (user), resource server, authorization server


protocol flow
----------------

1. client 从 resource owner 处获取 authorization grant。resource owner通过browser访问auhtorization server (authroization endpoint)，完成user authentication。

#. client 基于 authorization grant 从 auhtorization server (token endpoint) 处获取 access token。

#. client 基于 access token 从 resource server 处获取 protected resource。

authorization grant types
--------------------------

- authorization code

- refresh token

- client credentials (例如client secret, 用于签发JWT的client private key等)

extension:

- Device Authorization Grant, RFC8628。


access token
---------------

- token string: bearer token，不含token详情。RFC7662 token introspection用于resource server向authorization server查询token详情。

- JWT: 自包含token详情+signature(mac/sig), RFC9068。

sender-constrained extension:

- DPoP: RFC9449 

- mTLS: RFC8705


client type
---------------

- web app: backend server存token，client与backend server之间通过cookie处理。含client secret在HTTP Basic header。

- browser-based application: 浏览器访问，应避免在browser中存token。无需client secret，用PKCE。

- native app

client redirection endpoint
--------------------------------

authorization server 提前登记，精确匹配，避免恶意重定向。


client authentication
----------------------

- client secret
- JWT
- mTLS

例如client secret如果以HTTP Basic认证，则最后HTTP header为 Authorization: Basic  base64(client_id:client_secret)


security
-----------

bearer token 禁止存cookie。

bearer token 禁止出现在page URLs。

PKCE。

user authentication需用户参与。

防clickjacking，限制frame来源，如Content-Security-Policy、X-Frame-Options等。

Native App 用 external user agent（例如browser），而非embedded user agent（内嵌webview）。

in-app browser tabs，无需切换app，同时有browser的保护效果。

redirect URI默认https，仅loopback可http，Private-Use URI存在类似customer permission的抢注问题。




