RPKI
##########################################################

RFC6480  

包含3部分：Resource Public Key Infrastructure(RPKI)，ROA 对routing objects签名，distributed repository system存放PKI objects和ROA

IANA / RIR (Regional Internet Registry) / NIR (National Internet Registry) / LIR (Local Internet Registry) 分发地址

ISP基本上可看所是LIR

Certification Authority (CA) Certificates: 用于证明resource holder确实拥有该AS以及AS下辖的IP prefix。一个CA不能为其下属的两个不同实体签发相同名称的证书。

End-Entity (EE) Certificates: 每个EE只用于签发一个object，与ROA/manifests打包使用，一起撤销，同生同灭。由于只用于签名验证，也不需复杂的私钥存储管理之类。

Trust Anchors (TA): 每个 relying party (RP) 需要选择其 trust anchors (TAs) 做为PKI信任的起始。RP可以选择建立并管理自身的TAs，因为它自己也可以多层次分发资源。

ROA
==========================================================

Route Origination Authorizations (ROA): 资源拥有者认证了，某个AS拥有某些prefix。一个ROA里只有一个AS。BGPsec基于此验证源路由。ROA随EE生成，随EE撤销。

示例见Figure 1，

RIR, CA -> ISP, CA -> ISP, EE -> ROA

NIR, CA -> ISP, CA -> ISP, EE -> ROA

Repositories
==========================================================

存放CA, CRLs, manifests, ROA

支持外部pull上述数据

注意数据时效性，数据更删等关键操作的访问控制

一般来说，Registry的RP含有用其CA签发的所有CA，EE，CRLs, manifests数据，LIRs/ISPs的RP里还有ROAs.

每个Certificate有其对应的URI，层次指针示例见Figure 2。假设Cert A签发了Cert B/Cert C。则Cert A中的Subject Information Access(SIA)指向A的Responitory Publication Directory，在该Directory中包含了Cert B/Cert C。并且，Cert B/Cert C中的 Authority Information Access(AIA)指向Cert A, Cert B/Cert C中的CRL Distribution Points(CRLDP)指向A的CRL。

通过这种SIA/AIA层次结构URI指针的路径保持稳定性，当某个CA certificate is reissued **with the same public key**, 下面的certificates不用再重新签发一次。当CA certificate的key更新时，也只要随之更新该certificate直接签发的对象，而不用签发整个下级子树。

RP数据可用rsync增量更新

Manifests
==========================================================

manifest里存的是当前Repositories里的signed object list，即certificate/CRL/ROA之类每个object的filename及hash

manifest本身也要被对应的certificate签名

ROA的manifest生存期与EE certificate一致

manifest内容包含：编号，签发时间，预计下次签发时间，<filename, hash>的列表

Local Cache Maintenance
==========================================================

RP要获取相关certificates, manifests, CRLs

验证manifest签名，检查失效时间

验证manifest中的CA, CRLs

验证EE

Common Operations
==========================================================

注意resource holder **不能** 对多个CA certificates配置相同的public key

由于subject name由上级issuer指定，那么同一个subject在不同上级issuers签发的certificates里会被指定为不同的subject name

resource holders管理ROA的原则是，make before break，因为invalid后果可能严重

multi-homed subscribers
----------------------------------------------------

如果一个subscriber接入了多个上级ISP

那么该subscriber最好申请一个专用AS号。一种方案是由primary ISP签发CA certificate，然后subscriber签发ROA。另一种方案是由primary ISP签发ROA，注意其他ISP提供的prefix也在这个ROA里一起签发。

没有专用AS号，就只能各个ISP各签一个ROA，不推荐。
