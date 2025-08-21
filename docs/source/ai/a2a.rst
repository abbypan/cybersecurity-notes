A2A
#####

https://a2a-protocol.org/latest/specification/

https://a2a-protocol.org/latest/sdk/python/api/a2a.html

https://a2a-protocol.org/latest/topics/enterprise-ready/#2-authentication

authentication
===================

A2A spec不涉及credential的分发。

复用现有HTTPS + OAUTH2 + JWS 相关逻辑。

AgentCard中声明securitySchemes，参考 https://swagger.io/docs/specification/authentication/ 。

credential仅在header传递，不在payload里，例如

        Authorization: Bearer <oauth2 token>, 

        Basic: <Basic_Auth_STR>

        API-Key: <key_value>


A2A server 应对每个A2A client request校验credential。

A2A Clients 校验 A2A Server Certificate中绑定的A2A server identity。


In-Task Authentication (Secondary Credentials)
-------------------------------------------------

辅助credential通过PushNotificationAuthenticationInfo交互。

https://a2a-protocol.org/latest/topics/streaming-and-async/#security-considerations-for-push-notifications

A2A client提交 PushNotificationConfig 到A2A server，包含url (webhook url)，token(optional, 随机生成)，authentication(optional，指定webhook url的认证机制)。

A2A server应校验webhook url，防范SSRF。

A2A server将token置于X-A2A-Notification-Token header中标识session，根据authentication scheme向A2A client提交credential。

核心在于A2A client如何认证A2A server，如bearer token (jwt)/api key/hmac/mTLS。

注意防重放。

key rotation。

well-known endpoint url: publish JWKS (JSON Web Key Set)



Authorization
===============

与client/user identity绑定


agentCard
===========

well-known uri: https://{server_domain}/.well-known/agent-card.json

如果agentCard内容中包含敏感信息，则要求强制mTLS。

不推荐在agentCard中包含明文secret。

agentCard自身内容的完整性通过JWS格式的AgentCardSignature保障。



安全分析
==========

public key 信任根问题仍在于Hijack。

HMAC型的psk存在management问题。

