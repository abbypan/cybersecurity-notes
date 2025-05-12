IPv6 Security
#################

NAT阻止认证首部，在NAT场景仅能加密载荷，破坏端到端通信模型。

IPSec是端到端安全。

IPv6能提供全局唯一地址，无需NAT。

具有巨型扩展首部链的数据包可能是危险的。

路由器不再执行分段和重组，避免大数据包的穿越。

RFC3971 安全邻居发现SEND

RFC3972 CGA方案：

    修饰符+该节点公开密钥+子网前缀 => SHA1 => 接口标识符

    子网前缀 + 接口标识符 => 网络地址


doc
======

`Eric Vyncke - IPv6 Security Vendor Point of View <https://www.slideshare.net/slideshow/eric-vyncke-ipv6-security-vendor-point-of-view/6342224>`_

`NIST IPv6 Profile <https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.500-267Ar1.pdf>`_
