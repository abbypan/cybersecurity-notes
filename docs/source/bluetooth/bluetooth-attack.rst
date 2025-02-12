bluetooth attack
==========================================================

blesa
----------------------------------------------------

`BLESA: Spoofing Attacks against Reconnections in Bluetooth Low Energy <https://www.usenix.org/system/files/woot20-paper-wu-updated.pdf>`_

缺乏authentication & encryption，伪造交互信息，从secure connection降级

其根源在于向后兼容，允许降级

misbinding attack 
----------------------------------------------------

`Misbinding Attacks on Secure Device Pairing and Bootstrapping <https://arxiv.org/pdf/1902.07550.pdf>`_


ble pairing & eap-noob 的核心问题在于，pairing时并未对device identifier做认证。因此存在identity misbinding的风险。

缓解：sts, sigma, ike

device provsioning protocol (dpp)

invalid curve
----------------------------------------------------

`CVE-2018-5383: Breaking the Bluetooth Pairing – The Fixed Coordinate Invalid Curve Attack <https://eprint.iacr.org/2019/1043.pdf>`_

knob
----------------------------------------------------

`CVE-2019-9506: The KNOB is Broken: Exploiting Low Entropy in the Encryption Key Negotiation Of Bluetooth BR/EDR <https://knobattack.com/>`_

bias
----------------------------------------------------

`Bluetooth Impersonation Attacks (BIAS) <http://securityaffairs.co/wordpress/103480/hacking/bias-attack-bluetooth.html>`_



vulnerable pairing
---------------------

`Breaking BLE — Vulnerabilities in pairing protocols leave Bluetooth devices open for attack <https://www.microcontrollertips.com/breaking-ble-vulnerabilities-in-bluetooth-pairing-provide-openings-for-attack-faq/>`_

