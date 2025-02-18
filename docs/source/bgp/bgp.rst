BGP
#######

资料
==========================================================

`BGP rule11 <https://rule11.tech/bgp/>`_

`BGP Routing Security <http://moo.cmcl.cs.cmu.edu/~dwendlan/routing/) 一些资料索>`_

`whois.bgpmon.net <https://whois.bgpmon.net/index.php) 可以查bgp异常监测>`_

案例
==========================================================

`2014.03.17-Google DNS servers suffer brief traffic hijack <http://mobile.itnews.com.au/News/375278,google-dns-servers-suffer-brief-traffic-hijack.aspx>`_

路由泄漏 `More Leaky Routes <http://www.potaroo.net/ispcol/2015-06/tmleak.html>`_

More Specifics 问题
----------------------------------------------------

`More Specifics in BGP <http://www.potaroo.net/ispcol/2017-06/morespecs.html>`_

Type I - Hole Punching : more Specifics与origin有不同的出口

Type II - Traffic Engineering：more Specifics出口与origin相同，中间路径不同

Type III - Overlay：多个more Specifics与origin的路径完全相同

内容描述比较简单直接，重点在文章最后一句

BGP安全研究 
==========================================================

`BGP安全研究 <http://www.jos.org.cn/ch/reader/view_abstract.aspx?file_no=4346>`_

前缀劫持
----------------------------------------------------

internet地址分配：IANA -> RIR -> LIR

某个AS宣告一个未获授权的前缀，该前缀事实上属于其他AS或尚未分配。

运维：IGP到BGP的路由重分发配置出错，容易导致此类问题。

恶意：伪造BGP Update中的NLRI信息，某个AS向外宣告到<别的AS所拥有的IP段>的路由，其问题是同一个前缀被多个AS通告(MOAS, multiple origin as)

恶意：伪造NLRI信息和AS_PATH路径，某个恶意AS伪造前缀并声称自身是对端节点到该前缀的较优路径的一跳

路由泄漏
----------------------------------------------------

路由通吿给了不合适的对象，导致流量重定向

通告信息本身是合法的，只不过一般是违反了通用处理策略。路由认证机制无法解决该问题。

尤其是从 customer -> peer -> provider 的链条来说，考虑经济利益，通用的出站策略

与TCP协议相关的安全性
----------------------------------------------------

TCP攻击导致BGP消息update困难 


BGP Security Analysis
==========================================================

RFC4272 : BGP Security Vulnerabilities Analysis

BGP问题可导致：流量被传到没法deliver it的节点，挂掉；网络拥塞；丢包，黑洞；延迟；循环；窃听；分区，自以为流量与其他分离，事实则否；分割，自以为不会被路由到某些网络，事实则否；churn，快速变化；不稳定，不保证能到；过载；资源耗尽；地址伪造。

BGP攻击：窃听，重放，插入，删除，篡改，中间人，拒绝服务。。。

BGP脆弱性：完整性、时效性，节点认证，NLRI源AS校验，path attributes的源AS认证

合法的BGP peer如果出错？

各种加签名。。。

BGP消息自身保护，可达性。。。

