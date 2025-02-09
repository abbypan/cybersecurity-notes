wpa2
########

wpa2-psk
======================================

`Parallel active dictionary attack on WPA2-PSK Wi-Fi networks <https://www.researchgate.net/publication/308862817_Parallel_active_dictionary_attack_on_WPA2-PSK_Wi-Fi_networks/figures?lo=1>`_

`wpa2 <https://www.slideshare.net/ENGMSHARI/wpa2>`_

4次握手：
- psk + ssid 派生 pmk，pmk + anouce/snonce + amacaddr/smacaddr 派生PTK
- PTK拆分3部分：KCK，KEK，TK(CCMP)
- 如果需要GTK，使用KEK wrap处理

显然，握手负责认证兼密钥派生。核心问题在于pmk的生成是固定的，导致dictionary attack
