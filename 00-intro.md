# What is Lightning network

A second-layer payment protocol built on top of the Bitcoin blockchain that enables fast, low-cost, and scalable Bitcoin transactions by using off-chain payment channels. It allows users to conduct multiple transactions with each other without broadcasting every single one to the main blockchain, significantly reducing fees and congestion. Once a payment channel is opened and closed, only the final balance is settled on the main Bitcoin network, using smart contracts for security.

# What is gossip protocol

A gossip protocol is a decentralized communication method used in distributed systems to spread information, like system state or failure status, across a network. Each node periodically shares information with a few randomly chosen peers, and those peers then propagate the information further to other nodes. This approach enables efficient, scalable, and fault-tolerant data dissemination, ensuring that all nodes eventually reach a consistent understanding of the system's state without a central authority.

# Core Lightning network

https://docs.corelightning.org/docs/home

# BOLTS

https://github.com/lightning/bolts/tree/master

# Bitcoin network vs Lightning network

https://www.ka.app/learn/bitcoin-network-vs-lightning

# Why bitcoin network has higher fees during peak usage

Bitcoin network fees increase during peak usage due to limited block space and heightened demand for transaction confirmations. When many users try to send transactions at once, they compete to get their transactions included in the next block by offering higher fees to miners. Miners prioritize these higher-fee transactions, creating a dynamic market where fees rise and fall based on network activity. 
- Limited Block Space
    - Fixed Block Size: Each Bitcoin block has a fixed size (about 1MB) and can only hold a certain number of transactions. 
    - Bottleneck: During periods of high activity, the network gets congested because more transactions are broadcast than can fit into a single block. 
- Increased Demand
    - Competition for Space: When the network is busy, such as during a price rally or major news event, many users try to send transactions simultaneously. 
    - Higher Fees for Priority: Users willing to pay more receive priority from miners, ensuring their transactions are processed and confirmed faster. This creates a bidding war for the available space in each block. 
- Miner Incentives
    - Motivation to Mine: Transaction fees are a key incentive for miners to secure and validate transactions. 
    - Prioritizing High-Fee Transactions: Miners choose which transactions to include in the next block based on the fees offered. Higher fees are more attractive, as they lead to greater profitability for miners.

# Glossary

- Announcement:
A gossip message sent between peers intended to aid the discovery of a channel or a node.

- Channel:
A fast, off-chain method of mutual exchange between two peers. To transact funds, peers exchange signatures to create an updated commitment transaction

- Commitment transaction:
A transaction that spends the funding transaction. Each peer holds the other peer's signature for this transaction, so that each always has a commitment transaction that it can spend. After a new commitment transaction is negotiated, the old one is revoked.

- HTLC: Hashed Time Locked Contract.
A conditional payment between two peers: the recipient can spend the payment by presenting its signature and a payment preimage, otherwise the payer can cancel the contract by spending it after a given time. These are implemented as outputs from the commitment transaction.

- Payment hash:
The HTLC contains the payment hash, which is the hash of the payment preimage.

- Payment preimage:
Proof that payment has been received, held by the final recipient, who is the only person who knows this secret. The final recipient releases the preimage in order to release funds. The payment preimage is hashed as the payment hash in the HTLC.

- Invoice: A request for funds on the Lightning Network, possibly
including payment type, payment amount, expiry, and other information. This is how payments are made on the Lightning Network, rather than using Bitcoin-style addresses.

- Funding transaction:
An irreversible on-chain transaction that pays to both peers on a channel. It can only be spent by mutual consent.

- Penalty transaction:
A transaction that spends all outputs of a revoked commitment transaction, using the commitment revocation private key. A peer uses this if the other peer tries to "cheat" by broadcasting a revoked commitment transaction.

- Mutual close:
A cooperative close of a channel, accomplished by broadcasting an unconditional spend of the funding transaction with an output to each peer (unless one output is too small, and thus is not included).

- Closing transaction:
A transaction generated as part of a mutual close. A closing transaction is similar to a commitment transaction, but with no pending payments.

- Revoked commitment transaction:
An old commitment transaction that has been revoked because a new commitment transaction has been negotiated.

- Revoked transaction close:
An invalid close of a channel, accomplished by broadcasting a revoked commitment transaction. Since the other peer knows the commitment revocation secret key, it can create a penalty transaction.

- Commitment revocation private key:
Every commitment transaction has a unique commitment revocation private-key value that allows the other peer to spend all outputs immediately: revealing this key is how old commitment transactions are revoked. To support revocation, each output of the commitment transaction refers to the commitment revocation public key.

- Unilateral close:
An uncooperative close of a channel, accomplished by broadcasting a commitment transaction. This transaction is larger (i.e. less efficient) than a closing transaction, and the peer whose commitment is broadcast cannot access its own outputs for some previously-negotiated duration.

- Fail the channel:
This is a forced close of the channel. Very early on (before opening), this may not require any action but forgetting the existence of the channel. Usually it requires signing and broadcasting the latest commitment transaction, although during mutual close it can also be performed by signing and broadcasting a mutual close transaction.

- Final node:
The final recipient of a packet that is routing a payment from an origin node through some number of hops. It is also the final receiving peer in a chain.

- Hop:
A node. Generally, an intermediate node lying between an origin node and a final node.

- MSAT:
A millisatoshi, often used as a field name.