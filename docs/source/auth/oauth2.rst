OAuth2 
##########################################################

FIDO, OAuth2, CA SSO 对web服务实际效用会更好一些

`RFC6749: [The OAuth 2.0 Authorization Framework <https://tools.ietf.org/html/rfc6749>`_


基础
==========================================================

典型使用场景：服务方(例如新浪微博)为第三方应用提供授权，使得该服务的用户（微博用户）能够使用第三方应用获取该服务的某些资源。


1. 第三方应用(client) -> 资源拥有者(user) ： 认证请求

#. user -> client : 认证授权

#. client -> 认证服务器( Authorization Server ) : 认证授权

.. note::

    GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz
    &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com

    Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz

#.  Authorization Server -> client : Access Token 

.. note::

    POST /token HTTP/1.1
    Host: server.example.com

#.  client -> 资源服务器( Resource Server ) : Access Token

#.  Resource Server -> client : Protected Resource

.. note::

    GET /authorize?response_type=token&client_id=s6BhdRkqt3&state=xyz&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com


Access Token 过期之后，会有 Refresh Token 重新请求

注意点
==========================================================

认证成功后，授权资源的选项劫持？

HTTPS？

重定向URL的安全性，是否存在二次跳转？

TOKEN重绑定间隔？

bearer token(用plain text，没有secret/签名等机制，仅依赖底层tls)难以规避伪造、重放、重定向、泄漏等安全问题

讨论
==========================================================

oauth2资源授权分级的灵活性高，明显适用于服务方的生态圈场景。第三方不保存用户密码，而是通过access token临时获取访问资源的权限。

在用户侧，第三方应用相当于换个表单登陆，或者换个按钮登陆。

大型服务方一般不会喜欢openid。

fido方案偏设备硬件。

bitid是基于特殊overlay，还要post公钥到callback的uri，有时也是挺累。

