ETSI eTLS
##############

overview
==========================================================

`ETSI releases standards for enterprise security and data centre management <https://www.etsi.org/news-events/news/1358-2018-11-press-etsi-releases-standards-for-enterprise-security-and-data-centre-management>`_

`Enterprise TLS <https://www.etsi.org/deliver/etsi_ts/103500_103599/10352303/01.01.01_60/ts_10352303v010101p.pdf>`_

`Network Gateway Cyber Defence <https://www.etsi.org/deliver/etsi_tr/103400_103499/103421/01.01.01_60/tr_103421v010101p.pdf>`_

满足企业监控流量的场景需求。

应用
==========================================================


在企业内部的server使用固定的key pair用于tls1.3的DH协商过程。注意这个key pair跟证书公钥不同。

只有firewall与外界会进行标准的tls1.3 ephemeral DH交换。

.. note::
   
    client  <--- tls1.3 ---> firewall <--- etls ---> enterprise server (static key pair)

    enterprise client <--- etls ---> firewall (static key pair) <--- tls ---> internet server

显然，由于static key pair可管控，企业在etls的链路上可以随时部署middlebox进行监控。

visibility information
==========================================================


.. note::
   
    VisibilityInformation ::= SEQUENCE {
        fingerprint         OCTET STRING (SIZE(10)),
        accessDescription   UTF8
    }


上述static DH public key的SHA-256摘要，拆成(H0, ..., H7)的8个32-bit数，在serverhello里的key_share extension传输，参考FIPS 180-4。

.. note::
   
    fingerprint = H0 || H1 || (H2 >> 16)  ,  共80-bit

accessDescription 是文本说明。

VisibilityInformation可以以OtherName做为subjectAltName的field entry，在证书里进行OID标识，参考RFC5280 Subject Alternative Name。

一个证书可以登记多个static DH public key，对应的，也可以写入多个VisibilityInformation。

Server可以在certificate里以VisibilityInformation主动标示etls，但这不是标准扩展，可能导致严格检查证书的client协议栈拒绝连接————企业可以把内部client也改掉。

影响
==========================================================

在企业内部环境可以任意监控。会适合很多强监管的内网。

不具备per-session forward security。100% 不是IETF TLS所倡导的安全方向。

Forward Secrecy 即长期使用的主密钥泄漏不会导致过去的会话密钥泄漏。

现实一点说，如果定期rotate static DH key pair, 运维为了省事，很可能就不在证书里标注VisibilityInformation了。
