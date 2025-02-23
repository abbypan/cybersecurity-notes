alpn
#########

RFC7301 ALPN: Transport Layer Security (TLS) Application-Layer Protocol Negotiation Extension

在tls连接阶段，把tls上层的协议也选好，可以省1个RTT。

client在client hello中带上自身支持的应用层协议类型，server在serverhello中返回选取的应用层协议类型。

典型场景例如，web server选择over tls的http/1.1, http/2, spdy/2。
