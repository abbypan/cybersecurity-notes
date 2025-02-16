BGP Security Protocol
==========================================================

`BGP安全之争 <http://www.lxway.com/56552054.htm)>`_  sbgp跟sobgp的组件

SIDR：

AS是否拥有某IP前缀的合法授权？(origin AS的真实性)

BGP路由中的AS_PATH是否与其NLRI实际传播路径一致？(AS_PATH的完整性)

S-BGP
----------------------------------------------------

BBN公司，Stephen Kent

ISP自身有两套证书：IP证书，AS证书

ISP再签发两套证书：自身拥有的AS证书，BGP路由器证书

地址证明（Address Attestations）：ISP签发，该AS被ISP允许通告其拥有的IP地址前缀

路由证明（Route Attestations）：BGP路由器签发，BGP路由对等体的AS号是否能继续通告该IP前缀的授权

用ISP公钥校验地址证明，用路由器证书逐跳校验AS_PATH中的路由证明

需要引入两套PKI，路由收敛时间长、计算开销大、内存需求大

soBGP
----------------------------------------------------

思科

第三方认证，网状信任

EntityCert：AS证书，标识某实体确实拥有该AS，由第三方进行签名（网状信任）

AuthCert：源授权证书（Origin Authorization Certificates），AS拥有IP前缀

注意AuthCert带有上一级AS的EntityCert的指针

ASPolicyCert：AS策略证书（AS Policy Certificates）, 当前AS与其连接的对等体AS列表证书

各AS可以通过“AS策略证书”构建AS的网络拓扑图，从而判断路由通告中的AS_PATH属性是否真实

soBGP机制只能提供路由源验证，无法保证路径验证

RPKI + BGPSec
----------------------------------------------------

RPKI 负责对互联网码号资源INR（包括IP地址前缀和AS号）的所有权和使用权的认证，进行合法路由源（Origin AS）的验证

BGPsec在RPKI的基础上，引入BGPsec_Path，进行AS_PATH的验证

基于DNSSEC
----------------------------------------------------

`DNSSEC protected routing announcements for BGP <https://tools.ietf.org/html/draft-donnerhacke-sidr-bgp-verification-dnssec-04>`_
通过dnssec认证bgp宣告，draft没成

`rover <https://www.nanog.org/meetings/nanog55/presentations/Tuesday/Gersch.pdf>`_
地址源认证

基于DNSSEC部署成果。。。

难点
----------------------------------------------------------

`BGP: the Tragedy of the Commons <http://blog.ipspace.net/2017/12/bgp-tragedy-of-commons.html>`_

`Do We Really Need a New BGP? <https://rule11.tech/do-we-really-need-a-new-bgp/>`_

`Securing BGP: A Case Study <https://rule11.tech/securing-bgp-case-study/>`_

BGP的任务：宣告自身拥有的前缀，标记邻居连接信息及路由策略，接收处理邻居路由更新信息，过滤伪造路由信息

运维错误没法通过技术手段避免，合法不等于正确，legal vs correct

三个主要问题：宣告不属于自身的前缀，宣告错误路径，内部路径泄漏

层次信任 vs 网状信任

一个AS可能并不想公开自身与其他AS的全部连接信息，可观察 vs 全宣告，即 observe vs claim

单AS唯一公钥 vs 每路由器一个公钥，per AS vs per eBGP speaker, 涉及前缀变动时的签名更新、接收路由更新、证书撤销等等的成本考量

任一AS_PATH路径上的所有路由必须做签名计算，因为是链式签名的连续开销，签名各各不同

如果有效签名的合法信息已失效，如何确保其他AS同样更新？如何防重放？

如果维护全局签名撤销集中数据库，开销、效率？

如果在BGP update里加入时间戳信息，那么全局更新频率设置，以及全球路由器的路由计算负载

If you haven’t found the tradeoffs, you haven’t looked hard enough.
