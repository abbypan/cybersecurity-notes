OID & ORS
############################

Object Identifiers (OID)
==========================================================

RFC 3061

    0 - ITU-T assigned
    1 - ISO assigned
    2 - Joint ISO/ITU-T assignment

层次化标识，支持各国家分区管理

    Human-readable notation: {joint-iso-itu-t(2) alerting(49) wmo(0) authority(0)}
    Dot notation: 2.49.0.0

OID Resolution System (ORS)
==========================================================

`Information technology – Open systems interconnection – Object identifier resolution system <https://www.itu.int/rec/T-REC-X.672-201008-I/en>`_

各层次的ORS其实就是zone files里配置了ORS域的DNS各级权威，向ORS client最终返回NAPTR记录以标识获取信息的URL

各级ORS配置DNAME，以同时支持数字/字符串格式的域名，例如 joint-iso-itu-t.oid-res.org. IN DNAME 2.oid-res.org




