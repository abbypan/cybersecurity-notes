MOST (Media Oriented Systems Transport)
==========================================================

`Media Oriented Systems Transport (MOST) <https://vector.com/vi_most_en.html>`_

`MOST: THE AUTOMOTIVE MULTIMEDIA NETWORK <http://www.ciando.com/img/books/extract/3645250611_lp.pdf>`_

`MOST150 – The Next Generation Automotive Infotainment Backbone <https://www.mouser.com/pdfdocs/Microchip-Next-Generation-Automotive-Infotainment.pdf>`_

`Bridging MOST to IEEE Standards <http://www.ieee802.org/1/files/public/docs2012/new-Bridging-MOST-Muyshondt-Bridging-MOST-to-IEEE-Standards-0712.pdf>`_

`Vehicle Networks Multimedia Protocols <https://www.sti-innsbruck.at/sites/default/files/courses/fileadmin/documents/vn-ws0809/06-VN-MultimediaNetworks.pdf>`_

`New elements in vehicle communication “media oriented systems transport” protocol <http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.1006.4793&rep=rep1&type=pdf>`_

`MOST Technology Report - MOST Cooperation <https://www.mostcooperation.com/publications/.../most-technology-report/>`_

一般是环形拓扑，最多64个节点，节点即插即用。也可采用星形，或双环。

有一个Time Master节点负责向ring发布frame，其他Time Slave节点保持同步。25Mbps~150Mbps，光纤，主要用于车载娱乐系统(音频，视频，数据）。60 Channels.

Every slave synchronizes with the frame preamble by a Phase-Locked Loop (PLL)

MOST提供控制指令的信道，也可以用专用信道tunnel Ethernet Communication。

帧格式
----------------------------------------------------

MOST25, 64Bytes: Preamble, Boundary Descriptor, Synchronous channel, Asynchronous Packet Data channel, Control channel 2Bytes, Frame Control, Parity Bit

MOST50, 128Bytes: Control 4Bytes, Synchronous, Asynchronous Packet Data Channel

MOST150, 384Bytes: Control 4Bytes, Synchronous/isochronous streaming channel, Asynchronous Packet Channel/Most Ethernet Packet (MEP) Channel

Ethernet over MOST150
----------------------------------------------------

`Interfacing to the MOST Ethernet Channel <http://www.ieee802.org/1/files/public/docs2013/AC-Muyshondt-InterfacingMOSTEthernetChannel-0713-v01.pdf>`_

安全
----------------------------------------------------

娱乐系统一般很少有入侵动机，除非做为攻击跳板。

MOST frame比较大，传输关键channel数据的时候可加校验。
