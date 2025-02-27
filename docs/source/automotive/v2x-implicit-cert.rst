v2x implicit cert
#####################

doc
==========================================================

`Alternative Applications of the SCMS V2X Infrastructure <https://icmconference.org/wp-content/uploads/E21b-Romansky.pdf>`_

`Vehicle to Vehicle Safety Application using ECC PKI <https://csrc.nist.gov/csrc/media/events/workshop-on-elliptic-curve-cryptography-standards/documents/presentations/session2-lambert-rob.pdf>`_

`IEEE 1609.2 and Connected Vehicle Security: Standards Making in a Pocket Universe <https://www.researchgate.net/publication/311452790_IEEE_16092_and_Connected_Vehicle_Security_Standards_Making_in_a_Pocket_Universe>`_

核心是满足v2v场景的限定条件：短时有效、匿名化、批量申请、字节数尽量少

适合可信节点的匿名化提交信息场景、可信身份的匿名化校验场景。。。

ecqv implicit certificate
==========================================================

`SEC 4:  Elliptic Curve Qu-Vanstone Implicit CertificateScheme (ECQV) <https://www.secg.org/sec4-1.0.pdf>`_

`Implicit_certificate <https://en.wikipedia.org/wiki/Implicit_certificate>`_

字节数少通过ecqv的方式实现

butterfly key expansion
==========================================================

`IEEE 1609.2 and Connected Vehicle Security:Standards Making in a Pocket Universe <https://csrc.nist.rip/groups/ST/ssr2016/documents/presentation-tue-whyte-invited.pdf>`_

`A Security Credential Management System for V2X Communications <https://arxiv.org/pdf/1802.05323.pdf>`_

短时有效、匿名化、批量申请通过butterfly key expansion机制实现

performance
==========================================================

`Schnorr-based implicit certification: improving the security and efficiency of V2X communications <https://www.semanticscholar.org/paper/Schnorr-based-implicit-certification%3A-improving-the-Barreto-Simpl%C3%ADcio/af96f4189589b0fc8f611b6cc50dc1ac5769669d>`_

把cert的hash（即h值）的乘法，挪到CA的公钥点上，而不是像原来的临时中间点。

由于CA的公钥点固定，就可以提前算好，节省时间

batch operations
==============================

`Accelerating V2X Cryptography through Batch Operations <https://www.semanticscholar.org/paper/Accelerating-V2X-Cryptography-through-Batch-Bottinelli-Lambert/4ee8de0b546ea964b89747bb5f543df86a1326ef>`_

EE从PCA获得批量V2X证书时，通过把证书校验的左右式子分别批量加法，合并基点、PCA公钥的乘法，减少运算量

ECDSA*批量校验也类似，其实是基于PCA的公钥相同的预设。注意ECDSA*是ECDSA的变种。这个的问题在于，批量数目越大，平均等待时延也会随之增大，效果不好说。

