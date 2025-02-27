eap-noob
############

doc
==========================================================

`Nimble out-of-band authentication for EAP (EAP-NOOB) <https://datatracker.ietf.org/doc/draft-aura-eap-noob/>`_

`Secure Bootstrapping for IoT devices <https://iiref.in/icire/ppts/Mohit-EAP-NOOB.pdf>`_

`Connecting IoT appliances securely to the cloud(eap-noob) <https://www.cs.helsinki.fi/group/close/edge-computing-2016/lib/slides/tuomas_aura.pdf>`_

dynamic OOB messages, refresh cycle 3600s。

secret nonce (Noob): first authentication, mutually authentication。

cryptographic fingerprint(Hoob): verify the integrity of the key exchange, detect impersonation and mitm on the in-band channel。

OOB mesage url example (60bytes): server domain name (60 characters base64),  PeerId (22 characters base64),  secret nonce (Noob) 16-byte, fingerprint (Hoob) 16-byte。

overview
==========================================================

核心场景是： no pre-configured authentication credential 的 iot device 如何 Bootstrapping

oob channel: camera, microphone, display screen, speaker, blinking led light ； 可见比ble ssp 的 numeric comparing 信息量大

oob message 的核心在于，iot device 反向授权 某个server（例如近场phone） 控制自身。

eap protocol
==========================================================

协议交互比较简单，就是通信双方相互交换 public key  & nonce 。
- Noob 16 bytes，属于secret nonce，用于计算mac，双向校验ecdhe交互信息的完整性。
- Hoob 16 byte fingerprint, hash值，用于校验exchange内容的完整性，能够覆盖oob channel 的Noob被compromised的场景。

Kz 为 persistent key material，快速重连，校验MACs2

KzPrev 之前的 Kz value，在Kz校验失败时，备选校验MACs2

session key 的派生采用nist的kdf，除了ecdh的z，还加上双方的nonce，以及Noob。

注意reconnect场景下，派生session key的key derivation input参数不同。

data field
==========================================================

server info : type, server name, server url, ssid list

peer info: type , peer name, manufacturer, model, serial number, mac address, ssid, bssid

special-use domain name: eap-noob.arpa

security
==========================================================

authentication principle: Noob & Hoob；Noob可以碰一碰直连。。。

identifying correct endpoint: 设备证明...，避免clone the device identity

trusted path issues and misbinding attack:  physical & device certification

peer identifiers and attributes: server assign random peerID

identity protection: privacy, ...

downgrading threat: 支持reconnect的时候协商升级ciphersuite

protected success and failure indications: successful verification with mac；failure are not protected

recovery from loss of last message: Kz ...

eap security claims
----------------------------------------------------

authentication mechanism: ecdhe key exchange with oob authentication (Noob)

protected cryptosuite negotiation

mutual authentication

integrity protection

replay protection

confidentiality : no

key derivation 

key strength

dictionary attack protection

fast reconnect

cryptographic binding : not applicable

session independence

fragmentation: no

channel binding

