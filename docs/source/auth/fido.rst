FIDO
##########################################################

overview
==========


`FIDO (Fast Identify Online) 笔记 <https://abbypan.github.io/2015/11/15/fido-pay>`_

`fido overview <https://fidoalliance.org/specifications/overview/>`_

`FIDO (Fast Identity Online) definition <http://searchsecurity.techtarget.com/definition/FIDO-Fast-Identity-Online>`_

`FIDO Alliance <https://en.wikipedia.org/wiki/FIDO_Alliance>`_

`fido spec 详细资料 <https://fidoalliance.org/specifications/download/>`_

fido 基于公钥的用户认证体系，本地认证（UAF简化用户密码操作）+ 远程认证（U2F统一各类网站）结合。

支持两种场景，UAF (Universal Authentication Framework) 与 U2F (Universal Second Factor)。

UAF类似指纹支付、声音控制、划屏解锁、pin码，主要是本地简单认证之后，客户端自动化与服务器登陆（有足够的复杂的密钥）。

U2F类似硬件token卡、usb key之类，二次认证，主要是统一化的远程二级验证接口（不同网站可以用相同的认证设备）。

UAF
==========================================================

用户注册某个远程服务时，本地自动针对该网站生成一个新的公钥对，用公钥信息提交注册。远程用公钥签名challenge，本地用私钥验证后完成注册。（此时不需要传统的密码验证，同时可以实现网站密钥各不相同）

这里对稳私保护到位的是，本地解锁的认证信息不需保存在远程。

在本地简单解锁后，自动化进行远程的公钥认证。

安全：风险进一步转移到本地终端，通过后门控制终端，登陆敏感站点进行恶意操作。针对网站侧的撞库防范重要性降低。

FIDO UAF Architectural Overview
----------------------------------------------------

Fig1 的要点在于 用户设备<->服务器 之间的复杂认证过程对用户透明了

FIDO AppID and Facet Speciﬁcation v1.0 
----------------------------------------------------

FacetID 是触发 FIDO 过程的唯一标识，其实就是认证的实体。web/android/ios场景示例：

.. note::

    https://login.mycorp.com/
    android:apk-key-hash:<sha1_hash-of-apk-signing-cert>
    ios:bundle-id:<ios-bundle-id-of-app>

OAuth/OAuth2 是一个server-to-server的模式。server分发的"application secret"，其脆弱性在于同一个app所有用户复用（例如weico的key案例）。

认证支持覆盖子域。

FIDO Security Reference 
----------------------------------------------------

整体架构的安全问题讨论。

传统的问题点仍旧存在：客户端，服务器，中间人攻击。

主要减少的还是弱密码的风险。服务器对客户端的安全策略比传统的密码+ip+cookie弹性高，毕竟可以绑上设备id。

当然接下来问题又绕回附加的id信息怎么确认是真的，加上设备物理安全。

总之客户端认证的脆弱性影响需要重新考虑，不论是猜测，重放，还是软件破解，硬件破解。（敏感操作需要行为分析）

在手机不设防，又负责校验N多安全信息的场景下。。。绕回来对手机自身加上某些小型安全硬件+可用时限，可能也是一门生意，反正银行游戏商已经做过一轮了。。。

考虑ios的状况，支持fido，设备上有优势。server端做为用户身份认证的集中代理，用户的隐私也难说。然而不开二次认证，icloud不也拦不住弱密码么，毕竟现阶段这同时还是支持用户+密码登陆的web服务。

doc
----------------------------------------------------

FIDO Technical Glossary 缩写字表

FIDO UAF Authenticator-Speciﬁc Module API 接口格式

FIDO UAF Authenticator Commands 指令集

FIDO UAF Authenticator Metadata Statements 数据结构

FIDO UAF Application API and Transport Binding Speciﬁcation 应用绑定

FIDO UAF Authenticator Metadata Service 服务配置



U2F
==========================================================

`FIDO U2F <https://developers.yubico.com/U2F>`_

`FIDO U2F OVERVIEW <https://developers.yubico.com/U2F/Protocol_details/Overview.html>`_

`CIS14: An Overview of FIDO’s Universal 2nd Factor (U2F) Specification <http://www.slideshare.net/CloudIDSummit/cis14-37721981>`_

`u2f client <https://github.com/ashtuchkin/u2f-client>`_

`U2F Technical Overview <https://developers.yubico.com/U2F/Protocol_details/Overview.html>`_

支持USB、蓝牙、NFC等近场通信

其关键在于不同的远程server可以用同一U2F device认证用户身份（不用每一个银行自己发一个usb key）

注册
-------

browser初始化时传server服务信息的hash（域名，端口，协议）给到u2f设备 （request origin）

U2F设备 --(public key , key handle) -->  远程 server；

key handle中包含request origin，标识了对应的u2f设备上的key pair

登陆
-------

server --(key handle) --> U2F设备；

U2F设备根据key handle找到private key，签名后回复server；

server端完成验证


FIDO U2F Implementation Considerations
----------------------------------------------------

请求U2F token须在500ms内响应（硬件速度）。为用户hold状态时延约10s


