FlexRay
==========================================================

`FlexRay Specification <https://elearning.vector.com/index.php?wbt_ls_kapitel_id=508201&root=378422&seite=vl_sbs_introduction_en>`_

`The FlexRay Protocol <https://www.ece.cmu.edu/~ece649/lectures/23_flexray.pdf>`_

`FlexRay Module Training <http://www.ti.com/lit/ml/sprt718/sprt718.pdf>`_

`FlexRay Communications System Protocol Specification <https://svn.ipd.kit.edu/nlrp/public/FlexRay/FlexRay%E2%84%A2%20Protocol%20Specification%20Version%203.0.1.pdf>`_

`FlexRay 通訊協定與設計 <https://www.artc.org.tw/upfiles/ADUpload/knowledge/tw_knowledge_IA-96-0025.pdf>`_

`Overview of FlexRay scheduling issues <http://retis.sssup.it/~marco/files/lesson10-introduction_to_FlexRay.pdf>`_

FlexRay 主要用于线控(X-by-Wire)、底盘控制、引擎控制、动力总成。

高带宽、高容错(确定性)、低延迟，单channel最高10Mbps，双channel可以达到20Mbps(非冗余模式)。

节点不需要同时连两个channel，也可以只连一个channel。

支持总线、星形拓扑。

支持频率、相位同步，单节点故障不会干扰其他节点的同步。

帧格式
----------------------------------------------------

含3个部分：Header, Payload, Trailer

Header: Indicator(5bit, Reserved, Payload Preamble, Null Frame Indicator, Sync Frame Indicator, Start Frame Indicator), ID(11bit), Payload Length(7bit), CRC(11bit), Cycle Count(6bit)

Payload: 0~254 Bytes

Trailer: CRC(24bit)

可见11bit的ID支持定义2048种FlexRay帧。

Header里的CRC针对前面的内容进行校验。

分时
----------------------------------------------------

TDMA (Time Division Multiple Access) 分时

每个Cycle含四个部分：Static Communication Segment, Dynamic Communication Segment, Symbol Window, Network Idle Time

Static Communication Segment内按frame ID，每种帧获得固定的时间

Dynamic Communication Segment内如果某个frame ID没有发消息，则在指定的短时间内结束该frame ID占用的time slot，轮到下一个frame ID

安全
----------------------------------------------------

FlexRay 主要优势是通过分时time slot确定传输的frame，避免冲突。其次是低时延、以及相比于CAN的高吞吐量。

总线拓扑问题与CAN相同，默认也没有针对Frame ID来源合法性校验，网关增强。。。

星形拓扑可以在star节点做一些处置。
