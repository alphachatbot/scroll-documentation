---
section: developers
date: Last Modified
title: "L1 and L2 Briding"
lang: "en"
permalink: "TODO"
excerpt: "TODO"
---

# L1 and L2 Bridging

The Scroll bridge enables the transfer of ETH, ERC20 tokens, NFTs, and arbitrary messages between the Goerli and Scroll Alpha. It serves as a secure mechanism for moving various digital assets across L1 and L2.

To facilitate the transfer of ETH and ERC20 tokens, the Scroll bridge utilizes the Gateway Router. This contract ensures the smooth passage of these assets between the Goerli and Scroll Alpha networks, allowing users to transfer their Ethereum-based tokens seamlessly.

The ERC721 and ERC1155 Gateway enables the transfer of non-fungible assets between the two networks, allowing users to move their NFTs across the Goerli and Scroll Alpha networks.

In addition to token transfers, the Scroll Messenger contract enables cross-chain contract interaction. This means that contracts on one network can interact with contracts on the other network through the Scroll Messenger contract. This functionality expands the possibilities for decentralized applications and smart contracts to operate seamlessly across both networks.

## L1 Gateway architecture

<figure><img src="../../.gitbook/assets/L1GatewayWHITE.png" alt=""><figcaption><p>L1 Gateway</p></figcaption></figure>

There are many entry points from the user to the Scroll bridge. This will depend on what you want to do and how you want to do it. If you want to send ETH or ERC20 tokens, you should use the `GatewayRouter` . If you want to send NFTs, you should use the `L1ERC721Gateway` or `L1ERC1155Gateway`. If you want to send arbitrary data, you should use the `L1ScrollMessenger`. All Gateway transfers use the Scroll Messenger to send assets cross-chain, whose job is to append the transactions to the Message Queue for L2 inclusion.

## L2 Gateway architecture

<figure><img src="../../.gitbook/assets/bridge &#x26; rollup-withdrawWHITE (1).png" alt=""><figcaption><p>Bridge and rollup withdraws</p></figcaption></figure>

Regarding possible permissionlessly callable entry points, the L2 Gateway Architecture is very similar to L1. The difference is that when sending a message from L2, calling the `appendMessage` function will store the message in an append-only binary merkle tree (aka withdraw tree) in the `L2MessageQueue`. When a new message is sent to the `L2MessageQueue`, the relayer will detect it and store it in the database. When the block is finalized, it will generate a proof of the new merkle path and pass it to the L1geth node to execute on `L1ScrollMessenger` . All finalized withdraw roots will be stored in the rollup contract so we can verify the proof against them. In the next Scroll versions, the Relayer won't be needed since all users will be able to finalize the transaction on L1.

In the upcoming sections, we will explore the technical aspects of the bridge, including the smart contract API required to utilize its capabilities. Detailed guides with code examples are provided in the Developer Guides section to assist developers and users in understanding and implementing these functionalities.