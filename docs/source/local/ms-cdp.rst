MS-CDP: Connected Devices Platform Protocol
###############################################

`MS-CDP: Connected Devices Platform Protocol Version 3 <https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-cdp/f5a15c56-ac3a-48f9-8c51-07b2eadbe9b4>`_

协议相对简单

connect
======================================

兼容：UDP/TCP/Cloud/BLE/RFcomm/Wifi Direct

UDP: device name, device id salt 4-byte, device id hash 4-byte

BLE: salt 4-byte, device hash 24-byte, BLE发现可能是broadcast/multicast

auth
======================================

Device Auth: self-signed, system boot时生成 => interesting

User Device Auth，system sign-in时生成 => 放哪呢？！

encryption
======================================

clientNonce (64-bit), serverNonce(64-bit)

ecdh 后 hkdf 生成 key (16-byte), iv (16-byte), hmac-key (32-byte)
