iot security
###############

rfc8576 
==========================================================

iot security

从威胁讲起：漏洞、隐私、clone of things、替换、监听、MITM、镜像、信息提取、路由攻击(改包、选择性转发、分光、伪装）、提权、ddos。

影响：业务影响、安全风险、隐私风险、安全事件处理

基于IP的安全框架。。。

PSK, Raw Public Key, Cert 安全模式。。。

主要问题点：异构网络、资源受限、DDoS、E2E、初始化、group comm、移动网络状态变换、secure update、update old and insecure cryptographic primitives、end of life (eol)、设备证明、应急响应、quantum-resistance、privacy (idenfication, localization, profiling, interaction, life cycle transitions, inventory attack, linkage)、逆向、可信操作。

iotsf 
==========================================================

`IoT Security Foundation Publications <https://www.iotsecurityfoundation.org/best-practice-guidelines/>`_

`Secure Design Best Practice Guides <https://www.iotsecurityfoundation.org/wp-content/uploads/2019/12/Best-Practice-Guides-Release-2_Digitalv3.pdf>`_

classification of data, physical security, device secure boot, secure os, application security, credential Management, encryption, network connections, securing software updates, logging, software update policy, secure boot, secure update, side channel attack。

`IoT Security Assurance Framework <https://www.iotsecurityfoundation.org/wp-content/uploads/2021/11/IoTSF-IoT-Security-Assurance-Framework-Release-3.0-Nov-2021-1.pdf>`_

分了几个安全等级，以及对上面的菜名的细化要求。 

gfce
=======

`International IoT Security Initiative <https://thegfce.org/initiatives/international-iot-security-initiative/>`_

`Internet of Things (IoT) Security GFCE Global Good Practices <https://cybilportal.org/wp-content/uploads/2019/10/GFCE-GGP-IoT.pdf>`_

思路不错，问题点，bcp（设计，实践，认证，基线，标准），challenge（供应链，碎片化，生命周期，rot，监控，人员） 都列了一下。


etsi
==========================================================

`ETSI IoT Security WORKSHOP <https://docbox.etsi.org/Workshop/2016/201606_SECURITYWS/S03_RISKSFROMTRANSPORTDOMAIN/RENAULT_LONC.pdf>`_

nist
==========================================================

`NISTIR 8228 Considerations for Managing Internet of Things (IoT) Cybersecurity and Privacy Risks <https://csrc.nist.gov/publications/detail/nistir/8228/final>`_

主要关注device security, data security, privacy (personally identifiable information, PII)。

`NISTIR 8259 Foundational Cybersecurity Activities for IoT Device Manufacturers <https://csrc.nist.gov/publications/detail/nistir/8259/final>`_

8259主要扯厂商可以在iot device的出厂前，出厂后干些什么事。注意出厂后的安全生命周期、升级、过期等等处理。

8259A关注device Cybersecurity的基线: device idenfication, device configuration, data protection, logical access to interface, software update, cybersecurity state awareness。

8259B主要扯要有什么人，应该干什么事。

`NIST  SP 800-213 IoT Device Cybersecurity Guidance for the Federal Government: Establishing IoT Device Cybersecurity Requirements <https://csrc.nist.gov/publications/detail/sp/800-213/final>`_

800-213 主要是表态。

800-213A 是针对8259讨论的内容的一些描述与解释，看目录也行。

`NIST Cybersecurity for IoT Program <https://www.nist.gov/programs-projects/nist-cybersecurity-iot-program>`_

`Trusted Internet of Things (IoT) Device 4 Network-Layer Onboarding and Lifecycle 5 Management <https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.09082020-draft.pdf>`_

doc
==========================================================

- `The DNS and the Internet of Things: Opportunities, Risks, and Challenges <https://www.icann.org/en/system/files/files/sac-105-en.pdf>`_
- `Charting the Atack Surface of Trigger-Action IoT Platforms <https://adambates.org/documents/Wang_Ccs19.pdf>`_
- `9 Main Security Challenges for the Future of the Internet Of Things (IoT) <https://readwrite.com/2019/09/05/9-main-security-challenges-for-the-future-of-the-internet-of-things-iot/>`_
- `Enhancing-IoT-Security-Report <https://www.internetsociety.org/wp-content/uploads/2019/05/Enhancing-IoT-Security-Report-2019_EN.pdf>`_
- `Hardware or Software Security: Which is right for my IoT Device? <https://www.iotcentral.io/blog/hardware-or-software-security-which-is-right-for-my-iot-device>`_
- `Privacy, Discovery, and Authentication for the Internet of Things <https://arxiv.org/abs/1604.06959>`_
- `A Privacy-Enhancing Framework for Internet of Things Services <https://eprint.iacr.org/2019/1471.pdf>`_






