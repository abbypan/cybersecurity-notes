Wi-Fi Easy Connect
===================

`Wi-Fi Easy Connect <https://www.wi-fi.org/discover-wi-fi/wi-fi-easy-connect>`_

`Wi-Fi Easy Connect Specification <https://www.wi-fi.org/file/wi-fi-easy-connect-specification>`_

简单配网，基于公钥的互联，无需psk/sae。

configurator (例如mobile)  配网  Enrollee (AP/STA)

DPP (Device Provisioning Protocol) 

Initiator, Responder

bootstrap keypair: :math:`(b_I, B_I), (b_R, B_R)` 

Bootstrap Provisioning 主要是PKEX，push button Provisioning 简单一些

dpp service discovery: mdns + dnssd

controller TXT RR 提供 bootstrap key hash

Bootstrapping Services TXT RR 提供 DPP URI



STA and AP 派生 PMK and PMKID
