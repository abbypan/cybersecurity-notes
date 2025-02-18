EPC & ONS
#############

Electronic Product Code (EPC)    
==========================================================

RFC 5134

例子：urn:epc:id:sgtin:900100.0003456.1234567

注意，sub namespace支持自定义Validation mechanism，例如gtin可以自定义校验码生成规则

[ONS Version 2.0.1](https://www.gs1.org/epcis/epcis-ons/2-0-1)

EPC转换成FQDN
==========================================================

GS1 Identification Key type 有多种数据类型，例如GTIN/GDTI/GSIN等等

这些数据类型可以转换成统一格式的Application Unique String (AUS)， ONS Client可以把AUS转换成FQDN域名格式用于ONS查询：

    GTIN-13 061414132260 -> AUS : ||gtin|00614141322602  -> 0.0.6.2.2.3.1.4.1.4.1.6.0.gtin.gs1.id.onsepc.com

AUS到FQDN的转换大概规则：去掉  optional serial number / checksum digit ; 保留leading digit；reverse剩余的digit。

    ||gtin|00614141322602，去掉末位的2，提取首位0，剩余061414132260倒转为 062231414160，类型为 gtin，使用onsepc.com权威域，最终生成：

    0.0.6.2.2.3.1.4.1.4.1.6.0.gtin.gs1.id.onsepc.com

    0.  0.6.2.2.3.1.4.1.4.1.6.0.  gtin.gs1.id.  onsepc.com

Object Naming Service (ONS)
==========================================================

ONS Application -> ONS Client : Application扫瞄RFID之类获取信息，以 Global Trade Item Number (GTIN) 为例。

ONS Client -> DNS Recursive : ONS Client 将收到的 GTIN 最终转换成 FQDN 域名格式，发DNS查询其NAPTR记录

ONS Client -> ONS Application : 返回NAPTR记录

ONS Application -> EPC Information Services (EPCIS) server : 根据NAPTR记录给出的URL发起查询

    onsepc.com                  ONS maintained by GS1 Global
    ons.epcglobalcanada.org     Canadian ONS
    onsepc.fr                   French ONS
    ons.epcglobal.cn            Chinese ONS
