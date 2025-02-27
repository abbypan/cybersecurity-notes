esim
########

doc
==========================================================

`GSMA eSIM Spec <https://www.gsma.com/esim/esim-specification/>`_

`RSP Architecture <https://www.gsma.com/newsroom/wp-content/uploads//SGP.21_v2.2.pdf>`_

`SGP.22 RSP Technical Specification <https://www.gsma.com/esim/wp-content/uploads/2020/06/SGP.22-v2.2.2.pdf>`_

role
==========================================================

CI : 负责证书签发

EUM：euicc卡商

SIM：Subscriber Identity Module

SM-DS：Subscription Manager-Discovery Server  负责与LPA中的LDS模块沟通，提供SM-DP+的相关信息。

SM-DP+：Subscription Manager-Data Preparation +  负责生成esim profile

Local Profile Assistant : LPA，负责在SM-DP+与eUICC之间转发信息，协助用户激活eSIM。LPA包含3个子模块，LPD负责下载，LDS负责服务发现，LUI负责用户界面。

struct
==========================================================

概要图见 Remote SIM Provisioning System Architecture

对于euicc而言，遵守GP规范，每个运营商可以占一个security domain

cert hierarchy
==========================================================

    CI -> EUM ->  EUICC
    CI -> SM-DS
    CI -> SM-DP+

    GSMA CI Certificate (CERT.CI.ECDSA)
    EUM Certificate(CERT.EUM.ECDSA)
    SM-DP+ Certificates (CERT.DPauth.ECDSA 用于信息认证, CERT.DPpb.ECDSA 用于Profile Package Binding的信息认证)
    SM-DP+ TLS Certificate (CERT.DP.TLS 用于LPA与DP+的安全TLS连接)
    SM-DS Certificate (CERT.DSauth.ECDSA)
    SM-DS TLS Certificate (CERT.DS.TLS 用于LPA与DS的安全TLS连接)

Technical Specifications
==========================================================

profile 信息的处理

ppk
----------------------------------------------------

见 Profile  Package stage Description

SM-DP+ 以TLV的格式生成 Profile 数据，每个TLV为一份数据

SM-DP+ 以 Profile Protection Key (PPK-ENC) 加密上述数据，每份数据都用PPK-MAC算一个MAC。每份数据+MAC做为一个segement。

SM-DP+ 进行 Profile Package Binding，包括以session key（S-ENC）加密的CI信息，以session key（S-MAC）计算MAC的metadata明文，以session key（S-ENC）加密的PPK，以PPK处理各segment数据段。

其中，S-ENC、S-MAC 是由SM-DP+与eUICC通过双方生成的一次性临时ecc密钥对，进行ECKA计算派生的会话密钥。而双方的临时公钥的合法性，是通过双方自身的私钥签名提供的。传输时，签名发送方会给出对应的证书帮助接收方进行签名校验。

无ppk
----------------------------------------------------

如果SM-DP+不随机生成PPK-ENC、PPK-MAC，那么可以直接用S-ENC/S-MAC处理TLV数据。

也就无需使用S-ENC/S-MAC处理PPK。

