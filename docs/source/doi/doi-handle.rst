DOI & HANDLE
##########################################################

DOI : digital object identifier
==========================================================

`doi手册 <https://www.doi.org/doi_handbook/translations/chinese/doi_handbook/TOC.html>`_

DOI 号永久性地分配给一个对象，为该对象的当前信息提供可解析的持久性网络链接，这些当前信息包括该对象在互联网上的位置和资料等

doi:10.1000/123456 :  doi前缀 10.1000，doi后缀 123456

简单解析：解析到一个持久性URL；多重解析：支持解析出多条该对象的关联信息

使用HANDLE系统进行解析。支持代理服务器例如 http://doi.org/10.123/456

`运行本地 handle 服务 <https://www.doi.org/doi_handbook/translations/chinese/doi_handbook/9_OperatingProcedures.html>`_

全球 Handle 注册中心 (GHR)。本地 handle 服务 (LHS)。

每个前缀均在 GHR 中创建。前缀包括了各个LHS的唯一标识信息，如 IP 地址和服务器公钥。GHR 使用这些信息确定向何处发送 DOI 号解析请求。

Handle System 使用私/公钥或密钥。DOI 号的创建权限位于前缀层，但每个 DOI 号都有自己的管理员（通常为前缀记录中的一个群组）。

Handle
==========================================================

RFC 3650/3651/3652

Handle系统由CNRI开发

    <Handle> ::= <Handle Naming Authority> "/" <Handle Local Name>

以 ncstrl.vatech_cs/tr-93-35 为例，
- Client -> Global Handle Registry :  查询Local Handle Service信息 0.NA/ncstrl.vatech_cs
- Client -> ncstrl.vatech_cs 的 LHS : 查询handle信息 ncstrl.vatech_cs/tr-93-35

Client -> Handle服务器之间的通信支持：加密、认证、签名、隐私控制。支持 公钥+挑战码模式。

注意：缓存、代理服务器不做为Handle服务的一部分。
