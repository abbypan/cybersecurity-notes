LIN (Local Interconnect Network)
==========================================================

`LIN Specification <https://elearning.vector.com/index.php?wbt_ls_kapitel_id=508184&root=378422&seite=vl_sbs_introduction_en>`_

`LIN入门 <http://www.jingbei.com/xxpdf/R8C%20lIN%E5%85%A5%E9%97%A8.pdf>`_

`LIN Protocol and Physical Layer Requirements <http://www.ti.com/lit/an/slla383/slla383.pdf>`_

`LIN Bus <http://www.wangdali.net/lin/>`_

`Introduction to the Local Interconnect Network (LIN) Bus <http://sine.ni.com/np/app/main/p/ap/icomm/lang/en/pg/1/sn/n17:icomm,n21:9536/fmid/2955/>`_

ISO 17987，SAE（Society of Automotive Engineers）负责分配ID。

串行通信协议，1主N从式的结构，由master发指令，对应的slave应答。master接入上层ecu。

12V电压，节点数<=16，最高20kbps，适用于远距离节点间的低速通信。

主要应用在车身系统(对安全和整车性能影响不大)，比如门窗、雨刮、空调、座椅、照明、后视镜。

由进度表schedule控制frame时隙。master节点必须设置较高精度的时钟(作为时间基准，保证位速率），而slave节点则不必。


帧格式
----------------------------------------------------

帧由两部分组成：帧头（master发送）、应答（slave发送）。

帧头Header：sync break (14bit), sync (10bit), PID (Protect Identifier, 10bit)

应答Response：Data(10~80bit)，checksum(10bit)

帧类型
----------------------------------------------------

无条件帧 Unconditional Frame：一旦有帧头发出，必须有从机无条件应答

事件触发帧 Event Triggered Frame：一次查询多个slave信号是否发生变化。只有变化的slave才需要应答；如果所有slave都没变化则该帧只有头部段没有应答段；如果有多个slave变化导致应答冲突，则通过进度表解决冲突，进行轮询处理。

偶发帧 Sporadic Frame：master的信号发生改变，此时master作为发布节点(sender)，把信号发给slave节点。

诊断帧 Diagnostic Frame：主要用于配置、识别和诊断。master作为sender的帧ID为0x3C，slave作为sender的帧ID为0x3D。

保留帧 Reserved Frame：帧ID为0x3E/0x3F，用于未来扩展。

规范
----------------------------------------------------

LIN规范定义配置功能的服务时，参照了ISO制定的UDS(Unified DiagnosticServices，车辆统一诊断服务)标准和OBD(On-board Diagnostic，车载自动诊断)标准。配置功能各项服务及其SID都是ISO标准的子集。

Node Capability Files(NCF), 定义了节点名称和节点的属性值，包括产品代号、位速率、帧的定义等信息。

LIN Description File (LDF)，LIN子网设计工具收集到节点性能文件的信息，自动生成LIN描述文件(LDF)。LDF包含了整个子网的信息，包括所有的信号和帧的声明，以及进度表等信息。

安全
----------------------------------------------------

LIN比CAN更加简单，主要在于master节点、以及master节点上级ECU的安全控制。

slave节点由于计算资源限制，除了接收master指令外，一般别无处理。
