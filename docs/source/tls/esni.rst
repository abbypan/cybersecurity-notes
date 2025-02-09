esni
##########

`tls-esni <https://datatracker.ietf.org/doc/draft-ietf-tls-esni/>`_

`encrypted sni <https://blog.cloudflare.com/encrypted-sni/>`_

Server Name Indication

通过dns配置server端的public key

client hello的时候，可以通过数字信封之类加密sni。

结合dot/doh，减少信息泄漏，适用于cdn多个virtual host场景。
