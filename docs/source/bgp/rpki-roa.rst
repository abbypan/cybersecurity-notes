RPKI ROA
############

RFC6482

ROA: Route Origin Authorizations 路由源地址认证

注意每个ROA里只含一个AS

ROA结构
==========================================================

ROA : { version, asID, ipAddrBlocks } 其中，ipAddrBlocks是ROAIPAddressFamily地址序列

ROAIPAddressFamily : { addressFamily, addresses } 其中，addresses是ROAIPAddress地址序列，Address Family Identifier(AFI)区分ipv4/ipv6

ROAIPAddress : { address, maxLength } 其中，address是IPAddress的bit string，maxLength为最大前缀长度，如果未指定maxLength，就限定只能为地址中的前缀长度

ROA使用
==========================================================

用ROA验证路由宣告之前，需要事先验证ROA

注意ROA内容完整性校验支持用的是CMS: Crytopgraphic Message Syntax

RPKI ROA 部署问题
=======================

invalid的处理
----------------------------------------------------------

例如，上级A拥有 某个 /16 前缀的地址段，且划分了该地址段的某个 /24 子段给下级B

如果上级A部署了RPKI，宣告了 /16 的ROA

但下级B未部署RPKI时，可能导致 /24 的路由验证为invalid

这个时候，路由可能异常

---->>>>

我觉得这个是链式认证的断掉的invalid，而不是路由条目层面的invalid

思路类似DNSSEC链式部署，即使没有部署DNSSEC，依然能够"尽力而为"的查到一个应答

分配
----------------------------------------------------------

向下级分配不属于自己的AS资源(A->B)，由于链式认证，外部不会有影响

向两个下级重复分配资源（A->B1，A->B2），B1与B2冲突

由于ROA合并签发机制，如果一个CA签发的ROA里单个AS的IP前缀有对有错，那么好人也会遭殃（别人不信，影响程度与该AS的覆盖有关），所以问题的关键是：由于同一个CA只能针对一个AS签发一个合并的ROA，当该AS的IP前缀覆盖面越大，出错后的打击面也越大
