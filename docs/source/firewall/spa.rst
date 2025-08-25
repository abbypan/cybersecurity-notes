
Single Packet Authorization
================================

https://network-insight.net/2019/06/18/zero-trust-single-packet-authorization-passive-authorization/

client & server 共享 AES key + HMAC key。

握手后，使用 AES + HMAC 处理首个packet payload，内含 timestamp + client info。

首包校验后，firewall放行。


