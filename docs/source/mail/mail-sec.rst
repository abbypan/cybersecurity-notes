SPF 
==========================================================

`Anti-spoofing for Custom Domains (SPF, DKIM & DMARC)  <https://protonmail.com/support/knowledge-base/anti-spoofing/>`_

`SPF and DKIM settings <https://wiki.vtiger.com/vtiger6/index.php/SPF_and_DKIM_settings>`_

域名foo.com的SPF记录指定负责发送邮件的服务器列表 

xxx@foo.com -> yyy@bar.com

当bar.com的mx收到一封声称来自xxx@foo.com的邮件时，可校验其源IP是否匹配foo.com的spf记录

`SPF_Record_Syntax <http://www.openspf.org/SPF_Record_Syntax>`_

.. note::
   
    $ dig google.com txt +short
    "v=spf1 include:_spf.google.com ~all"
    $ dig _spf.google.com txt +short
    "v=spf1 include:_netblocks.google.com include:_netblocks2.google.com include:_netblocks3.google.com ~all"
    $ dig _netblocks.google.com txt +short
    "v=spf1 ip4:64.233.160.0/19 ip4:66.102.0.0/20 ip4:66.249.80.0/20 ip4:72.14.192.0/18 ip4:74.125.0.0/16 ip4:108.177.8.0/21 ip4:173.194.0.0/16 ip4:209.85.128.0/17 ip4:216.58.192.0/19 ip4:216.239.32.0/19 ~all"

DKIM
==========================================================

`DomainKeys Identified Mail (DKIM) Signatures <http://dkim.org/specs/rfc4871-dkimbase.html>`_

域名的DKIM记录用于提供邮件签名验证的公钥信息

xxx@foo.com -> yyy@bar.com

xxx@foo.com的mta发出邮件之前，对邮件进行hash摘要，然后私钥签名。

当bar.com的mx收到一封声称来自xxx@foo.com的邮件时，可获取邮件中指定的dkim域名的公钥信息，进行签名校验，确认邮件内容未经篡改。

.. note::

    #邮件头部信息，$s=xxx, $d=xxx，$s._domainkey.$d 是登记dkim公钥信息的域名
    DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
            d=google.com; s=20161025;
            h=mime-version:reply-to:precedence:list-unsubscribe:feedback-id
             :list-id:message-id:date:subject:from:to;
            bh=Ak4PgiDB78kK8hwVXuCNWO80SC6Z7MZ/g87eUi7tAlo=;
            b=UB/RH6YW0MM3zrG3YKoNqMscj30p5/0CZKlO3buevvLCVwc839y21eujpWwonTGCH+
             bcwDUuAoB4Pm2QueSgwradkG1C5aNOJAF9+PMU6vJUGugAeF5ehwhvUNogmKCnbg5MNu
             vOJhzEY0VSN0/X2Zt52AtDXGtvJD9+UxEGdZSNHmeWNVHntngGprFzAvvEdXGMPut4/u
             sr5+Z3TmmG3UgxbP82aCUKw4vTybmoEcH9ifZKxAy3k/ZuBApe/iAox3jW2TYlVTan97
             /+CoxLW/kbe99rnLDhnUr0Nmx4nUYZtJbo7FyrSMH+p24DF2HH3ZpY3UW7esAsSVV3Y0
             A2rw==

    $ dig 20161025._domainkey.google.com txt +short
    "k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwXNZF1j8sJPDleRjf9SPBNem0ik58kF1ilC1nUgKAttl9v7FX9hXJXPmLNhVtSKVZ8yruaeOZLeIxtgtk1s81zzIE5Mj0AiGn2wlFt4kYfqlDfYe95YLQHjynu4i7vj1Tj" "ksf62btcCbL+3XhbK+oD5PlqYhXHWuzoKoEp5L4lCihgkONvU/oy7NNeE6quqfF/y0YSLwF2WVA2Kd8L6R0Ar2dYT/3wZCFknI7xhvPqh9HNcIWBELGPwtXcsHbX1wvBlCgNQAUcdJrf2YWzAwqmZ564/1ipL1IMk1nafPJk75ktumVNz6ORuIn3jbZWp9rRpnaeI9cu/8KfSKH2EY9QIDAQAB"

DMARC
==========================================================

`DMARC: RFC7489 <https://tools.ietf.org/html/rfc7489>`_

xxx@foo.com -> yyy@bar.com

当SPF或DKIM不通过时，bar.com的mx如何处理来自foo.com的邮件，例如，可以reject，把相关拒绝记录聚合通报到rua邮箱

.. note::

    $ dig _dmarc.google.com txt +short
    "v=DMARC1; p=reject; rua=mailto:mailauth-reports@google.com"

另一块是如果发现foo.com是垃圾邮件域，则注册商或权威服务器可以采取部分操作屏蔽该域名

ARC
==========================================================

`Authenticated Received Chain (ARC) Protocol <https://tools.ietf.org/html/draft-ietf-dmarc-arc-protocol-09>`_

由于端到端的特性局限，邮件转发过程中，SPF/DKIM可能会校验失败。

ARC-Authentication-Results (AAR) 邮件校验结果，例如spf=pass，dkim=pass，dmarc=pass等等

ARC-Message-Signature (AMS) 注意ARC-Seal头部不加入签名素材，DKIM-Signature header fields必须加入签名素材，AAR有可能加入签名素材

ARC-Seal (AS) 格式与 DKIM-Signature 类似。ARC-Seal只对header fields签名，不含任何邮件内容

i=1 标记转发跳数


spam on new gtld 
==========================================================

`Why Some Phishing Emails Are Mysteriously Disappearing <https://blog.cloudflare.com/combatting-phishing-with-dns/>`_

`dns-operations the real reason for ICANN's gTLD expansion seems to be... <https://lists.dns-oarc.net/pipermail/dns-operations/2017-December/017104.html>`_ 

讨论了new gTLD的副作用

关键是这句

    TLS provides secure transport, not an honest peer.

事实上，SPF/DKIM/DMARC/ARC等等协议并不能解决恶意发送垃圾邮件行为，更多是抵御伪造、篡改邮件的问题。

垃圾邮件发送者通过注册大批量便宜的new gTLD域名，利用发现后再屏蔽的时间差发送海量邮件。由于new gTLD域名往往比传统的com/net/org之类便宜很多，海量注册垃圾邮件域名的成本也随之大幅下降。以至于部分安全软件直接按顶级域屏蔽某些重灾区new gTLD的所有邮件，这对该new gTLD下的良民是显失公平的。

然而每个域名都要给ICANN交手续费，垃圾邮件发送者也是为了域名生意做了大贡献的。所以ICANN的大网安全责任感也要看一下程度，不会说只卖域名不管用途，也不会说new gTLD就不搞了。

普通个人/公司注册大量域名，是抢注域名而不实际使用，还是用于发垃圾邮件？这个事前按付款账户登记识别可能会有些线索（如果不是匿名支付的话，paypal/visa之类与TLD关联合作信誉度），事后核查屏蔽等问题其实是没完没了的，本质上还是因为恶意行为发现的时间差。但是事前审核的思路也容易演变成登记审查制，折腾良民，整体安全成本可能更高。

又或者说一句废话，安全协议不是万能的，没有安全协议则万万不能。

