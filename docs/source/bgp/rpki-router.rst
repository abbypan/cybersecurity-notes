The RPKI to Router Protocol 
=========================================================

RFC6810  

Local Cache 周期性的获取 Global RPKI 数据

Router 从一个或多个 Cache 获取相关 RPKI 数据

serial number
==========================================================

Router 与　Cache 之间，增量更新是基于Serial Number的比对

Cache 更新Serial Number之后，会给当前连接的所有router发一个Notify，但并非强制要求router立即更新

由于certificate，roa的有效性依赖于时间，还需要1小时的容忍时间差

session id
==========================================================

cache 与 router 传输数据的 Session ID必须一致。如果不一致，回退，重置。

如果cache发现router复用了旧session id：如果 serial number已经差很多，cache reset；如果serial number差不多，cache response

如果router发现cache提供的内容与自身不一致，也会reset session

如果router发现cache response内容与当前自身状态一致，则可以等time period正常更新

protocol message type
==========================================================

根据PDU type区分

Serial Notify: 通知有更新

Serial Query: 询问cache是否有更新

Reset Query: 重置，获取全量数据

Cache Response: 增量/全量更新内容，主要看serial number

IPv4 Prefix:  as, ipv4 prefix, prefix length, max prefix length

IPv6 Prefix:  as, ipv6 prefix, prefix length, max prefix length

End of Data: 标识一次session的结束

Cache Reset: cache知会router，表示不支持增量更新；router可以选择Reset Query，或切换cache源

Error Report

protocol sequences
==========================================================

原文 sec 6

security 
==========================================================

cache validation 有效性验证

cache peer identification 双向身份认证

transport security 传输安全，TCP-AO, SSHv2, TCP MD5, IPsec。。。

IANA
==========================================================

rpki-rtr

rpki-rtr-tls
