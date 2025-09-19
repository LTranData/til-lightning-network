# How bitcoin work

## Core

Bitcoin is a decentralized digital currency operating on a peer-to-peer (P2P) network without a central authority. Its core is a public, distributed ledger called the blockchain.

## Network

The Bitcoin network is a P2P mesh topology. There's no central server; instead, every computer running the Bitcoin software (a node) connects to several other nodes. When you initiate a transaction, your node broadcasts it to its connected peers, who in turn relay it to their own peers. This ripple effect ensures the transaction propagates rapidly across the entire network.

This decentralized architecture provides censorship resistance and resilience. If some nodes go offline, the network remains operational as data can still be routed through other active connections. Full nodes maintain a complete and updated copy of the entire blockchain, validating transactions and blocks against the protocol's rules.

## Data structure

The blockchain is a linked list of blocks, but with a cryptographic twist that ensures its immutability. Each block contains a header and a list of transactions.

- Block Header: A fixed-size data structure (80 bytes) containing metadata:

Version: Indicates the software version used.

Previous Block Hash: A cryptographic hash of the previous block's header. This is the "link" that chains the blocks together. Altering a past block's data would change its hash, invalidating the link and all subsequent blocks.

Merkle Root: A single hash that summarizes all transactions in the block.

Timestamp: The time the block was created.

Difficulty Target (nBits): An encoded representation of the target hash miners must find.

Nonce: A 32-bit arbitrary number that miners change to solve the Proof-of-Work puzzle.

- Merkle Tree: All transactions within a block are organized into a Merkle tree (or hash tree). This binary tree data structure uses a series of cryptographic hashes to condense all transaction data into a single root hash, the Merkle Root. To build the tree, transaction hashes are paired and hashed together recursively until only one hash remains.  This provides an efficient way to verify if a transaction is included in a block without having to download or process all other transactions in that block.

## Bitcoin Mining

Bitcoin mining is the process of creating new blocks to add to the blockchain. It serves two primary functions: securing the network and issuing new bitcoins. Miners compete to solve a computationally intensive puzzle, known as Proof-of-Work (PoW).

- Transaction Aggregation: A miner collects a set of pending transactions from the mempool (a holding area for unconfirmed transactions) and assembles them into a new block. They then build a Merkle tree from these transactions to compute the Merkle Root.

- The Hashing Puzzle: The miner must find a nonce such that when the block header (including the Merkle Root, previous block hash, and other metadata) is passed through a cryptographic hash function (SHA-256), the resulting hash is less than or equal to the network's difficulty target.

- Broadcasting the Solution: The first miner to find a valid hash broadcasts their block to the network. Other nodes verify the solution by running the same hash function on the proposed block header. Since verification is trivial, it takes very little time.

- Reward and Confirmation: Once a block is verified by the network, it is added to the blockchain. The successful miner receives a block reward of newly minted bitcoins (currently 3.125 BTC) plus the transaction fees from all transactions within that block. The transactions in the new block are now considered "confirmed."


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