SOME/IP
###########

基础
==========================================================

`Scalable service-Oriented MiddlewarE over IP (SOME/IP) <http://www.some-ip.com/index.shtml>`_

`SOME/IP Protocol Specification <https://www.autosar.org/fileadmin/user_upload/standards/foundation/1-0/AUTOSAR_PRS_SOMEIPProtocol.pdf>`_

SOME/IP 用于车载ECU之间的Remote Procedure Call (RPC) 通信，inter-ECU Client/Server Serialization

remote procedure calls, event notifications, underlying serialization/wire format

AUTOSAR支持解析SOME/IP的RPC PDU(protocol data units)，将信号传到目标app

Protocol Specification
==========================================================

service 提供 events, methods, fields

Events 提供 周期性发送的data，或者当data有变化时从provider传给subscriber

Methods 提供 subscriber 调用 remote procedure calls, 而在provider侧执行

Fields 有下述3种：
- 当data有变化时，由provider -> subscriber送notifier
- subscriber向provider查询value，getter
- subscriber更新provider的value，setter

notifier of events 只在change时发送

notifier of fields 在subscription后就send the data

Format
==========================================================

Message ID (Service ID/Method ID) 32bit 整个系统唯一；Service ID 16bit, 0 (1bit), Method ID 15bit；Service ID 16bit, 1 (1bit), Event ID 15bit

Length 32bit

Request ID (Client ID/Session ID) 32bit 对provider而言，无需了解request ID的内部结构；Client ID 16bit, Session ID 16bit；每次调用后，Session ID应该increment

Protocol Version 8bit, Interface Version 8bit

Message Type 8bit 第3高位的bit如果置1，则是TP-Flag，标识当前SOME/IP消息是一个segment

Return Code 8bit

Payload (variable size)

transport protocol
==========================================================

data > 1400bytes, no hard latency requirement => TCP

very hard latency requirement < 100ms => UDP

data > 1400bytes, hard latency requirement => UDP + SOME/IP-TP
