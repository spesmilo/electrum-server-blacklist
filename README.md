## Centralised blacklist of servers

This repo contains a list of electrum servers that we have seen behave intentionally maliciously towards users.

The list is used by the ElectrumX server implementation
[as part](https://github.com/spesmilo/electrumx/blob/b267f3c5ff72f5645bfb40f1da1b7508bf7a6883/src/electrumx/lib/coins.py#L322)
of its [peer discovery](https://github.com/spesmilo/electrumx/blob/master/docs/peer_discovery.rst).
As a result, ElectrumX servers filter these servers out from their `server.peers.subscribe` response.
Consequently these servers propagate less well in the gossip, and so clients (in particular old clients with unpatched vulnerabilities) are less likely to connect to them.

The Electrum Wallet does not use this list at all. Any client can freely connect to any server.


Relevant malicious server behaviour includes:
- historically, phishing users as response to `blockchain.transaction.broadcast`
    - ref https://github.com/spesmilo/electrum/issues/4968
    - Electrum Wallet <3.3.4 was vulnerable to this
- replacing a just-broadcast tx to try to trick user into RBF-ing it
    - ref https://github.com/spesmilo/electrum/security/advisories/GHSA-6jp4-8v48-cc5v (CVE-2026-117601)
    - Electrum Wallet <4.8.3 was vulnerable to this
