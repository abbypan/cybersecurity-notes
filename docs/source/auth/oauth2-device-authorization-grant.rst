OAuth Device Authorization Grant
====================================

RFC8628

当client device输入受限（例如smart TV等），则用户可以在user device (例如手机)协助认证&授权。

用户已在user device上成功登录authorization server。

client 从 authorization server获取 device code, user code, Verification URI。

user device 从 client device 获取 user code & Verification URI，例如扫码访问Verification URI后输入user code。

authorization server校验通过。

client以device code请求access token。
