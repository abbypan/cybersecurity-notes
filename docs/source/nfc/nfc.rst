nfc
#######

doc
==========================================================

《beginning NFC: Near Field Communication with Arduino, Android, and PhoneGap》

《Near Field Communication From Theory to Practice》

`APPROACHES TO THE DATA SECURITY AND SAFETY OF THE PARTICIPANTS OF NFC-ECOSYSTEM <http://www.novacard.ru/en/actual/?id=860>`_

`gp spec <https://globalplatform.org/specs-library/>`_

`NFC for embedded applications <https://www.nxp.com/docs/en/brochure/75017587.pdf>`_

rfid, nfc
==========================================================

rfid & nfc 都工作在 13.56 MHz

rfid 主要用于标签识别，而非通信

无源模式：目标方没有电源供电

有源模式：目标方有电源供电

nfc模式
==========================================================

nfc支持的通信模式：有源、无源

nfc设备的操作模式：读写器，读写目标方的数据；卡模拟，与rfid标签类似；点对点，双向数据交换

nfc标签
==========================================================

类型1,2,4 基于iso-14443a；类型3基于iso-18092

标签数据一般 96 bytes～4KB~1MB

ndef: nfc数据交换格式
==========================================================

每次交互传送一条 ndef message

每条 ndef message 包含 >= 1 条 ndef record，分块内容不能跨ndef message传递

每个 ndef record 包含: header (TNF 指定payload type字段值的结构, record ID, payload type, payload ID, ...), payload

一个ndef record最大载荷为 2^32 - 1 字节（4GB - 1)

record type:  text, uri, smart poster, signature

nfc点对点通信涉及llcp(逻辑链路控制协议)和snep(简单NDEF数据交换协议)

tnf
==========================================================

tnf 类型名称格式，例如0x01 标识nfc rtd，表示well-known type；0x02标识mime media-type，rfc2046；...

nfc rtd code，例如T表示Text，U表示URI，Sp表示Smart Poster，...

uri识别码(uic)，例如0x01表示http://www.，0x06表示mailto:，...

PUF 防物理克隆
==========================================================

physical unclonable function, physical random function

- SE安全芯片生产过程中提取PUF query-response 信息
- 以每一个芯片的PUF查询&响应对作为输入参数，生成公私钥对。将私钥写入SE安全芯片。
- 后台记录PUF查询字，以及公钥。
- 后续其他客户端可以发送PUF查询字向后台查询公钥，与该安全芯片通信。

APDU
==========================================================

命令式的APDU：4字节(CLA 指令类, INS指令码, P1指令参数, P2指令参数)，Lc数据体长度，命令数据体，Le最大响应字节数。

响应式的APDU：响应数据体，状态字2字节(SW1, SW2)

更新密钥
----------------------------------------------------

替换1或n个现有密钥，密钥标识符不变

添加1或n个新密钥，密钥标识符与现有密钥不同

MST磁安全传输支付
==========================================================

根据主账号(Primary Account Number, PAN)生成替代码（token），以支付标记、有效期替代传的卡号、卡有效期。

联网情况下，token在线下发。

在TEE的TA，或eSE的Applet应用在联网时生成一个标记号，备用于无网环境。


role
==========================================================

三类角色：nfc mobile（支持nfc的移动设备，例如手机、手表）；nfc reader（例如pos机）；nfc tag（与传统的rfid相同）。

如果一个nfc component有自己的embedded power source，那么它可以形成自己的RF field，做为active device初始化通信；如果没有embedded power source，那么它只能做为passive device，响应来自active device的请求。

也就是说，初始化通信的设备，必须是active device；而响应该通信请求的设备，可以是active device，也可以是passive device。

例如nfc tag就是passive device，只是存储数据，供active device读写。

operation mode
==========================================================

reader/writer mode: nfc mobile读写nfc tag。消息格式follow NDEF。

peer to peer mode: 两个active device进行半双工通信。消息格式follow NDEF，传输协议follow LLCP。

card emulation mode：nfc mobile模拟现有的各种智能卡片，例如信用卡、公交卡等等。由nfc reader初始化通信，nfc mobile应答。事实上，相当于nfc reader与nfc mobile上的SE application通信。

se的形态
==========================================================

embedded hardware：嵌入式的芯片，跟随nfc mobile整合。

secure memory card（smc）：跟着sd卡之类的存储卡整合。

universal integrated circuit card (uicc)：跟着sim卡整合。MNO或其他可信实体通过移动网络OTA。

security domain
==========================================================

issuer security domain (isd)：卡片管理者的安全域，一般是发卡方。

supplementary security domain (ssd)：应用提供方、或卡片管理者的安全域。

controlling authority security domain (casd)：是一种特殊的ssd，用于管控卡片上所有应用的安全策略。

trust service manager (tsm)
==========================================================

为application provider、MNO提供安全的交互服务，将nfc application安全的部署到用户侧的se中。

uicc 的 card content management models
==========================================================

simple mode：发卡方直接用MNO的OTA平台，service provider把所有的管理都授权给TSM。MNO负责执行卡片内容的增删改，TSM驱动应用的锁定、解锁、配置更新等。

delegated mode with full delegation to the TSM：TSM从MNO处获得预授权，TSM负责执行卡片内容的增删改。service provider自己也可以搞。

authorized mode with full delegation to the TSM：TSM无需从MNO获得授权，直接执行卡片内容的增删改。service provider自己也可以搞。

security
==========================================================

tag
----------------------------------------------------

防止内容cloning，无授权change，恶意替换

smart cards
----------------------------------------------------

invasive attack, side channel attack

communication
----------------------------------------------------

窃听，数据篡改，中间人攻击，重放攻击

stakeholder
----------------------------------------------------

se issuer, platform manager, ota provider

business model
----------------------------------------------------

MNO centric: MNO 作为 se issuer，全面管控

distributed: se issuer 跟 service provider 角色分离；通过tsm管控；

tsm centric: 不同service可以共用同一个tsm，省得重复建设

privacy
==========================================================

unobservability

unlinkability

pseudonymity

anonymity


