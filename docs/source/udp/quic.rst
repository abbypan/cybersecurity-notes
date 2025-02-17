QUIC
#######

quick udp internet connections

`QUIC at 10,000 feet <https://docs.google.com/document/d/1gY9-YNDNAB1eip-RTPbqphgySwSNSDHLq9D5Bty4FSU/edit>`_

`QUIC FAQ for Geeks <https://docs.google.com/document/d/1lmL9EF6qKrk7gbazY8bIdvq3Pno2Xj_l_YShP40GLQE/edit#heading=h.h3jsxme7rovm>`_

目的是用 udp + quic 取代 tcp+tls+spdy 的组合

低连接时延
==========================================================

避免tcp + tls多次握手认证

首次通信 1-roundtrip handshake 认证，后续可以 0 roundtrip 复用

这个时延优化效果比较给力。但复用认证也需要弄一些安全措施加固。

拥塞控制
==========================================================

原始数据包 与 重传数据包 采用 独立的序列号，不复用。此时能够区分原始/重传ack，避免tcp拥塞反复重传。

ack还带上收包<->发出ack包的时间，提高roundtrip测量精度。

256 NACK range，相比于 TCP的SACK，对失序/丢包的容忍度更大。

`nak <https://books.google.com/books?id=9-kAI9-VwDsC&pg=PA297&lpg=PA297&dq=tcp+nak&source=bl&ots=BdWLiplmJu&sig=3uUmJyBeg4PX98X6VpkiJyEksp8&hl=zh-CN&sa=X&ei=fZw_VfD0LIeQyATcs4CoAw&ved=0CE4Q6AEwBg#v=onepage&q=tcp%20nak&f=false>`_

`sack <http://packetlife.net/blog/2010/jun/17/tcp-selective-acknowledgments-sack/>`_

反正是在udp上面嘛。。。

多路复用
==========================================================

spdy + tcp 能支持多个流，但是当tcp丢包时，就要傻等。

quic 基于 udp，某个流的udp丢包，其他流不受影响。

这个是继承udp的好处。

前向校验
==========================================================

开启FEC (forward error correction)选项，一组数据包即使有部分丢包，也可以通过FEC重组（不需重传）。

FEC选项由发送方决定是否开启。

这个选项明显消耗更多资源，我觉得有没有人愿意用很难说。除非是网络质量比较差的时候再开。。。

连接迁移
==========================================================

以client随机生成的64 bit连接ID进行标识，而不像传统的 srcip, srcport,dstip, dstport 四元组。

client换ip也可以复用之前的connection id，不用重新起连接。

这个还是N年的老问题，ip地址某种程度上存在 身份+寻址 复合作用，这里用连接ID硬拆开些。

数据包结构
==========================================================

`QUIC wire layout spec <https://docs.google.com/document/d/1WJvyZflAO2pq77yOLbp9NsGjC1CHetAXV8I0fQe-B_U/edit#heading=h.bnj2xqadb8oz>`_

包格式比较简单，没啥好说的。

Connection ID  0, 8, 32, 64 length

Sequence Number 8, 16, 32, 48 length

data packet 用的是 type + payload 结构，根据type类型解析不同payload。

传输安全
==========================================================

`QUIC Crypto <https://docs.google.com/document/d/1g5nIXAIkN_Y-7XJW5K45IblHd_L2f5LTaDUDwvZ5L6g/edit>`_

一般用 handshake + nonce 启动连接，用于防止源地址欺骗，重放攻击。

DNS协议是典型的无连接，所以上面两个问题自作自受（这个俺同意）。

QUIC 处理源地址欺骗
----------------------------------------------------

server 向 client 扔一个 source address token，相当于 authenticated-encryption block ，包含client源IP、server timestamp信息

client 回头带上这个token

client与server第一次初始连接、或者client切换ip时，server要生成新token。

token 生存时间控制（这个主要是防重放）

此外还有entropy bit机制检查数据包到达率（说白了除了丢包率高，就是伪装重放）：server 发包时设置一个 entropy bit，client收到后回复entropies的hash值，标识真的有收到。

求极端加速时去掉token限制，当server发现某个srcip得不到应答的数据包比率升高时，加强token限制（我觉得不太靠谱，坏蛋太多。。。）

QUIC 处理重放
----------------------------------------------------

TLS的connection是client/server双方握手扔nouce，确保两边都是fresh

QUIC要省时延，0-round trip 只让client直接往server扔nouce id；同时又要防重放（同一个connection不要来两次）：区分连接状态 + 要求时钟同步。

server 并不是检查 client nonce 是否 unique，而是检查 client nonce 在给定的"orbit" 内是否唯一。

(client nouce, orbit) <-> server 

client 缓存 server的orbit值，如果connection被route到别的server，每当server检验到orbit值不符，会重启handshake。

nounce里加上时间戳信息，还可以自然die，或者时间不对版直接重新handshake（类似于cookie时限）

已连接过的client nouce登记到 strike-register，如果回头一个connection在strike-register里，就识别为重放。

正常connection：timestamp正确，orbit正确，没在strike-register出现过 => 允许连接，更新到register

握手
----------------------------------------------------

QUIC server 提供静态 server config：连接参数 + Diffie-Hellman 公钥

server config 设置有效期，且以 server 私钥认证。

connection key 通过 Diffie-Hellman 交换实现: client 先用server config提供的D-H加密扔过去，server回复一个临时生成的D-H key给client（此次connection就用这个临时的）

第9页有client-server连接示意图，比较清楚（以前没连过，就inchoate，否则直接hello，server检查非重放，则返回当前连接的secure encryption，即完成rekey）

后面是字段细节，懒得看了，真用到再来查。

设计详情
==========================================================

`QUIC Design Document and Specification Rationale <https://docs.google.com/document/d/1RNHkx_VvKWyWg6Lr8SZ-saqsQx7rFV-ev2jRFUoVD34/mobilebasic>`_


感想
==========================================================

我觉得这个在**非强安全连接需求**的应用场景下，肯定可以的，前提是ISP有节操到不乱丢udp包。

此外就是把在内核实现的一些tcp连接控制有选择或者微调的在quic上实现了，优化策略主要围绕降时延。

其他相关细节主要是用connection id替代tls+tcp传统四元组标识连接，nouce之类用 srcip + timestamp + server orbit 绕开每次双向握手。

还有就是server侧存register信息，与tls双向临时nouce相比，以server的处理资源换连接时间。

注意inchoate连接server不会直接回一大堆server config信息，而是在source address token验证之后才给全。避免伪造源地址放大攻击第三方。
