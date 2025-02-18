BGPsec Protocol Specification
##########################################################

`BGPsec Protocol Specification <https://datatracker.ietf.org/doc/draft-ietf-sidr-bgpsec-protocol/>`_

BGPsec_Path
==========================================================

用于源认证，处理路由劫持问题

BGPsec Capability format 的 dir 指示功能方向：0 收，1 发

一个前提是支持BGP multiprotocol扩展(rfc4760)

支持4字节AS号(rfc6793)

BGPsec_Path 与 AS_PATH 互不相容

BGPsec_Path 由两部分组成，Secure Path + Sig Blocks。Secure Path与AS_PATH类似。每个Sig Block中含有Secure Path中的每个AS的签名片段。为了向后兼容，可以有两个Sig Block。

Secure Path
----------------------------------------------------

Secure Path的片段格式（见Fig5）：1字节 pCount，1字节Flags，4字节AS号

pCount标识对应签名覆盖的as个数

Flags 是最左一位记为Confed_Segment flag，取1则标识该bgp update message是发向与当前peer相同AS confederation的

Sig Blocks
----------------------------------------------------

每个Signature Segment格式（见Fig7）：20字节SKI，2字节sig长度，最后是sig

SKI就是对应的RPKI证书

链式的hash签名见Fig9，[ Secure_Path Segment: 1, Secure_Path Segment: 2, Signature Segment: 1, ..., Secure_Path Segment: N, Signature Segment: N-1 ]，中括号里的内容即为Signature Segment N将要hash+sign的data

注意事项
==========================================================

BGPsec speaker为每个对端生成独立的BGPsec update message

如果BGPsec router收到消息含AS_Path，不能换成BGPsec；如果没有AS_PATH，例如来自内部对端节点的NLRI，则可以加BGPsec往外发

为减少DDoS风险，先过简单的语义合规检查，再做签名验证

注意BGPsec是保护BGP消息不受篡改、正确性，但是BGP消息本身的合理性是另一层面的问题
