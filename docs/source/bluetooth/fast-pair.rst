Fast Pair
===============

Nearby

`Google Fast Pair Service (GFPS), or Fast Pair (FP) <https://developers.google.com/nearby/fast-pair/landing-page>`_

Accessory 一型一密：Model ID, Anti-Spoofing Public/Private Key Pair

Seeker (phone) <-> Provider (accessory)
---------------------------------------------

Provider:
- 广播 Model ID
- 支持存储 128-bit Account Keys，至少5个key slots。每个Account Key标识一个用户账号。Account Key太多，影响adv长度。
- 支持RPA。 

advertisement用bloom filter:
- 对每个Account Key (K)，结合 salt，计算 H = sha256( K || salt )
- 再将H压缩到8个4 bytes的集合 X = { X_0, .., X_7 }
- 将X映射到bloom filter的8个bit上
- salt随RPA更新


Key-based Pairing
---------------------

基于Anit-Spoofing Key Pair：
- Seeker隐式确认 Provider拥有其广播的Model ID所对应的Anit-Spoofing Private Key
  
基于Account Key:
- Seeker & Provider 确认双方拥有相同的Account Key


pairing
-----------

Seeker -> Provider: Encrypted Request (含seeker addr, provider addr, salt), Public Key (optional)

Provider:
- 如果含Public Key，则provider计算 K = SHA256(ECDH(Anit-Spoofing Private Key, Public Key))[0 .. 15]，解密Encrypted Request
- 如果不含Public Key，则provider尝试使用Accout Key List中的Account Key作为K，尝试解密Encrypted Request

Provider -> Seeker: Encrypted Response (含provider addr, salt)

Seeker:
- 选择Numeric Comparison模式
- ble stack生成6-digit passkey

Seeker -> Provider: Encrypted passkey block (含passkey, salt)

Provider:
- 检查passkey是否是expected （依赖accessory形态）

Provider -> Seeker: Encrypted passkey block (含passkey, salt)

Seeker & Provider : 成功pairing 

Seeker -> Provider : Account key (encrypted)


Characteristic: Additional Data
-------------------------------------

AES-CTR & HMAC-SHA256


ukey2
=========

应用层kex, AES_256_CBC-HMAC_SHA256

`ukey2 <https://github.com/google/ukey2>`_


M_1 : client -> server : [ { ciphersuite, client pubkey commit } ]

M_2 : server -> client : selected ciphersuite, server pubkey

client -> server : client pubkey of the selected ciphersuite ( matched with the client pubkey commit )

DHS = DH(client privkey, server pubkey) = DH(server privkey, client pubkey)

PRK_AUTH = HKDF-Extract("UKEY2 v1 auth", DHS)

PRK_NEXT = HKDF-Extract("UKEY2 v1 next", DHS)

AUTH_STRING = HKDF-Expand(PRK_AUTH, M_1|M_2, L_auth)

NEXT_SECRET = HKDF-Expand(PRK_NEXT, M_1|M_2, L_next)
