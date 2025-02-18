Threat Model for BGP Path Security
==========================================================

RFC 7132  

PKI : address space holder 拥有该前缀

ROA : 前缀拥有者确认，某个AS被授权该前缀

注意BGP路由以AS为核心

INRs ( Internet Number Resources ) : IPv4 / IPv6 address space and ASNs

Network Operator 可能为调整路由，到对其自身更为经济的线路，而这个调整可能对其他相关方不是一件好事

repository system 是 RPKI 生效的关键所在，数据可用性，及时更新，稳定性。。。

如果错误在CA自身发生，则外部无法确定该错误，因为CA是下属节点的授权起点。而其下属的受灾户，则会迅速发现。。。

如果CA为overlapping INRs签名，则RPs也无法确认哪个是错误的重复分配。证书合法，不代表内容正确。。。

Security Requirements for BGP Path Validation
==========================================================

RFC 7353  

除AS_PATH之外的BGP attributes只应用于本地

数据面可能不按控制面提供的path走
