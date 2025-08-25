OAuth Step Up Authentication Challenge Protocol
==========================================================

rfc9470


client -> resource server : 请求resource。

resource server -> client : acr_values + max_age 作为challenge，返回client。防重放。

client -> authorization server: 请求access token，请求内容中包含challenge。

authorization server -> client：返回access token，详情中包含challenge。

client -> resource server : 以新access token请求resource。
