Problem Definition and Classification of BGP Route Leaks
==========================================================

RFC 7908  

参考文献是一堆route leak accident，可以重点浏览

route leak : 路由宣告被传播到非预期的scope，违反了AS_PATH中的某些AS的预期策略。多数由于误操作引起

后果：流量被重定向到其他path，导致窃听、过载、丢包、。。。

offending AS : 错误发布了违反预期策略的路由宣告的AS

有几种类型：

* Hairpin Turn with Full Prefix: 上游A -> offending AS customer -> 上游B，同等条件下，上游B会一般会优选来自customer的宣告；数据包经由offending AS到达目的地，如果offending AS无法处理这些重定向的流量，则可能造成丢包

* Lateral ISP-ISP-ISP leak: ISP A -> offending AS ISP B -> ISP C，通过全局BGP update进行detect，经验认为三个大型ISP之间不会这么乱买transit，当然如果真的买了就是known case标记处理

* Leak of Transit-Provider to Peer:  上游A -> offending AS customer -> lateral peer

* Leak of Peer Prefixs to Transit-Provider:  Lateral Peer -> offending AS -> 上游

* Prefix Re-origination with Data Path to Legitimate Origin: re-origination/mis-origination，offending AS从某个上游ISP获知route，又修改该route，使得另一个上游ISP误以为该offending AS才是Origination，此时，数据包可经由offending AS到达目标地址，也可能被offending AS中间误丢

* Accidental Leak of Internal Prefixes and More-Specific prefixes: 把内部的prefixes错误的宣告给Transit-Provider/ISP peer，由于地址前缀长度的优选变化，可能导致流量没法走best path。

总结：1-4都是传播route给错误的对象，5是篡改，6是内部信息错误发布到外面


Methods for Detection and Mitigation of BGP Route Leaks
==========================================================

`Methods for Detection and Mitigation of BGP Route Leaks <https://tools.ietf.org/html/draft-ietf-idr-route-leak-detection-mitigation-06>`_

per-hop route-leak protection field in BGP update: RLP

只有OV(Origin Validation)，没有BGPsec；或者 OV + BGPsec

RLP在没有部署BGPsec的场景下，也可使用

阻止local AS的route leak；检测并缓解收到的上游 AS route leak

neighbor AS的3种类型：1) Privder; 2) Customer; 3) Lateral Peer; 4) Complex : 不同前缀，角色不同，但共用同一条AS link

neighbor AS的关系，可经带外确认

local AS
----------------------------------------------------

Non-Transitive BGP pRLP Attribute for Intra-AS Messaging

Ingress (receiving) router: 如果从provider/lateral的对端接收，则置位；如果从customer，则否。

Egress (sending) router: 把一个未标记pRLP的route消息发给neighbor；如果已标记pRLP，则不能发给transit-provider或lateral peer

downstream AS
----------------------------------------------------

0: nothing specified

1: 禁止接收者转发该route到transit-provider/lateral peer 

注意RLP是per prefix,而非per hop/per AS

因此，当发update给customer/lateral peer时，RLP=1；当发update给transit-provider，RLP=0

不能重写别人的RLP

BGP RLP Attribute and BGPsec Flags
----------------------------------------------------

AS_PATH : { ASN: N, RLP: 1/0 }, ..., { ASN: 1, RLP: 1/0 }

BGPsec 则是在Flags field里，是签名数据的一部分

检测route leak
----------------------------------------------------

如果router接收到的update是从customer/lateral peer发过来的，且RLP=1，则标记为Route Leak

当然，与自身直接相邻的，不用看RLP也知道是否route leak

优选non-leak的路径，clean优先于"prefer customer"

优选"经ROA验证通过的customer"的宣告，如果某个route的前缀未经ROA验证，但由某个customer宣告，有可能是leak

此外考虑ddos Mitigation的流量牵引

与其他方案的比较
----------------------------------------------------

prefix filtering 在于customer cones的精度，尤其是多层分发的场景

AS path based Outbound Route Filter (ORF)　在于AS关联视图，没到per prefix

Per-Hop RLP Field or Single RLP Flag per Update
==========================================================

Per-Hop RLP Field: AS_PATH里的每个AS都有一个RLP标记

Single RLP Flag per Update: 只有源AS标记RLP，其余AS只转发不再添加RLP

显然Per-Hop提供的信息量更多点，方便识别leak

而Single RLP提供的消息不一定足以让接收方100%标记到leak AS（此时如果leak的路径来自customer，很可能属于优选)
