OpenID & OIDC (OpenID Connect)
==========================================================

`OpenID Specifications <http://openid.net/developers/specs/>`_

`Authentication and Authorization: OpenID vs OAuth2 vs SAML <https://spin.atomicobject.com/2016/05/30/openid-oauth-saml/>`_

OpenID 基于xml，部署比较麻烦。

OIDC 改成在 OAuth2 的基础上，使用RESTful HTTP & JSON实现OpenID的功能，提供identity统一认证信息

`A Framework To Implement OpenID Connect Protocol For Federated Identity Management In Enterprises <http://www.diva-portal.org/smash/get/diva2:1121361/FULLTEXT01.pdf>`_

RP (Client) 向 OP 请求验证，

End-User 登录 OP

OP向RP返回认证应答

RP从OP获取用户信息
