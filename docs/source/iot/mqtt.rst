MQTT
########


overview
=============

MQTT is a Client Server publish/subscribe messaging transport protocol.

client a -> broker -> client b

client 向 broker 订阅/发布 消息，broker向已订阅某topic的client发布自身收到的该topic的消息

broker可以根据消息主题、内容、类型进行相关过滤、选取

通配符 + 单层，# 多层, $开头：

.. raw::

    myhome/groundfloor/+/temperature
    myhome/groundfloor/#/temperature
    $SYS/

MQTT over TCP/IP : port 1883, MQTT over TLS : port 8883

支持oauth2认证，payload encryption（对称，非对称），完整性校验

MQTT-SN
============

为嵌入式设备优化的协议，可以在非TCP/IP环境，例如Zigbee下使用。


参考资料
===========

- `MQTT (MQ Telemetry Transport) <http://internetofthingsagenda.techtarget.com/definition/MQTT-MQ-Telemetry-Transport>`_
- `MQTT Essentials <http://www.hivemq.com/blog/mqtt-essentials/>`_
- `MQTT/MQTT-SN Protocol Specifications <http://mqtt.org/documentation>`_
- `MQTT: Get started with IoT protocols <https://opensourceforu.com/2016/11/mqtt-get-started-iot-protocols/>`_
- `MQTT For Sensor Networks (MQTT-SN) <http://mqtt.org/new/wp-content/uploads/2009/06/MQTT-SN_spec_v1.2.pdf>`_
- `MQTT-SN Protocol <https://emqttd-docs.readthedocs.io/en/latest/mqtt-sn.html>`_
