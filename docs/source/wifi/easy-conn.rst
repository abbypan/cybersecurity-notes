Wi-Fi Easy Connect
===================

`Wi-Fi Easy Connect <https://www.wi-fi.org/discover-wi-fi/wi-fi-easy-connect>`_

`Wi-Fi Easy Connect Specification <https://www.wi-fi.org/file/wi-fi-easy-connect-specification>`_

简单配网，基于公钥的互联，无需psk/sae。

configurator (例如mobile)  配网  Enrollee (AP/STA)

DPP (Device Provisioning Protocol) 
-------------------------------------

Initiator, Responder

bootstrapping keypair: :math:`(b_I, B_I), (b_R, B_R)` 用于 DPP authentication exchange

Bootstrap Provisioning 主要是PKEX，push button Provisioning 简单一些

network access keypair，用于网络访问，STA and AP 派生 PMK and PMKID

configurator signature keypair，用于相关配置

protocol keypair

discovery 
-------------

dpp service discovery: mdns + dnssd

controller TXT RR 提供 bootstrap key hash

Bootstrapping Services SRV RR 提供Restful URI，由configurator post提交各enrollee的DPP配置信息 (例如SN, bs pub key, dp role: sta/ap/configurator) 

该restful URI 双向TLS + out-of-band usr/pwd 认证。

查询enrollee的bs pub key list应有访问控制。







