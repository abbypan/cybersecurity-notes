OAuth2 Dynamic Client Registration
=====================================

rfc7591

client -> registration endpoint: client基于initial credential & client metadata，请求注册。

registration endpoint -> client: 校验成功后，为该client分配 client id, client secret 等。

client 可基于client secret 的 grant_type，向token endpoint请求 code/token/...，进而再向authorization endpoint请求token。



client registration request
###################################

initial credential:

- initial access token: 在http Authorization Header的Bearer token。

- software statement (JWT, sig/mac): 在http post body中。


client metadata
--------------------

- redirect_uris : 防attack redirect。

- token_endpoint_auth_method: client_secret -> token endpoint的用法，例如HTTP POST/HTTP Basic。

- grant_types: token endpoint可接受的client认证方式(authorization_code=code, implicit=token, password, ...)。

- response_types: authorization endpoint 可接受的client认证方式(code/token)。

- jwks_uri: client的JWK Set所在uri。

- jwks: client的JWK Set。

- software_id: 标识client developer / software publisher的固定UUID。

client registration response 
#############################

- client_id: registration endpoint 动态分发。

- client_secret

- client_id_issued_at

- client_secret_expires_at

client_id & client_secret 结合token_endpoint_auth_method & grant_types，可从token endpoint获得code/token。
