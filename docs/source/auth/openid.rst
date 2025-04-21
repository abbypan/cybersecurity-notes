OpenID & OIDC (OpenID Connect)
==========================================================

oauth2 based: openid connect 统一登录, uma 用户<->用户授权

OpenID 基于xml，部署比较麻烦。

OIDC 改成在 OAuth2 的基础上，使用RESTful HTTP & JSON实现OpenID的功能，提供identity统一认证信息

RP (Client) 向 OP 请求验证，

End-User 登录 OP

OP向RP返回认证应答

RP从OP获取用户信息

参考资料
----------------

- `A Framework To Implement OpenID Connect Protocol For Federated Identity Management In Enterprises <http://www.diva-portal.org/smash/get/diva2:1121361/FULLTEXT01.pdf>`_
- `OpenID Specifications <http://openid.net/developers/specs/>`_
- `Authentication and Authorization: OpenID vs OAuth2 vs SAML <https://spin.atomicobject.com/2016/05/30/openid-oauth-saml/>`_
- `Understanding OpenID Connect (OIDC) <https://learncsdesigns.medium.com/understanding-openid-connect-oidc-4419246826df>`_
- `Diagrams of All The OpenID Connect Flows <https://darutk.medium.com/diagrams-of-all-the-openid-connect-flows-6968e3990660>`_


