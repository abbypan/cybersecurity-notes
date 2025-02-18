BGP Prefix Origin Validation
==========================================================

RFC6811

VRP: Validated ROA Payload

BGP speaker 定期 load validated objects到本地，其内容包括：IP，前缀长度，最大前缀长度，源AS

Prefix : IP + 前缀长度

Route: Prefix + AS_PATH

Covered： 一个Route prefix被一个VRP covered，当且仅当，二者prefix address相同(cidr计算后比对bit位)，且VRP prefix length <= Route prefix length

Matched: 一个Route prefix被一个VRP matched，当且仅当，Route prefix被VRP coverd，且 Route prefix length <= VRP prefix max length，且二者AS号相同

因此，BGP Route的三种状态：

Not Found: 找不到Cover该Route的VRP

Valid: 至少有一个VRP match该Route

Invalid: 至少有一个VRP cover该Route，但没有VRP match该Route

对于invalid状态的处理由本地策略决定

如果invalid就discard，那么保存roa、vrp的数据库就成为风险单点。攻击者伪装成valid source AS，发布伪造的BGP route announcement
