MagicPairing
================

`MagicPairing: Apple’s Take on Securing Bluetooth Peripherals <https://arxiv.org/pdf/2005.07255>`_

iphone <-> accessory (例如airpods耳机)

基于icloud account，每个耳机只需初始化时单次pairing，即可与同账号多个iphone设备无感互联。

setup
------------

BLE ssp 之后，key distribution：

iphone -> master key: Master Key/Hint

Master Key  + Accessory Bluetooth Address Blob => aes-ecb，得到 Accessory Key

Master Hint  + Accessory Bluetooth Address Blob => aes-ecb，得到 Accessory Hint

iphone -> accessory: Accessory Key/Hint

pairing
--------------

双方基于 Accessory Bluetooth Address Blob/Accessory Hint识别对端pairing设备

基于Accessory Key，结合nonce，双向random，派生LK

通过HCI设置LK
