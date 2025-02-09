KEMTLS
#########

background
================

KEMTLS: Post-quantum TLS without handshake signatures <https://eprint.iacr.org/2020/534>_

传统tls1.3场景中，服务端证书用于校验握手参数（包含临时公钥）的合法性。

KEMTLS的改进基于一个背景，pqc的签名太大，如果用kem就小很多。

使用kem，替换传统tls握手时的签名，需要传递的数据变少，保证时延。

kem可以选 sike, kyber, ntru-hps等

overview
==========================================================

client 发送临时PQC公钥pk_e

server 用pk_e封装临时secret（记为ss_e)，封装后的内容记为ct_e

server 以ss_e派生的对称密钥（记为K1, K1')，选取其中的K1加密自身证书Cert_pk_s

client 解密ct_e，获得ss_e，同样派生K1、K1'。

client 使用Cert_pk_s中的公钥pk_s封装临时secret（记为ss_s），封装后的内容记为ct_s

client使用K1'加密ct_s

server解密获得ss_s

双方使用ss_e || ss_s派生当前会话2个方向的4个密钥（for key confirm, for application)

summary
==========================================================

这里ct_s里的pk_s是固定的长期公钥，直接用于kem封装。

前向安全性的保证，主要在于服务端不存ss_e，客户端不存临时PQC私钥sk_e。

为了兼容性，只改了握手的部分，证书的合法性检验套路不变。
