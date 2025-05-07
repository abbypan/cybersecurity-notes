TLS 1.3
################


overview
==========================================================

aead

移除 staic rsa & dh，强制 fs

serverhello后的所有handshake msg全部加密

encryptedExtensions msg可以支持serverhello之前的加密

kdf统一为hkdf

移除 changecipherspec

eddsa等新signature algorithm

移除旧的签名算法、dhe groups

signature_algorithms & signature_algorithms_cert
==========================================================

signature_algorithms 是客户端可以接受的签名类型

signature_algorithms_cert 是能接受的证书签名算法

显然，signature_algorithms可以严一些

 legacy_version & supported_versions
 ==========================================================

为了兼容旧版本tls，legacy_version 设为0x0303 = tls 1.2，supported_versions 的首个内容可以设为 0x0304 = tls 1.3

updating traffic secret
==========================================================

基于第n个application traffic secret派生第n+1个application traffic secret

hkdf-expand-label

nonce
==========================================================

[TLS nonce-nse](https://blog.cloudflare.com/tls-nonce-nse/)

tls 1.2  nonce = 显式的固定IV + SEQNUM

tls 1.3  nonce = 派生的固定IV xor SEQNUM

Resumption
==========================================================

`TLS Session Resumption <https://ldapwiki.com/wiki/TLS%20Session%20Resumption>`_

`A Survey of TLS 1.3 0-RTT Usage <https://ethz.ch/content/dam/ethz/special-interest/infk/inst-infsec/appliedcrypto/education/theses/MihaelLiskij_Thesis.pdf>`_

0-RTT的风险主要在于replay attack，其根源在于server没有fresh value参与该session处理。同时，不支持fs。

tls 1.3 的 handshake 默认是 1-RTT。对于server主动发包的应用协议，可以做到0.5-RTT。

对于resumption handshake，可以用psk binder标识重连，如果psk是基于之前的handshake share secret派生，那么psk binder标识到psk的映射，可能需要server端安全存储，即stateful session ticket；如果是基于session ticket encryption key (stek) 对 session parameters 做加密，那么涉及STEK的管理，即stateless session ticket。

缓解replay attack的方案，对于stateful session ticket主要是分布式server的重连记录同步（麻烦）、对于stateless session ticket主要是时间限制、STEK的定期更新（有限时间窗可用）。

实际上，应避免0-RTT的请求对server状态产生影响。即，无法确认应用场景的条件下，默认不应开启0-RTT。application & transport 的隔离被破坏了。

0-RTT 还要看一下ALPN。

extended master secret
==========================================================

RFC7627 extended master secret, 在handshake消息过程中引入hash value

不同握手过程中，master secrets 不同

tls enc
==========================================================

rfc5246:  The Transport Layer Security (TLS) Protocol Version 1.2

          data -> mac  
          data + padding -> cipher
          padding不在mac的保护下, padding oracle attack

rfc7366:  Encrypt-then-MAC for Transport Layer Security (TLS) and Datagram Transport Layer Security (DTLS)

data + padding -> cipher -> mac

ekm
=======

RFC5705: Keying Material Exporters for Transport Layer Security (TLS)

EKM: Exported Keying Material 

应用层协议复用 tls 密钥协商的 Key Material，派生应用层的相关密钥。

应用层协议可以在tls的client hello、serverhello指定派生的参数（label，context）。

至少可以省1个RTT。

应用层可以使用派生的密钥加密信息，做深层的密钥协商。


doc
==========================================================

- `RFC 8446: tls 1.3 <https://tools.ietf.org/html/rfc8446>`_
- `More Privacy, Less Latency tls1.3 <https://timtaubert.de/blog/2015/11/more-privacy-less-latency-improved-handshakes-in-tls-13/>`_
- `A Readable Specification of TLS 1.3  <https://davidwong.fr/tls13/>`_
- `A Detailed Look at RFC 8446 (a.k.a. TLS 1.3) <https://blog.cloudflare.com/rfc-8446-aka-tls-1-3/>`_
- `TLSeminar <https://tlseminar.github.io/>`_
- `TLS Crypto Seminar <https://www.felixguenther.info/teaching/2019-tls-seminar/2019-tls-seminar_02-21_TLS13-intro-MSKE-MKC.pdf>`_
- `SLOTH <https://www.mitls.org/pages/attacks/SLOTH>`_
- `Proving the TLS Handshake Secure (as it is) <https://hal.inria.fr/hal-01102229/document>`_
- `On the Security of TLS-DH and TLS-RSA in the Standard Model <https://eprint.iacr.org/2013/367.pdf>`_
- `Problems With Elliptic Curve CryptographyIn TLS and SSH <https://www.rochestersecurity.org/wp-content/uploads/2017/10/RSS2017-T2-Testa.pdf>`_
- `SMTP and Transport Layer Security (TLS) <https://www.fehcom.de/qmail/smtptls.html>`_
- `KCI Attacks against TLS <https://kcitls.org/>`_
- `The Illustrated TLS Connection <https://tls.ulfheim.net/>`_
- `ssl/tls test <https://browserleaks.com/ssl>`_
- `tls-tutorial <https://github.com/BenSmyth/tls-tutorial/>`_
- cypto -> tls -> ipsec/ike -> ssh : `BSI TR-02102 Cryptographic Mechanisms <https://www.bsi.bund.de/EN/Service-Navi/Publications/TechnicalGuidelines/tr02102/tr02102_node.html>`_









