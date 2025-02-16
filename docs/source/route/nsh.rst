Network Service Header (NSH)
################################


`RFC8300: Network Service Header (NSH) <https://tools.ietf.org/html/rfc8300>`_

`ODL: Service Function Chaining <https://events.static.linuxfound.org/sites/events/files/slides/odl%20summit%20sfc%20v5.pdf>`_

`slide: Network Service Header (NSH)  <https://www.ietf.org/proceedings/90/slides/slides-90-sfc-4.pdf>`_

`Network Service Header (NSH) Parameters <https://www.iana.org/assignments/nsh/nsh.xhtml>`_

`Service Function Chaining (SFC): Creating a Service Plane Using Network Service Header(NSH) <https://www.opennetworking.org/images/stories/downloads/sdn-resources/IEEE-papers/service-function-chaining.pdf>`_

NSH试图统一数据包在专用网内的策略流转接口

控制面下发策略到 Service Function Forwarder

数据包在Service Classifier入口处，就通过添加NSH头部，逐跳按预设好的策略在Service Function Forwarder链式流转

所以路径是相对固定可控的，host to end，例如：

    NAT -> FW -> DPI -> ... -> LB

可以与vxlan结合使用

路由器升级支持
