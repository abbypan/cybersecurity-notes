Evaluation of Out-of-Band Channels for IoT Security
==========================================================

`Evaluation of Out-of-Band Channels for IoT Security <https://link.springer.com/article/10.1007/s42979-019-0018-8>`_

secure bootstrapping in ad-hot IoT deployment。

Out-of-Band : NFC, QR Code, audio。

Extensible Authentication Protocol (EAP)。

One-time password (OTP): SMS。

group messaging
----------------------------------------------------

telegram, whatsapp, signal, support e2e encryption with oob verification, require users to compare information shown on each other's devices。

telegram: 生成一个图片展示已交换的keys。

whatsapp: 
- 60-bit string = hash (user's public identity key) 到 30-bit + 30-bit (两个string)；用户比较60-bit string
- 或者扫qr code
