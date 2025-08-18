ANP (Agent Network Protocol)
###################################

https://github.com/agent-network-protocol/AgentNetworkProtocol/

https://agent-network-protocol.com/code/

public key
===============

以 did:wba 作为did标识，通过 域名+identity 结合HTTPS well-known url 发布did.json，内含该identity的public key

思路与android applinks差不多，风险相对高一些

相当于以现有域名+HTTPS的生态环境，替换较重的blockchain-based did。


authentication
=================

基于did:wba的public key校验签名，通过后，进行token授权


e2e
=====

基于did:wba的public key作为long-term public key，实施ecdhe


安全分析
==========

public key 仍易受bgp/dns/ceritifcate hijack影响。

e2e 可优化为sigma-i之类。
