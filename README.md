# Awesome Taproot Asset [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

An opinionated list of everything awesome about Taproot Asset.

- [Summary](#summary)
- [How It Works](#how-it-works)
- [Architecture](#architecture)
- [Organizations](#organizations)
- [Platforms](#platforms)
- [Projects](#projects)
- [Resources](#resources)
    - [Social Media](#social-media)
    - [Influencers](#influencers)
    - [Websites](#websites)
- [Awesome Lists](#awesome-lists)
- [Contributing](#contributing)
- [Contributors](#contributors)

---

## Summary

Taproot Assets is a protocol developed by Lightning Labs for issuing and transferring custom assets, such as stablecoins and securities, on the Bitcoin blockchain and its Lightning Network. The protocol’s primary objective is to transform Bitcoin into a scalable, multi-asset network without altering its core code, creating a new token, or sacrificing its fundamental security and decentralization.

The core innovation of Taproot Assets lies in its efficient use of Bitcoin’s Taproot upgrade. Instead of cluttering the blockchain with asset-specific data for every transfer, the protocol embeds asset information within a cryptographic data structure known as a Merkle tree. This entire tree is committed to a single Bitcoin transaction output. This "off-chain but on-Bitcoin" approach ensures that asset ownership is anchored to the security of the Bitcoin chain, while the details of the assets and their transfers remain private and compact. On-chain transactions are only required for major settlement events, not for individual transfers between users.

This design delivers several key benefits:

1.  **Massive Scalability:** By integrating with the Lightning Network, Taproot Assets can be transferred instantly, in high volume, and with near-zero fees, overcoming the scalability limitations of traditional on-chain asset issuance.
2.  **Enhanced Privacy:** Asset transfers over Lightning are not publicly broadcast, providing a significant privacy advantage over transparent blockchain-based alternatives.
3.  **Efficiency and Lower Costs:** The protocol minimizes the on-chain footprint, drastically reducing the transaction fees and data load associated with managing assets.
4.  **Simplicity:** It leverages existing Bitcoin and Lightning infrastructure, avoiding the complexity and potential security vulnerabilities of sidechains or new L1 blockchains.

The most immediate and impactful use case is the integration of stablecoins, enabling a global, decentralized payment network where users can transact in familiar currencies with the speed of Lightning and the self-custodial security of Bitcoin. With the release of developer tools like `tapd` and the `litd` suite, the ecosystem is now primed for the creation of new wallets and financial applications, positioning Bitcoin not just as a store of value, but as a robust settlement layer for the future of digital finance.

## How It Works

The Taproot Assets protocol is a sophisticated method for creating and transferring assets on Bitcoin, designed for maximal efficiency, privacy, and scalability. Its operational mechanics are fundamentally different from earlier token protocols like Colored Coins or Omni Layer, which stored asset data directly and visibly on the blockchain. Instead, Taproot Assets leverages the capabilities of Bitcoin's Taproot upgrade to keep the vast majority of asset data off-chain.

**Minting (Asset Creation):** The process begins with an "asset genesis." The issuer defines the asset's properties, such as its name, total supply, and whether more can be issued in the future. This information, along with the initial distribution of the asset, is organized into a special cryptographic data structure known as a Merkle-Sum Sparse Merkle Tree (MS-SMT). This tree efficiently stores the asset data. The most crucial step is that only the single, compact cryptographic root of this entire tree is committed to the Bitcoin blockchain. This is done by embedding it within the script path of a Taproot output in a standard Bitcoin transaction. To an outside observer on the blockchain, this transaction is indistinguishable from a regular payment, providing significant privacy. The blockchain itself has no awareness that it is securing a complex asset structure; it only validates the Bitcoin transaction.

**Transferring Assets:** When a user wants to send a Taproot Asset, the transfer happens primarily off-chain. The sender does not broadcast the asset details to the entire network. Instead, they create an "asset transfer packet" and send it directly to the receiver. This packet contains all the necessary information for the receiver to independently verify the asset's validity. This includes: 1) a Merkle proof showing the specific asset exists within the on-chain genesis commitment, 2) the complete chain of ownership history for that specific asset (its "provenance"), and 3) details about the new Bitcoin UTXO where the asset will be anchored next.

The receiver performs client-side validation, meaning their software verifies the entire history of the asset back to its creation without trusting the sender or any third party. Once validated, the sender completes the transfer by creating a new Bitcoin transaction that spends the old UTXO and creates a new one for the receiver, with the asset commitment now tied to the receiver's UTXO. Again, this on-chain part looks like a normal Bitcoin transaction.

**Lightning Network Integration:** The ultimate scalability of Taproot Assets is unlocked through the Lightning Network. Assets can be locked into multi-asset Lightning channels. Once inside a channel, they can be transferred instantly and with extremely low fees, just like regular bitcoin satoshis. These transfers are off-chain Lightning payment updates, managed by specialized software that understands how to route both bitcoin and different Taproot Assets. This enables a high-throughput, low-cost payment system for any asset, from stablecoins to securities, all settled on the security of the Bitcoin network.

## Architecture

The architecture of the Taproot Assets ecosystem is a layered stack, with each component performing a specialized role. This modular design enhances security and allows for independent development while ensuring interoperability.

**1. The Bitcoin Blockchain (Layer 0):** At the base of the entire architecture is the Bitcoin network itself. Its role is singular but indispensable: to act as the ultimate court of appeal for security and finality. The blockchain is the global, decentralized time-stamping server that prevents double-spending of the Bitcoin UTXOs used to anchor the assets. It provides the protocol's security foundation but remains agnostic to the assets themselves. It does not store asset data, parse asset logic, or validate asset-specific rules. Its sole responsibility is to process standard, valid Bitcoin transactions, one of which happens to contain the cryptographic commitment (Merkle root) for a universe of Taproot Assets.

**2. The Lightning Network Daemon (`lnd`):** Sitting directly on top of the Bitcoin layer is a Lightning Network node, with `lnd` from Lightning Labs being the reference implementation. `lnd` is responsible for all core Lightning functionality. It manages the on-chain Bitcoin wallet, connects to peers, opens and closes payment channels, and routes standard Bitcoin payments. In the context of Taproot Assets, `lnd` provides the essential peer-to-peer transport layer and the channel management infrastructure upon which asset transfers are built. It handles the underlying Bitcoin mechanics required for the protocol to function.

**3. The Taproot Asset Protocol Daemon (`tapd`):** This is the core component that brings asset functionality to life. `tapd` is a specialized daemon that runs alongside and communicates directly with `lnd`. Its responsibilities are entirely focused on assets:
*   **Asset Management:** It maintains a local database known as a "Universe," which stores information about all the assets the user knows about or owns, including their genesis data and proofs.
*   **Minting and Issuance:** It provides the RPC/API calls to create new assets, generating the necessary cryptographic trees and instructing `lnd` to publish the genesis commitment on-chain.
*   **Transfer Logic:** It constructs, sends, receives, and validates the off-chain asset transfer packets described earlier.
*   **Lightning Integration:** It interfaces with `lnd` to enable multi-asset channels, ensuring that HTLCs (the core mechanism for Lightning payments) can carry and enforce asset-specific information in addition to bitcoin.

**4. The `litd` Suite (Lightning + Tapd):** To simplify development and deployment, Lightning Labs provides `litd`, a single, unified binary that packages `lnd`, `tapd`, and other liquidity services (like Loop and Pool) together. `litd` acts as a cohesive, all-in-one node that exposes a single API endpoint. This dramatically lowers the barrier to entry for developers who want to build applications like self-custodial multi-asset wallets, as they only need to manage and interact with one process instead of multiple daemons. This is the recommended architecture for building user-facing applications.

This layered architecture ensures a clean separation of concerns: Bitcoin provides security, `lnd` provides the payment network, and `tapd` provides the asset-specific logic.

## Organizations

The primary organization driving the development, specification, and implementation of the Taproot Assets protocol is **Lightning Labs**. Founded in 2016, Lightning Labs has established itself as a central and highly influential entity in the Bitcoin ecosystem, best known for creating `lnd` (Lightning Network Daemon), the most widely adopted implementation of the Lightning Network.

**Lightning Labs' Role and Philosophy:** Lightning Labs operates with a clear "Bitcoin-first" philosophy. Their mission is to scale Bitcoin and expand its capabilities without compromising its core principles of decentralization and security. Unlike projects that launch alternative blockchains or "utility tokens," Lightning Labs focuses exclusively on building layers on top of Bitcoin. Taproot Assets is the logical extension of this vision, aiming to transform Bitcoin from a single-asset network into a global, multi-asset financial settlement layer. The company is led by CEO Elizabeth Stark and CTO Laolu "roasbeef" Osuntokun, who is also the lead architect of the protocol. Their approach is heavily focused on creating robust, open-source infrastructure and providing developer-friendly tools to foster a vibrant ecosystem.

While Lightning Labs is a for-profit entity backed by prominent venture capital firms (such as a16z, Craft Ventures, and Ribbit Capital), its core protocols and software (`lnd`, `tapd`) are open-source. Their business model revolves around providing value-added services and enterprise-grade solutions that leverage this open infrastructure, most notably their suite of liquidity services like Lightning Loop (for managing channel liquidity) and Lightning Pool (a marketplace for channel liquidity). The development of Taproot Assets aligns with this model, as it creates a massive new market for these liquidity services, especially for stablecoin issuers and exchanges.

**The Broader Ecosystem:** While Lightning Labs leads the charge, the success of Taproot Assets is critically dependent on a wider, decentralized ecosystem of contributors and adopters. This includes:
*   **The Open-Source Community:** Developers from around the world can review the code, contribute improvements, and identify bugs, strengthening the protocol for everyone. The open nature of the specification (a Taproot Asset Protocol specification, or TAP) allows for alternative implementations, preventing a single point of failure.
*   **Wallet and Application Developers:** These are the builders who create the user-facing products. The success of Taproot Assets hinges on wallets integrating the protocol to allow users to easily send and receive assets like stablecoins.
*   **Exchanges and Financial Institutions:** For assets to gain liquidity and utility, major exchanges and financial service providers must support their deposit, withdrawal, and trading. Their adoption is a key metric for the protocol's real-world impact.
*   **Asset Issuers:** Companies and organizations that wish to issue stablecoins, securities, or other digital assets must choose to build on Taproot Assets, making them key stakeholders in the ecosystem.

In summary, while Lightning Labs is the central architect and driving force, it is strategically fostering an open ecosystem where the entire Bitcoin community can participate in building the future of finance on Bitcoin.

---

## Platforms

*List of all platforms e.g. wallet, trading software, trading platform, or infrastructure built for Taproot Asset*

* Wallet
    * [UXUY](https://uxuy.com/) - UXUY is a smart exchange protocol, powering the fastest and most accessible on-chain trading experience for mass adoption.
    * [Tree Root](https://treeroot.xyz) - Fastest, cheapest, easiest way to trade Taproot Asset
    * [LNbits Taproot Assets Extension](https://github.com/echennells/taproot_assets) - The Taproot Assets extension for LNbits provides integration with the Taproot Assets Protocol
* Trading Platform
    * [UXUY](https://uxuy.com/) - UXUY is a smart exchange protocol, powering the fastest and most accessible on-chain trading experience for mass adoption.
    * [Tree Root](https://treeroot.xyz) - Fastest, cheapest, easiest way to trade Taproot Asset
    * [LNFI](https://lnfi.network/) - An all-in-one financial layer for Taproot + RGB Assets
* Cloud Service
    * [Voltage](https://voltage.cloud/) - Global Lightning payments platform
* Explorer
    * [Lightning Labs](https://terminal.lightning.engineering/assets/mainnet/index.html)
    * [UXUY](https://uxuy.com/taproot)  - UXUY is a smart exchange protocol, powering the fastest and most accessible on-chain trading experience for mass adoption.
    * [Megalithic](https://megalithic.me/taproot_assets_explorer) - Supporter of Taproot Asset explorer
    * [Tree Root](https://treeroot.xyz) - Fastest, cheapest, easiest way to trade Taproot Asset

 
## Projects

*List of all projects built on top of Taproot Asset. None at the moment*

# Resources

To be updated.

## Social Media

* [Taproot Asset Telegram](https://t.me/LNTaprootAsset) - Taproot Asset Telegram group

## Influencers

*None*

## Websites

*List of all websites related to Taproot Asset e.g. Documentation*

# Awesome lists

*List of all awesome lists that I maintain*

* [Awesome RGB](https://github.com/afeezaziz/awesome-rgb)
* [Awesome AI](https://github.com/afeezaziz/awesome-ai)

# Contributing

Your contributions are always welcome! Please take a look at the [contribution guidelines](https://github.com/afeezaziz/awesome-taproot-asset/blob/main/CONTRIBUTING.md) first. If you have any question about this opinionated list, do not hesitate to contact me [@AfeezAziz](https://twitter.com/AfeezAziz) on Twitter or open an issue on GitHub.

# Contributors

<a align="center" href="https://github.com/afeezaziz/awesome-taproot-asset/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=afeezaziz/awesome-taproot-asset" />
</a>
