WPA2 KRACK
################


Key
======

Authenticator (AP) and Supplicant (client) and the Authenticator’s and Supplicant’s MAC Addresses (AA/SA).

pre-shared-key (PSK) 类似passphrase，俗称wifi密码

Pairwise Master Key (PMK) = PBKDF2(HMAC−SHA1, PSK, SSID, 4096, 256)  主密钥

Pairwise Transient Key (PTK)  = PRF(PMK + ANonce + SNonce + AA + SA) 临时密钥

PTK共512bits，拆分成5个子密钥
- Key Confirmation Key (KCK) 128bits - 用于生成MIC, Message Integrity Code.
- Key Encryption Key (KEK) 128bits - 用于ap加密数据内容
- Temporal Key (TK) 128bits - 用于单播数据包加解密
- MIC Authenticator Tx Key (MIC Tx) 64bits - 只用于ap发送TKIP配置
- MIC Authenticator Rx Key (MIC Rx) 64bits - 只用于client发送TKIP配置

Handshake
============

client 与 ap 有着相同的PMK，4-way Handshake:
- ap -> client: r, ANOUCE
- client -> ap: r, SNOUCE, MIC ; 注意这里client已独立生成了PTK
- ap -> client: r+1, GTK, MIC ; 注意这里ap独立生成了PTK
- client -> ap: r+1

4-way Handshake 完成之后，client跟ap就开始用协商好的PTK通信 

KRACK
========


KRACK 是利用了软件实现缺陷，使得WPA2的数据链路层加密效果大打折扣～

通过MITM的阻断、重放影响4-way Handshake，触发 PTK/GTK 的reinstallation，使得client设置可预知的r及nouce，更严重的是初始化一个全0的TK密钥。

Rather worryingly, our key reinstallation attack even occurs spontaneously if certain handshake messages are lost due to back-ground noise. 

This means that under certain conditions, implementations are reusing nonces without an adversary being present.

不用主动攻击，有时候客户端也会自己触发reinstallation，666。。。

该攻击的关键前置条件是：MITM。

所以楼上楼下用WIFI万能钥匙，或者公共WIFI。。。

MITM
=======

所以，在不知道WIFI密码的情况下，要怎么弄到MITM的位子就很关键。

As mentioned in the demonstration, the attacker first obtains a man-in-the-middle (MitM) position between the victim and the real Wi-Fi network (called a 
`channel-based MitM position <https://lirias.kuleuven.be/bitstream/123456789/473761/1/acsac2014.pdf>`_
).

这个channel-based MITM position大概思路：
- 伪造一个相同SSID的AP，在其他channel上开放服务。
- 在真实AP运行的channel上进行信号干扰，促使client主动切换到伪造AP的channel上。
- 伪造AP负责中间转发client与真实AP的数据包。

client自动切换channel是常有的事，所以客户端基本不设防。

4-way Handshake协议里面，也并未包含channel信息，难以直接检测。

参考资料
=============

- `Key Reinstallation Attacks: Forcing Nonce Reuse in WPA2 <https://papers.mathyvanhoef.com/ccs2017.pdf>`_
- `Key Reinstallation Attacks <https://www.krackattacks.com/>`_
- `KRACK - Key Reinstallation Attacks: Forcing Nonce Reuse in WPA2 <https://www.youtube.com/watch?v=fOgJswt7nAc>`_
- `CWSP – 4 Way Handshake <https://mrncciew.com/2014/08/19/cwsp-4-way-handshake/>`_
- `Authentication & Key Management <https://www.youtube.com/watch?v=8OPdE1MM1yE>`_
- `The 4-Way Handshake <https://www.youtube.com/watch?v=9M8kVYFhMDw>`_
- `How does WPA and WPA2 work? <https://www.youtube.com/watch?v=-Q_WXeEf8Fw>`_
- `IEEE 802.11i-2004 <https://en.wikipedia.org/wiki/IEEE_802.11i-2004>`_
- `Key Management for Link Layer Security <http://www.ieee802.org/1/files/public/docs2004/AFjul04KimKey_Management_For_Link_Layer_Security.pdf>`_
- `802-11i_key_management <https://www.cwnp.com/uploads/802-11i_key_management.pdf>`_
- `WPA PSK (Raw Key) Generator <https://www.wireshark.org/tools/wpa-psk.html>`_
- `Understanding WPA/WPA2 Pre-Shared-Key Cracking <https://www.ins1gn1a.com/understanding-wpa-psk-cracking/>`_
- `4-Way Handshake <https://wlan1nde.wordpress.com/2014/10/27/4-way-handshake/>`_
