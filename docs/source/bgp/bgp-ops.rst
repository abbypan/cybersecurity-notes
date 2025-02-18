运维解决方案
==========================================================

不修改路由协议
----------------------------------------------------

可加强路由过滤，例如地址信息更新、异常地址过滤、customer路由前缀信息检查。。。

IRR路由策略信息检验。。。

等等。。。

前缀劫持检测
----------------------------------------------------

与历史信息比对 + 主动探测，等等。。。 


BGP Operations and Security
==========================================================

RFC 7454  

BGP Operations and Security 的 BCP，主要是filter规则总结

对BGP Speaker的保护，避免破坏BGP会话

BGP TTL Security (GTSM) : 只有直接相邻的节点会出TTL=255的包，因此对直连peer有效

Prefix Filtering：各种预设不符的前缀，或太长的前缀长度，不许进/出，IPv6未分配地址约1个月的更新时延

如果某customer是多宿的，那么该customer的上游必须接受其对端peer/上游发过来的该customer prefix

如果已经有个一个IXP Lan prefix，就不要接受一个More-Specific的prefix，避免路由黑洞

On the Risk of Misbehaving RPKI Authorities 当authorities不规范操作时。。。

loose routing policy 可能破坏某些 security，然则strict可能造成大量traffic re-route到transit peers，伤害权衡看本地routing policy偏向
