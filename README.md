# Awesome Taproot Asset [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

An opinionated list of everything awesome about Taproot Asset.

- [Summary](#summary)
- [Awesome Taproot Asset](#awesome-taproot-asset)
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

## Website

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
