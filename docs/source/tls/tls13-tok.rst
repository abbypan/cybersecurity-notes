Token Binding for 0-RTT TLS 1.3 Connections
################################################

`Token Binding for 0-RTT TLS 1.3 Connections <https://tools.ietf.org/html/draft-nharper-0-rtt-token-binding-01>`_

`The Transport Layer Security (TLS) Protocol Version 1.3 <https://tools.ietf.org/html/draft-ietf-tls-tls13-16>`_


概要
==========================================================

这个主要是Token Binding随tls1.3调整，结构上没有什么变化

之前Token Binding扩展是在client-sever密钥协商成功之后，对其中的EKM进行签名

而tls1.3引入的0-RTT机制，resumption旧的连接信息传0-RTT data

因此，如果0-RTT时还想要加Token Binding，就需要调整原来用于签名的EKM值定义以适应tls1.3

Exporter函数与tls1.3的7.3.3节一致：

    HKDF-Expand-Label(early_exporter_secret, label, context_value, key_length)

0-RTT EKM 其中，label为"EXPORTER-Token-Binding"，context_value为空，key_length为32字节

安全
==========================================================

限制0-RTT EKM生效的时间窗

0-RTT EKM只在0-RTT连接的单次生效，在handshake结束后切换回正常的1-RTT EKM定义

服务器拒绝0-RTT的Token Binding

攻击者如果知道<psk，ClientHello相关参数，之前上下文data>就可以解出TokenBindingMessage，构造重放。不过解不出响应内容

注意这个重放难度比tls13的0-RTT data难度要高一点
