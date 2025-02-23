tls attack
##########################################################

`Lucky 13, BEAST, CRIME,... Is TLS dead, or just resting? <https://www.ietf.org/proceedings/89/slides/slides-89-irtfopen-1.pdf>`_

rsa problem
==========================================================

`Protecting RSA-based Protocols Against Adaptive Chosen-Ciphertext Attacks <https://paragonie.com/blog/2018/04/protecting-rsa-based-protocols-against-adaptive-chosen-ciphertext-attacks>`_

Padding Oracle Attack
==========================================================

- `BEAST vs. CRIME Attack <https://resources.infosecinstitute.com/beast-vs-crime-attack/>`_
- `Practical-Padding-Oracle-Attacks-on-RSA <http://secgroup.ext.dsi.unive.it/wp-content/uploads/2012/11/Practical-Padding-Oracle-Attacks-on-RSA.html>`_
- `Padding Oracle Attack <https://shainer.github.io/crypto/matasano/2017/10/14/rsa-padding-oracle-attack.html>`_

BEAST & Lucky 13 & POODLE

Web场景禁用cbc的ciphersuite

Lucky 13
==========================================================

`Lucky Thirteen: Breaking the TLS and DTLS Record Protocols <https://www.ieee-security.org/TC/SP2013/papers/4977a526.pdf>`_

本质在于针对cbc padding的time analysis

所以，或者random time delay、或者用流式、或者aead、或者设法达到constant time processing的效果。

其弱点比较类似dh里的k-bit问题。

Diffie-Hellman实际强度
==========================================================

Logjam (MITM) => TLS1.3

DH 参数强度 => 优先选用ECDH

Bleichenbacher attack/DROWN attack => 禁用RSA PKCS#1 V1.5 PADDING，使用RSA OAEP PADDING

`Hardness of Computing the Most Significant Bits of Secret Keys in Diffie-Hellman and Related Schemes <https://static.aminer.org/pdf/PDF/000/119/803/hardness_of_computing_the_most_significant_bits_of_secret_keys.pdf>`_

计算（破解）:math::`(log p)^(1/2)` MSB 的 dh secret 的复杂度，与计算整个dh secret的复杂度相当。

计算（破解）elgamal public key encryption message类似。

SLtrip Attack(MITM)
==========================================================

HSTS登记网站

Browser配合

coding
==========================================================

Heartbleed => 参数检查

CRIME => tls compression算法禁止选择deflate

nonce misuse
==========================================================

`Nonce-Disrespecting Adversaries: PracticalForgery Attacks on GCM in TLS <https://eprint.iacr.org/2016/475.pdf>`_

避免gcm的错误实现引入漏洞

要么像 chacha20-poly1305，aes-ocb一样，基于seq number & key 生成nonce

要么像 aes-siv 之类，实现类似 mac-then-encrypt的机制，生成tag & nonce

www 
==========================================================

`Does “www.” Mean Better Transport Layer Security? <https://eprint.iacr.org/2019/941.pdf>`_

部署的问题

cert
==========================================================

2016 wosign

`WoSign and StartCom <https://s2.e15r.co/wp-content/uploads/sites/2/2017/01/WoSign-and-StartCom.pdf>`_

`CA:WoSign Issues <https://wiki.mozilla.org/CA:WoSign_Issues>`_

robot
==========================================================

tls-rsa

`ROBOT attack <https://robotattack.org/>`_
