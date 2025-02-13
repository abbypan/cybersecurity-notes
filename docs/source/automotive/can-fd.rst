CAN FD (FD=Flexible Data-rate)
==========================================================

`CAN FD传统CAN之比较 <https://www.kvaser.cn/wp-content/uploads/2015/04/comparing-can-fd-with-classical-can.pdf>`_

`CAN FD Protocol Specification <https://can-newsletter.org/uploads/media/raw/e5740b7b5781b8960f55efcc2b93edf8.pdf>`_

`CAN FD - The basic idea <https://www.can-cia.org/can-knowledge/can/can-fd/>`_

`CAN with Flexible Data-Rate –CAN-FD <http://www.aut.upt.ro/~pal-stefan.murvay/teaching/nes/Lecture_05_CAN-FD.pdf>`_

减小时延(峰值到8Mbit/s，均值到5.9Mbit/s)，提高带宽，增大Data Length(从8byte扩到64byte)，改进CRC。。。 

CAN FD 标准帧：帧起始(SOF 1bit)，仲裁段（ID 11bit，r1），控制段（IDE, EDL, R0, BRS, ESI, DLC），数据段(0~64byte)，CRC段(CRC 21bit，DEL），ACK段(ACK, DEL)，帧结束(EOF 7bit，ITM 3bit)。

CAN FD 扩展帧：帧起始, 仲裁段(ID 11bit, SRR, IDE, Extended ID 18bit, r1)，控制段(EDL, R0, BRS, ESI, DLC)，。。。

数据字节<16时，CRC为17bit。

EDL(Extended Data Length)用于区分CAN帧与CAN FD帧。

BRS(Bit Rate Switch)用于区分位速率。如果BRS=1 则所有位将与仲裁段使用相同的位速率发送；如果BRS＝0，则仲裁段之后，CRC定界符之前的所有位将使用较高的速率发送。

ESI(Error State Indicator)错误状态指示，主动错误1，被动错误0。

CAN FD中不含遥控帧，直接将CAN帧中的RTR位换成r1，可以直接用做CAN FD帧。

注意CAN FD的Data Length扩展到64字节，DLC字段值使用变长编码（传统CAN帧长度内仍是线性编码）

CAN FD将填充位纳入CRC计算中。。。

明显CAN FD比CAN更合适增加加密、认证等安全措施（数据段变长了）。。。
