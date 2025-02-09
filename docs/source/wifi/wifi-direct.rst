wifi direct
###############


abstract
======================================

wifi p2p 其实是一个节点成为group owner(GO)，角色类似AP。

所以首个参与协商的节点，是支持wifi p2p的client；确定了GO之后，也可以是传统wifi的legacy client加入。

Group Formation
==========================================================

P2P Standard Group Formation： 互相发现对方，Go Negotiation（决定谁当GO），WPS Provisioning，DHCP

P2P Autonomous Group Formation：互相发现对方，事先预定了谁当GO，，WPS Provisioning，DHCP

P2P Persistent Group Formation：互相发现对方，双方事先拥有了该Group的信息，client向GO发invitation请求，WPS Provisioning只需Phase 2无需Phase1，DHCP

attack
==========================================================

`EvilDirect: A New Wi-Fi Direct Hijacking Attackand Countermeasures <https://people.engr.tamu.edu/guofei/paper/EvilDirect_ICCCN17.pdf>`_


参考资料
======================================

- `Wi-Fi Direct - Overview and Features <https://hsc.com/DesktopModules/DigArticle/Print.aspx?PortalId=0&ModuleId=1215&Article=221>`_
- `Device to device communications with WiFiDirect: overview and experimentation <http://www.it.uc3m.es/~pablo/papers/pdf/2012_camps_commag_wifidirect.pdf>`_
- `WiFi Direct Configuration Scripts <https://processors.wiki.ti.com/index.php/WiFi_Direct_Configuration_Scripts>`_
- `Wi-Fi Direct to Hell <https://www.blackhat.com/docs/eu-17/materials/eu-17-Blanco-WI-FI-Direct-To-Hell-Attacking-WI-FI-Direct-Protocol-Implementations-wp.pdf>`_
- `Wi-Fi Direct <https://www.slideshare.net/whitehat1409/wifi-direct-27839482>`_

