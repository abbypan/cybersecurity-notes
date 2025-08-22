OAuth2 PKCE
=============

implicit flow 风险：
- redirect URI 篡改
- 避免access token在browser历史中可读
- 避免access token在windows.location中刻度，cross-site scripting
- 避免access token泄露到third-party script



app -> authorization server: 登录时，带一个code_challenge = hash(code_verifier)

authorization server -> app: 校验通过，返回authorization code

app -> authorization server: 使用authorization code请求access token，带上code_verifier

authorization server -> app: 使用code_verifier校验code_challenge，校验通过，返回access token

app -> resource server: 使用access token请求data

resource server -> authorization server: 请求JWKS

authorization server -> resource server: JWKS

resource server : 校验JWKS，确认access token可信

resource server -> app : 返回结果

参考资料
----------------

- `Implicit Flow vs. Code Flow with PKCE <https://christianlydemann.com/implicit-flow-vs-code-flow-with-pkce/>`_


