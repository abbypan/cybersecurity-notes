Web Authentication
########################

主要内容与FIDO一致，基于非对称密钥认证用户身份


注册关键数据流
==========================================================

- client触发注册
- relying party server将challenge和相关注册配置信息给到client
- client组装相关信息给到authenticator
- authenticator校验成功后，随机生成key pair，并基于上述信息生成attestation（其中包含signature，思路类似PKCS#10 CSR）
- authenticator将公钥、attestation等信息给到client
- client将authenticator提供的信息组装后给到relying party server
- relying party server校验成功后，存储对应关键信息（例如credential id, 公钥等等）

.. |register| image:: https://w3c.github.io/webauthn/images/webauthn-registration-flow-01.svg

认证关键数据流
==========================================================

- client触发认证
- relying party server将challenge和相关认证配置信息给到client
- client将对应relying party id和data hash给到authenticator
- authenticator校验成功后，针对data hash进行签名
- authenticator将签名数据传给client
- client将authenticator提供的信息组装后给到relying party server
- relying party server校验成功后，client成功登录

.. |register| image:: https://w3c.github.io/webauthn/images/webauthn-authentication-flow-01.svg

doc
==========================================================

- RFC8264, RFC8265, RFC8266
- `Client to Authenticator Protocol (CTAP) <https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html>`_
- `RFC7049: Concise Binary Object Representation (CBOR) <https://tools.ietf.org/html/rfc7049>`_
- `RFC8152: CBOR Object Signing and Encryption (COSE) <https://tools.ietf.org/html/rfc8152>`_
- `RFC8230: Using RSA Algorithms with CBOR Object Signing and Encryption (COSE) Messages <https://tools.ietf.org/html/rfc8230>`_
- `Web Authentication <https://w3c.github.io/webauthn/>`_
- `What is FIDO2 and Web Authentication? <https://developers.yubico.com/WebAuthn/>`_


ARKG for webauthn
==========================

硬件有效复用

`Asynchronous Remote Key Generation: An Analysis of Yubico’s Proposal for W3C WebAuthn <https://eprint.iacr.org/2020/1004.pdf>`_

`webauthn-recovery-extension <https://github.com/Yubico/webauthn-recovery-extension>`_

    DerivePK:
    (𝑒, 𝐸) ←$ KGen
    𝑘cred ← KDF1 (ECDH(𝑒, 𝑆))
    𝑘mac ← KDF2 (ECDH(𝑒, 𝑆))
    𝑃 ← (𝑘cred · 𝐺) + 𝑆
    cred ← (𝐸, MAC(𝑘mac, 𝐸 ∥serverId))

    DeriveSK:
    𝑘cred ← KDF1 (ECDH(𝑠, 𝐸))
    𝑘mac ← KDF2 (ECDH(𝑠, 𝐸))
    if cred ≠ 𝐸 ∥ MAC(𝑘mac, 𝐸 ∥serverId) then abort
    𝑝 ← 𝑘cred + 𝑠 mod 𝑛

PQC for webauthn
===================

`Quantum-Safe Account Recovery for WebAuthn <https://eprint.iacr.org/2024/678.pdf>`_

`Post-Quantum Account Recovery for Passwordless Authentication <https://uwspace.uwaterloo.ca/items/d1f73f71-e3b2-438c-b261-11632becdbb2>`_

整体思路与ARKG一致。

把ecdh的方案扩展到pqc kem。

本质是通过kem的encap，结合blindPK 派生。
