SeND
========

`RFC3971: SEcure Neighbor Discovery (SEND) <https://tools.ietf.org/html/rfc3971>`_

router verify node
======================================

`CGA: cryptographically generated addresses <https://en.wikipedia.org/wiki/Cryptographically_Generated_Address>`_

节点生成公私钥对。基于公钥、随机数等信息hash出IP地址。

路由器可以发timestamp + challenge，让节点计算签名，进行校验。

注意：无法避免初次绑定的伪造。


node verify router
======================================

校验router x509证书。。。

需要一些预置工作。。。

IPv6 NDP
======================================

SeND 保护NDP

Neighbor Discovery

SLAAC
======================================

IPv6 Stateless Address Autoconfiguration
