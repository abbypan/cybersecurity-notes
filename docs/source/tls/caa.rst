CAA
######

RFC6844, DNS Certification Authority Authorization (CAA) Resource Record

`Baseline Requirements for the Issuance and Management of Publicly-Trusted Certificates <https://cabforum.org/wp-content/uploads/CA-Browser-Forum-BR-1.5.4.pdf>`_

结构
==========================================================

CAA RR: 域名拥有者指定某些CA信息，这些CA授权发布该域名的证书。

CAA RR: flag ，property tag-value

.. note::

    $ORIGIN example.com
       .       CAA 0 issue "ca.example.net"
       .       CAA 0 iodef "mailto:security@example.com"
       .       CAA 0 iodef "http://iodef.example.com/"


flag
----------------------------------------------------

    Issuer Critical : flag = 1，issuer必须能正确解释对应的property tag-value,否则不能签发证书 。

property tags
----------------------------------------------------

    issue <Issuer Domain Name>  [; <name>=<value> ]*
    issuewild <Issuer Domain Name>  [; <name>=<value> ]* : 为当前域名发布wildcard certificates的issuer域名信息
    iodef <URL> : issuer发布的一些信息


查找
==========================================================

假设域名为X，则查找该域名CAA的顺序：

A(X) 是 X 的 CNAME/DNAME 目标值，P(X) 是 X 的上级域。

.. note::

    R(X) = CAA（X） ||  R(A(X))  || ( X不是TLD ; R(P(X)) ) || Empty

分析
==========================================================

查找：CDN CNAME场景下，A(X)显然不好设CAA。TLD同理，一般不会设CAA，所以上溯到TLD是干啥？

错误发布CAA，运维故障

伪造CAA，没有DNSSEC部署率就约等于DNS劫持+MITM风险。相对来说，浏览器采用HTTP Public Key Pinning (HPKP) 会更实际且经济一点。

CAA记录更新，类似DNSSEC KSK rolling
