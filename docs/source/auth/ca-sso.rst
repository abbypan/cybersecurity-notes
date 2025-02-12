CA SSO (SiteMinder)
==========================================================

`Designing a CA Single Sign-On Architecture for Enhanced Security <https://acclaimconsulting.com/wp-content/uploads/2015/02/designing-a-ca-sso-architecture-for-enhanced-security.pdf>`_

传统统一登录cookie的问题之一是一次登录，全站有效。一站泄漏cookie，全站影响。

改进方案是，一次登录到一个cookie provider，如login.ca.com

由cookie provider对该域下的不同服务站点，分发不同的cookie。例如app1.ca.com的cookie与app2.ca.com的cookie不同。

此时可以把窃取cookie漏洞的影响问题限定在该漏洞子站内。
