# Outline
- [Motivation](#motivation)
- [Overview](#overview)
- [Roles](#roles)
- [Glossary](#glossary)
- [The Infinitechain Protocol](#the-infinitechain-protocol)
- [Analysis](#analysis)

# Motivation
There are more and more decentralized app on the blockchain. These applications need a stable operation environment. However, there are three main issues on the current public blockchain:

- Insufficient Bandwidth
Every user can send a transaction not only for normal transaction but also for application(smart contract) operating such like decentralized cryptocurrency exchange or cryptokitty game. Unfortunately, the current bandwidth of the public blockchain (Bitcoin or Ethereum) are limited at 7-25 transactions per second (tps). This problem caused `Insufficient Bandwidth` for all the users who want to send a transaction to the blockchain.

- Blockchain Bloat
Even if the bandwidth problem is solved (such like using private chain), the numerous transactions are confirmed on the blockchain will let the storage increase quickly which is called `Blockchain Bloat`. It means that all the full nodes will store huge transactions and block data in their disk.

- Lack of Privacy
Because of the open and transparent. All the transactions on the public blockchain are allowed to be seen by all the users. So, it's hard to keep the privacy.

Because of the above issues, there are few centralized application such like bank services or gorvernment services are built on the blockchain. Ususally, centralized services want to achieve data security and high throughput.

To solve the above issues, we create Infinitechain protocol to increase bandwidth, reduce the full node's disk loading, keep the privacy, and the most importantly, let centralized systems able to use the public blockchain easily.

# Overview
![](https://i.imgur.com/Z7g1N5b.jpg)

**Infinitechain** is a bridging solution of connecting blockchain and centralized system.

With Infinitechain protocol, user can easily interact with centralized services with their ETH and all ERC-20 compatible tokens and performs much more amazing features, such as:

1. Easily make a trustless off-chain payment between you and any centralized services.
2. Realtime confirmation.
3. Private transactions, only the transaction participants can see the relative informations.
4. Private keys are always under your control.
5. Centralized services can't tamper any payments and is supervised by the crowd.
6. Stackholders can instantly withdraw their profits on bockchain after a new stage is published.

# Roles
In Infinitechain protocol, there are four kinds of roles: **Client**, **Server**, **Node** and **Stakeholder**.

- Client
  - Could be a mobile app or a browser extension.
  - Create _rawPayments_.
  - Execute _auditing_.
- Server
  - Could be a live video streaming platform, E-commerce website or other centralized services.
  - Collect _rawPayment_ from the Client.
  - Genetate _payment_ from _rawPayment_.
  - Send _payment_ to Node.
  - Request a new _stage.
- Node
  - Construct _indexed merkle tree_.
  - Package informations of a new _stage_.
- Stackholder
  - One of the Clients.
  - Cooperate and have right to share profits with the Server.
  - Ask for the _indexed merkle tree_ and try to decrypt the leaf data.

# Glossary
### Raw Payment
The original information generated by the client. It has the following structure:

```
rawPayment = {
  // who send the payment
  from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',
  // who receive the payment
  to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
  // the coin value
  value: 100,
  // a payment counter. it prevent replay attacks
  localSequenceNumber: 99,
  // client expect this payment will be confirmed on a specific stage
  stageHeight: 3,
  // allow user write any comment or data but should include pkClient and pkStakeholder
  data: {
    pkClient: 'clientPublicKey',
    pkStakeholder: 'stakeholderPublicKey',
    foo: 'bar'
  }
}
```

### Payment
The information signed by server with private key. It has the following structure:

```
payment = {
  // server will commit this payment on the stage which is assigned by client
  stageHeight: 3,
  // hash of the stageHeight
  stageHash: 'c89efdaa54c0f20c7adf612882df0950f5a951637e0307cdcb4c672f298b8bc6',
  // hash of the encrypt of raw payment
  paymentHash: '6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
  // cipher which is encrypted via client's public key
  cipherClient: 'blahblah',
  // cipher which is encrypted via stakeholder's public key
  cipherStakeholder: 'blahblah'
  // signature of the payment hash
  v: 28,
  r: '0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
  s: '0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
}
```

A payment is fundamental unit in a sidechain. Payments can be made off-chain rapidly and eventually take effect on public blockchain.

The payment hash is constructed by the formula below:

```
payment hash = keccak256(cipherClient + cipherStakeholder)
```

### Stage
Stage is the state that lives in the Infinitechain smart contract, which contains a roothash of a indexed merkle tree. The roothash is a cryptographic proof that stands for the integrity of a collection of serveral _payments_.

The Infinitechain node will publish a new stage onto blockchain while the centralized service decides to share its current profits with all its stackholders.

### Stage hash
A stage height can be hashed into a stage hash which could be used to look up the stage contract address.

### Indexed Merkle Tree (IMT)
![](https://i.imgur.com/DmLzIlI.png)

Indexed merkle tree is a data structure to store hash value of _payments_. It is designed to extract or access elements in it rapidly and can easily prove the existence and integrity of one particular element.

# The Infinitechain Protocol

!['Infinitechain protocol'](https://imgur.com/0lGspKc.png)

## Client produces _rawPayment_ and sends it to Server (1~3)
1. [Client produces _rawPayment_ via infinitechain SDK](https://github.com/TideiSunTaipei/infinitechain_browser#3-make-raw-payment-with-specific-format)
2. Client saves the _rawPayment_ at client side. The stored _rawPayment_ will be used to verify _payment_ returned from server later.
3. Client sends the _rawPayment_ to server.

## Server generates _payment_ from _rawPayment_ and sends it to Node and Client (4~6)
1. [Server encrypts _rawPayment_ and genetates _payment_ and signs it with server's private key.](https://github.com/TideiSunTaipei/infinitechain_nodejs#3-make-a-valid-payment)

2. Server sends _payment_ to both [Node](https://github.com/TideiSunTaipei/infinitechain_nodejs#4-send-payments-to-infinitechain-node) and Client.

3. Node puts the _payment_ to the pending pool. These _payments_ will later compose a _indexed merkle tree_ for next _stage_.

## Client [verifies _payment_](https://github.com/TideiSunTaipei/infinitechain_browser#4-verify-payment) and saves it (7)
After getting the _payment_, the client verifies:
- If the ciphers are decryptable
- If the received paymentHash corresponds to the hash computed from payment ciphers
- If the paymentHash is in client's storage

If the _payment_ is verified, the client saves the _payment_ in order to keep server's signature for taking further actio such as _objection_

## Server [commits a _stage_ of _payment](https://github.com/TideiSunTaipei/infinitechain_nodejs#5-commit-payments-to-blockchain)_ (8~11)
1. Server sends a request to Node for fetching the root hash.
2. Node builds the _indexed merkle tree_ for _payments_ from the pending pool and returns the root hash.
3. Server receives the root hash and adds a new stage on blockchain.

## Node updates pending pool (12~13)
1. Node watches for the _AddNewStage_ event.
2. Node removes _payments_ which are already on _stage_ from pending pool.

## Client [_audits_ _payment_](https://github.com/TideiSunTaipei/infinitechain_browser#5-audit-payment) distributedly (14~15)
_Auditing_ is the process that Client will compute the root hash from the stored _rawPayment_ locally and compare this result with the root hash from blockchain.

_Auditing_ is the crutial mechanism for Client to check whether Server handles the _payment_ honestly and correctly.

If the _auditing_ succeeds, that means Server handls the _payment_ correctly and honestly. If the _auditing_ fails, then the client could take further action, which is _objection_.

## Client _[takes objection](https://github.com/TideiSunTaipei/infinitechain_browser#6-take-objection)_ when the audition fails (16)
While Client discovers that server not handle _payments_ correctly, Client could _take objection_ to inform the Server.

## Server _[exonerates](https://github.com/TideiSunTaipei/infinitechain_nodejs#6-exonerate-payment)_ the ingrity of _payment_ when the _objection_ is taken (17~18)
It's possible that client request a false _take objection_ even the Server handles _payment_ correctly.

In that case, the Server could _exonerate_ to reject the false _objection_.

## Server _[pays penalty](https://github.com/TideiSunTaipei/infinitechain_nodejs#7-pay-penalty-payments)_ for each _objection_ when the exoneration fails (19)
The Server could _pay penalty_ to the Client if there is a valid _objection_.

## Server _[finalizes](https://github.com/TideiSunTaipei/infinitechain_nodejs#8-finalize-stage)_ the current _stage_ when all _objections_ are settled (20)
A _stage_ is settled if and only if all the _objections_ are handled, wether the panelties are paid or _objections_ are proved to be invalid.

Server could _finalize_ to complete the current stage when all _objections_ are settled.

## Stakeholder collects _payment_ to count profit (21~24)
1. When a _stage_ is finalized, the stakeholder can fetch indexed merkle tree from Node.
2. The Stakeholder decrypts the ciphers of _payments_ included in the _indexed merkle tree_.
3. The Stakeholder counts the profit from the decrypted payments.

# Analysis
We have performed a series of experiments to demonstrate the feasibility of the proposed architecture. We install a cloud server as our auditing node. It is a virtual machine of AWS EC2 r4.xlarge with 4 CPUs. The operating system is Ubuntu 16.04. The Node.js 8.6.0 is employed to implement the auditing node. We divide the experiments into three major parts including measuring the time required to generate an index merkle tree, extract slices from an index merkle tree, and the requirement gas consuming in the function execution of smart contract in Ethereum blockchain. The time and its standard derivation required to generate an index merkle tree is shown in Table 1. For an index merkle tree which contains transactions less than 10,000, we only require about 4.6 seconds to generate it. 

##### Table 1: The time required to generate an index merkle tree
| # of transaction | Average time (ms) | Standard Deviation (ms) |
| --------: | --------: | --------: |
| 100      | 26.22     | 10.79   |
| 1,000    | 282.79    | 13.20   |
| 10,000   | 4685.59   | 175.94  |
| 100,000  | 62380.20  | 1015.32 |
| 1,000,000| 1052168.24| 44106.64|

Table 2 lists the time required to extract slices from index merkle trees which store different number of transactions with different heights. It always needs less than one minisecond to extract a slice even when an index merkle tree contains one million transactions.

##### Table 2: The time required to extract slices from index merkle trees
| # of transaction | Height of the index merkle tree | Average time (ms) | Standard Deviation (ms) |
| --------: | --------: | --------: | --------: |
| 100      | 7  | 0.47 | 1.32     |
| 1,000    | 10 | 0.18 | 0.08     |
| 10,000   | 14 | 0.30 | 0.27     |
| 100,000  | 17 | 0.42 | 1.24     |
| 1,000,000| 20 | 0.54 | 1.33     |

In the third part of our experiment. We implement the InfiniteChain in the public blockchain, Ethereum. Ethereum provides a decentralized Turing-complete virtual machine, the Ethereum Virtual Machine (EVM), which can execute scripts using an international network of public nodes. Gas is the internal pricing for running a transaction or contract in Ethereum. Using more computation and storage in Ethereum means that more gas is used (See the yellow paper for a breakdown of operations and the respective gas costs).

One fundamental reason for metering is that it provides an incentive for miners to operate the contract code. Since computation with global consensus is expensive, transactions have a gas limit field to specify the maximum amount of gas in which the sender is willing to buy. If the gas used exceeds this limit during execution, processing is stopped. It protects full nodes from attackers who could make them execute infinity loops without a gas limit. If a transaction would take longer than a block to process, then it would not be included in the block.

A block also has a field called gas limit. It defines the maximum amount of gas all transactions in the whole block combined is allowed to consume. Its purpose is to keep block propagation and processing time low, thereby allowing for a sufficiently decentralized network. The protocol allows the miner of a block to adjust the block gas limit by a factor of 1/1024 (0.0976%) in either direction. Currently, the gas limit is around eight millions. In our experiment, the highest gas is 3.7 millions in which the height of the index merkle tree is 20 for storing one million transactions in it and the transactions in the leaf node is 9. It is obvious that the size of the cryptographic proof and required gas are small enough to be performed as a function in Ethereum smart contract.

##### Table 3: Gas consumption in Ethereum blockchain platform
𝛂 = Height of the index merkle tree
𝛃 = Number of transactions in the lead nodes

|  |  𝛂=1 | 𝛂=3 | 𝛂=5 | 𝛂=7 | 𝛂=9 |
| -------- | -------- | -------- | -------- | -------- | -------- |
| 𝛃=3| 315422| 511245| 810761|1216315|1730911|
| 𝛃=5| 548841| 744664|1044305|1450118|1964195|
|𝛃=10|1138873|1334255|1633713|2039442|2553564|
|𝛃=15|1736287|1931590|2230814|2636102|3150952|
|𝛃=20|2340295|2526356|2835031|3240553|3755455|
