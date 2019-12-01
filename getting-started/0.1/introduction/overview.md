# IOTA overview

**IOTA is an open-source distributed ledger technology that allows [connected devices](https://en.wikipedia.org/wiki/Connected_Devices) to transfer data and [IOTA tokens](../clients/token.md) among each other for zero fees.**

<iframe src="https://www.youtube.com/embed/Gr-LstcDcAw" frameborder="0" width="560" height="315" style="max-width: 100%;"></iframe>

## How does IOTA work?

An IOTA network consists of [nodes](../network/nodes.md) and clients, and anyone is free to act as either in the [public IOTA networks](../network/iota-networks.md).

### Nodes

Nodes are the backbone of an IOTA network as they are the only devices that have read and write access to the immutable record of transactions called the Tangle.

Interconnected nodes form an IOTA network by running the same [node software](root://node-software/0.1/introduction/overview.md), allowing them to validate transactions and attach them to the Tangle.

### Clients

Clients are the devices that connect to nodes to transact or store data on the Tangle.

All clients in an IOTA network have a secret password called a [seed](../clients/seeds.md), which acts as their identity. Seeds give clients access to [addresses](../clients/addresses.md), which are like accounts with a balance of IOTA tokens.

To transfer IOTA tokens or even to send only data, clients package the transfer instructions into objects called [transactions](../transactions/transactions.md) and send them to a node to attach to the Tangle.

## Why should I use IOTA?

By using IOTA, you can build applications that benefit from the following:

- **Authenticity:** Prove that you sent data and/or own IOTA tokens
- **Integrity:** Prove that your data is unchanged
- **Confidentiality:** Control who has access to your data through encryption
- **Micropayments:** Send small amounts of IOTA tokens without paying any fees 

IOTA networks have many benefits compared to traditional ways of transferring data or value.

### Trust

Each node in an IOTA network validates transactions, then sends them to other nodes that do the same. As a result, all valid transactions are agreed on by all nodes, removing the need to trust a single one in the network.

You can even [run your own node](../how-to-guides/run-your-own-iri-node.md) to start validating transaction in the network.

### Immutability

All transactions in the Tangle are immutable and transparent.

Each transaction references the transaction hashes of two previous ones. So, if the contents of any transaction were to change, the hashes would be invalid, making the transactions invalid.

### Security

IOTA uses quantum-resistant one-time [signatures](../clients/signatures.md) to stop attackers from stealing IOTA tokens.

IOTA networks are peer-to-peer networks where no central authority controls the Tangle. Instead, all nodes hold a copy of it and reach a consensus on its contents.

### Cost saving

IOTA is free to use. You don't need to pay a subscription, or sign a contract. Even transactions are feeless.

You can store data on the Tangle with no restrictions. All you need is a node to which you can send transactions.

### Scalability

For each transaction that's attached to the Tangle, two previous transactions are validated. This process makes IOTA incredibly scalable because more new transactions lead to faster validations.

## What are some example use cases?

IOTA is a secure platform for sharing and accessing data, using a single source of truth. As such, IOTA can benefit many industries by improving efficiency, increasing production, and ensuring data integrity.

- [Mobility](https://www.iota.org/verticals/mobility-automotive)
- [Global trade and supply chains](https://www.iota.org/verticals/global-trade-supply-chains)
- [Industrial IoT](https://www.iota.org/verticals/industrial-iot)
- [Healthcare](https://www.iota.org/verticals/ehealth)
- [Energy](https://www.iota.org/verticals/smart-energy)

These are some applications of IOTA:

| **Company**  | **Description** |  **References** |
| :-------:| :-------:| :---------: |
| bIOTAsphere|bIOTAsphere connected a Tesla car to an IOTA network. The car buys dynamic insurance with IOTA tokens |[YouTube video that showcases this use case](https://www.youtube.com/watch?v=2zvrA5KqeYw) |
| iampass|iampass is an identity and access management system that authenticates users by taking an encrypted scan of their palms and checking for that data on the Tangle in an IOTA network  | [iampass website](https://iampass.io/)|
|+CityxChange consortium |CityxChange consortium is a project that builds smart cities, which use IOTA as the secure data transfer layer |[+CityxChange website](http://cityxchange.eu/)|
eCl@ss|eCl@ss is cooperating with IOTA to share and store ISO/IEC-compliant product classifications and descriptions|[eCl@ss website](https://www.eclass.eu/en/association/cooperation.html)|
|Elaadnl| Elaadnl is creating electric car chargers that accept payment in IOTA tokens| [Elaadnl website](https://www.elaad.nl/news/worlds-first-iota-charging-station-released/)

## Where do I start?

**Non-developers:** Use the [official Trinity wallet](root://wallets/0.1/trinity/introduction/overview.md) to create and store your seed, send and receive transactions, and more.

**Developers:** [Get started with one of the client libraries](../how-to-guides/get-started.md) or [join an IOTA network by running your own node](../how-to-guides/run-your-own-iri-node.md)

**Exchanges:** Use the [official Hub wallet](root://wallets/0.1/hub/introduction/overview.md) to integrate IOTA into your exchange.










