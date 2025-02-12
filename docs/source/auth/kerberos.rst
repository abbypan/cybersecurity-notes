Kerberos
==========================================================

Kerberos的关键在于KDC的安全性。

client 事先与 KDC 有共享密钥，server 事先与 KDC 也有共享密钥。

client 向 KDC 请求票据，再根据该票据向server请求资源。

票据是时间敏感的。
