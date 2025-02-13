CAN (Controller Area Network)
==========================================================

`Introduction to CAN <https://elearning.vector.com/index.php?wbt_ls_kapitel_id=1329975&root=378422&seite=vl_can_introduction_en>`_

`CAN入门书 <http://archive.eet-china.com/www.eet-china.com/ARTICLES/2006SEP/PDF/rcj05b0027_can_intro.pdf?SOURCES=DOWNLOAD>`_

`CAN基础 <http://d1.amobbs.com/bbs_upload782111/files_34/ourdev_599225UCE6YK.pdf>`_

`Introduction to the Controller Area Network (CAN) <http://www.ti.com/lit/an/sloa101b/sloa101b.pdf>`_

`CAN词典 <https://www.can-cia.org/fileadmin/resources/documents/publications/candictionary_v1_cn.pdf>`_

CAN 控制器根据两根线上的电位差来判断总线电平。

多个单元以CSMA/CD方式访问总线，同时发送则以ID (Identifier) 区分优先级。

标准：ISO11898 (高速，最高1Mbps, 总线最大长度40m/1Mbps，连接单元数最多30)及ISO11519(低速，最高125kbps，总线最大长度1km/40kbps，连接单元数最多20)。

帧格式
----------------------------------------------------

5种帧：数据帧、遥控帧(向某个单元请求某个ID的数据)、错误帧、过载帧(接收单元通知说尚未准备好接收数据)、帧间隔(将数据帧与遥控帧与前面其他帧分开)。

数据帧和遥控帧有标准格式和扩展格式两种格式。标准格式有11bit的ID，扩展格式有29bit的ID。

IDE = 0 标准帧，IDE = 1 扩展帧

DLC : Data Length Code, 4bit

CAN数据帧组成：帧起始(1bit)，仲裁段(ID 11bit，RTR 1bit), 控制段(6bit，含IDE, r0, DLC 4bit)，数据段(0~64bit)，CRC段(16bit, 含CRC 15bit, CRC界定符 1bit)，ACK段(2bit)，帧结束(7bit)

CAN扩展数据帧组成：帧起始(1bit)，仲裁段(ID 11bit, SRR 1bit, IDE 1bit, Identifier 18bit, RTR 1bit), 控制段(6bit，含r1, r0, DLC 4bit)，数据段(0~64bit)，CRC段(16bit, 含CRC 15bit, CRC界定符 1bit)，ACK段(2bit)，帧结束(7bit)

遥控帧不含数据段。

主要用于车身（车窗、座椅等）、状态（仪表、故障诊断等）、实时控制（发动机控制、变速控制等）系统。不同功能域的CAN总线会连接到网关。

安全
----------------------------------------------------

CAN协议是完全默认信任的，没有任何认证处理。

任何一个单元都可以发送任意高优先级ID的消息，抢占资源或引发拒绝服务。

任何一个单元都可以伪造其他ID的消息。

任何一个单元都可以窃听其他单元交互的消息。

安全方案：公私钥对+HMAC做认证(预置对端ECU公钥)，AES+HMAC做加密及认证，识别ECU发送的CAN电平信号特征做入侵检测及前期阻断。。。

相关HASH函数：FIP180 SHA2, SHA-256/384/512；FIPS 202, SHA3-224/512。




