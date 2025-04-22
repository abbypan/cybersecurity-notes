wpa3
#########

`wpa3-spec <https://www.wi-fi.org/download.php?file=/sites/default/files/private/WPA3_Specification_v3.0.pdf>`_

`Understanding Server Authentication in WPA3 Enterprise <https://www.mdpi.com/2076-3417/10/21/7879/pdf>`_

`wifi security <https://www.wi-fi.org/zh-hans/discover-wi-fi/security>`_

wpa3-personal
======================================

wpa3-sae

核心是引入dragonfly认证，基于psk派生一次一密的pmk。pmk派生ptk的过程与wpa2的4次握手相同。

wpa3-enterprise
======================================

与wpa2-enterprise基本相同，本质是eap认证。

套件可以支持CNSA 192bit。

wpa3 fast bss tranition (FT)
======================================

`Fast BSS Transition (802.11r) <https://blogs.cisco.com/networking/what-is-802-11r-why-is-this-important>`_

两个（或多个）AP有相同的ESSID，同一个sta，从current AP切到target AP，避免重认证，快速连接。

一种方案是sta直接连target AP快速认证，另一种方案是sta通过current AP找target AP快速认证。

server certificate validation
======================================

选用EAP-TTLS, EAP-TLS, EAP-PEAPv0 or EAP-PEAPv1 等模式时，sta必须检查server证书的合法性（以下任选一种）：
- EAP credential里的server cert  = sta 接收到的server cert。
- EAP credential里指定root cert，是sta 接收到的server cert的root；如果EAP credential里指定了域名（或者zone），则必须与sta 接收到的server cert的san/cn域匹配。
- sta 接收到的server cert的root 在sta的可信根列表中，并且，EAP credential里指定的域名（或者zone）与sta 接收到的server cert的san/cn域匹配。

User Override of Server Certificate (UOSC)
----------------------------------------------------

User Override of Server Certificate (UOSC): 当server cert校验失败，弹窗问用户是否忽略失败，继续连接。

server cert里可能有特殊的oid，指定Trust Override Disable (TOD)策略:
- TOD-STRICT: UOSC不给用
- TOD-TOFU: UOSC仅能在首次连接时生效

sae-pk
======================================

sae-pk解决的问题场景是：如果黑客已知password，应如何抵御evil twin AP attack。避免STA连接到伪造的AP。

开启了sae-pk的场景下，password相当于ap的公钥指纹。

SAE Confirm message里包含的SAE-PK内容：
- AP的公钥K_AP, p-256
- 以KEK加密的Modifier(缩写为M)值, 128 bits
- signature，以AP的私钥签名, ecdsa: :math:`KeyAuth = Sig_{AP}(eleAP || eleSTA || scaAP || scaSTA || M || K_{AP} || AP-BSSID || STA-MAC)`

signture校验通过，则表示上述PMKSA内容未经篡改。

Modifier是随机生成，递增求解fingerprint，如果fingerprint的前 8*Sec bits 为0，则该Moidifer值即为目标值。

8*Sec bits 为0，主要用于压缩表达：
- 8*Sec + 19*λ/4 - 5  的fingerprint  可以用 5λ 压缩表达
- 19*λ/4 - 5 为 fingerprint 的一部分
- λ/4 bits 用于编码Sec的值(3或5)
- 5 bits 用于checksum

sae-pk credential
----------------------------------------------------

sae-pk credential: { ap key pair, Modifier (缩写为M), sae-pk password, sae-pk password identifier (optional) }

注意，相同ssid的AP配置的sae-pk credential相同。

相同的ap public key，可以结合不同的Modifier，派生不同的password。

.. math::

    Fingerprint = L(Hash(SSID || M || K_AP), 0, 8*Sec + 19*λ/4 - 5) 

    L(S, F, N): 取S的最左边 [ F, F+N-1 ] bits

    Sec：取3或5

    λ = 4*n, n>=3

    8*Sec + 19*λ/4 - 5 <= HashLen

Fingerprint结合Sec值，分段拼接，再转换为Base32的PasswordBase

.. math::

    PasswordBase = Base32(P(0) || P(1) || ... || P(λ/4-1))

    When i< (λ/4-1), P(i) = Sec_1b || L(Fingerprint, 8*Sec+(19*i), 19)

    When i=(λ/4-1), P(i) = Sec_1b || L(Fingerprint, 8*Sec+(19*i), 14)

    Sec_1b is a 1-bit integer equal to 1 when Sec=3, and equal to 0 when Sec=5

基于PasswordBase计算Verhoeff algorithm的checksum(5 bits)，再按4 character 添加 - 分隔，获得password

.. math::

    Password = AddSeparators(PasswordBase || ChkSum)

Authentication using sae-pk
----------------------------------------------------

注意这里的kck, kek 与 4次握手的 eapol-key kck, kek 不同。

.. math::

    kck_pmk_kek = KDF-Hash-Length(keyseed, "SAE-PK keys", context)

STA校验过程：
- 使用kek解密modifier
- 如果sta本地之前已存储了该ap的公钥`K_AP`，则进行公钥值比对。
- 如果sta本地没有该ap的公钥记录，则从password反向恢复fingerprint，与Modifier正向计算的fingerprint比对。
- 使用K_AP校验`sig_ap`签名

注意sta本地存储的ap公钥，是与password关联的。

attack
----------------------------------------------------

second preimage attack， 黑客如何找到伪造的(K_AP, Modifier)，派生出相同的password。

降级攻击，sta如何被骗ap不支持sae-pk。

wifi uri
======================================

就是把wifi的参数拼了一下，可以转二维码

Transition Disable
======================================

一刀切

Privacy
======================================

随机mac地址
----------------------------------------------------

随机mac地址。

sta连不同ess，用不同mac地址。

sta使用随机mac地址扫瞄。


切换mac地址时，seq number要重置。

scrambler seed ： 生成随机mac地址序列的随机数生成器seed要。。。

GAS：STA进行GAS查询的token记得换

ANQP: sta向ap发anqp查询时，应使用随机地址

pmf
======================================

PMF: Protected Management Frame

MFPC: Management frame protection capable

MFPR: Management frame protection required
