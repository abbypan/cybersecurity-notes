Wi-Fi Easy Connect
===================

`Wi-Fi Easy Connect <https://www.wi-fi.org/discover-wi-fi/wi-fi-easy-connect>`_

`Wi-Fi Easy Connect Specification <https://www.wi-fi.org/file/wi-fi-easy-connect-specification>`_

简单配网，基于公钥的互联，无需psk/sae。

configurator (例如mobile)  配网  Enrollee (AP/STA)

keys
-------------------------------------

Initiator, Responder

bootstrapping keypair: :math:`(b_I, B_I), (b_R, B_R)` 用于 DPP authentication exchange

Bootstrap Provisioning 主要是PKEX，push button Provisioning 简单一些

network access keypair，用于网络访问，STA and AP 派生 PMK and PMKID

configurator signature keypair: c-sign-key，configurator配置用。用于签名。

configurator Privacy-protection-keypair：configurator 用于隐私保护。

protocol keypair

connector数据结构：由c-sign-key对group id、dpp role、network access pubkey签名的JWS。

discovery 
-------------

dpp service discovery: mdns + dnssd

controller TXT RR 提供 bootstrap key hash

Bootstrapping Services SRV RR 提供Restful URI，由configurator post提交各enrollee的DPP配置信息 (例如SN, bs pub key, dp role: sta/ap/configurator) 

该restful URI 双向TLS + out-of-band usr/pwd 认证。

查询enrollee的bs pub key list应有访问控制。


Bootstrapping
------------------

out-of-band 交换 bootstrapping pubkey

`PKEX <https://datatracker.ietf.org/doc/draft-harkins-pkex/>`_ 协议:
分2阶段，一阶段Authentication使用code结合临时公私钥对建立SPAKE2认证，二阶段Reveal基于已认证的信道安全交换pubkey。

PKEX 协议，基于code，安全交换bootstrapping pubkey


DPP
-----------

复用PKEX，一阶段基于固定的code，结合已交换的bootstrapping pubkey，建立认证信道；二阶段使用临时公私钥对派生session key。

AES-SIV。

DPP Reconfiguration Authentication
----------------------------------------

随机生成ecc point：E-id

随机生成 a-nonce：A-NONCE = a-nonce * G

E'-id = E-id + a-nonce * Ppk

Configurator可以从E'-id - ppk * A-NONCE 恢复 E-id

Network Introduction
------------------------

HPKE

key establishment
-----------------------

基于双方network accesskey keypair，ECDH派生N；基于N派生PMK, PMKID。

实质session连接，还需临时密钥对，ECDH派生Z；基于PMK，与Z结合派生PTK。







