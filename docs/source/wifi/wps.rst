WPS (Wi-Fi Protected Setup)
#################################

WPS (Wi-Fi Protected Setup) Provisioning -> WSC (Wi-Fi Simple Configuration)
=====================================================================================

`wifi simple Configuration technical specification <https://www.wi-fi.org/certification/programs>`_

`深入理解wi-Fi Simple Configuration <https://blog.csdn.net/innost/article/details/21555225>`_

`Wi-Fi Protected Setup - WPS <https://routersecurity.org/wps.php>`_

`Brute forcingWi-FiProtected Setup <https://sviehb.files.wordpress.com/2011/12/viehboeck_wps.pdf>`_

WPS为client & GO建立安全连接。支持PIN码模式、Push Button模式。

7 digit PIN码不带checksum，8 digit PIN码的最后一位是checksum。

`wps.c <https://github.com/tuomaura/eap-noob/blob/master/hostapd-2.9/src/wps/wps.c>`_ ， Push Button是一种特殊的模式，主要方便用户直接按设备小按钮配对，PIN码取值为00000000。

`wps_common.c <https://github.com/tuomaura/eap-noob/blob/master/hostapd-2.9/src/wps/wps_common.c>`_ ，Registration Protocol Messages，基于DH key与双方nonce派生KDK；再使用KDK派生出authkey, kwk, emsk；基于authkey，结合DevicePassword(即PIN码)，派生psk1, psk2。psk1/psk2协助生成e-hash1/e-hash2，用于双方校验。

DH modulus 见 RFC3526。

emsk结合双方nonce，再派生出MgmtAuthKey，MgmtEncKey，用于AP消息的认证和加密。

rekeying: emsk结合双方nonce，还可以派生出新的psk（即DevicePassword）。

`wps_enrollee.c <https://github.com/tuomaura/eap-noob/blob/master/hostapd-2.9/src/wps/wps_enrollee.c>`_ ，random generate per-device psk

`wps_registrar.c <https://github.com/tuomaura/eap-noob/blob/master/hostapd-2.9/src/wps/wps_registrar.c>`_ ，处理pin/pbc等。

分成两个Phase。

Phase 1主要是基于WPA2协商，AES-CCMP cipher，随机生成 Pre-Shared Key (PSK) 用于双向认证。

Phase 2主要是client断开原有连接，以Phase1协商好的auth credential重连。

rfc5247: Extensible Authentication Protocol (EAP) Key Management Framework

analysis
==========================================================

显然，pbc无法防mitm。

pin码仅用于认证，不参与密钥派生。
