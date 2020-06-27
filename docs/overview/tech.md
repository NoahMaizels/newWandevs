## Gwan

Gwan is the GO implementation of Wanchain and the command line interface for running a full Wanchain node. Gwan allows users to interact with the Wanchain network, create accounts, transfer funds, deploy and interact with contracts. To learn more, check out the [Github page](https://github.com/wanchain/go-wanchain) or navigate to our [developer portal](https://wandevs.org/).

## Privacy Protection

Wanchain supports private transactions based on ring signatures, the same privacy algorithm powering other popular private cryptocurrencies such as Monero. This is an essential functionality for many business use cases such as private wealth management, salary issuance, and many more. If you would like to send a private transaction using the Wanchain official GUI wallet, you can find the instructions [here](wallet_and_tools/wallet-install?id=private-transactions). If you are a developer and would like to build private transactions into your app or would just like to know how to send private transactions using the CLI, please navigate to our [developer portal](https://wandevs.org/).

## iWan

iWan is the "interface to Wanchain", which provide users with secure, dependable access to the full array of Wanchain full node services, including the Wanchain main network and test network. Through the API and developer tools provided by iWan, developers now have access to both native Wanchain and cross chain services including cross chain transactions, account management, smart contract querries, and much more. iWan provides the core Wanchain interface for developers so that they can focus on building their business layer without worrying about the cost and complexity of setting up their own full node. Learn more at the official [iWan site](https://iwan.wanchain.org/).

## Smart Contracts

Wanchain supports smart contracts written in Solidity and can be developed using the same tools developers are familiar with from working on Ethereum. This allows blockchain developers with experience developing on Ethereum to quickly and easily leverage Wanchain's cross-chain capabilities since there is no need to learn a new smart contract scripting language or figure out new developers tools. 

![](media/crosschain.png)

## Cross-chain

With the rapid rise of blockchain technology, there are now thousands of public and private blockchains and blockchain platforms being used across the globe. However, these blockchains exist largely in isolation, unable to exchange information or value with one another. This severely limits their world-shaping potential. The purpose of a cross-chain solution is to connect different blockchains, like a bridge between islands. These connections will provide innumerable benefits, including enabling the near-instant and automated exchange of information and value across platforms, enhancing liquidity in the market and providing a distributed clearing mechanism.

### The Technical Problems

At present, there is no universally accepted cross-chain mechanism due to two difficult-to-overcome technical roadblocks that any viable cross-chain solution must solve for. The first is ensuring that the total number of tokens on the original blockchain do not decrease or increase following a cross-chain transaction - also known as the law of value conservation. We'll call this Problem Alpha.

As an example, when transferring value from Ethereum to Wanchain, you must ensure that the value that is transferred to the Wanchain blockchain is no longer accessible on the Ethereum blockchain - otherwise, a user could use the same finite amount of Ethereum to unlock unlimited new value on Wanchain. At the same time, you cannot destroy the Ethereum being transferred, as Ethereum is a limited and exhaustible resource. Destroying the transferred Ethereum would prevent two-way transactions, as Wanchain would be unable to generate new Ethereum tokens when the tokens need to cross back to the Ethereum blockchain.

As a result, when a cross-chain transaction is executed, the value that is made available on the destination chain must simultaneously be made unavailable, but not destroyed, on the original chain.

The second roadblock is finding a way to verify a cross-chain transaction on the blockchain on which it was initiated, in a trustless manner. We'll call this problem Beta.

This issue arises when developing any solution to Problem Alpha. As we've just established, when a cross-chain transaction is executed, the value that is made available on the destination chain must be simultaneously made unavailable on the original chain. For this to be executed properly, however, the original chain must receive immutable, trustless verification that the desired value has been made available to the recipient on the destination chain, before making that same value unavailable on the original chain.

As an example, if a user initiates a cross-chain transaction from Ethereum to Wanchain, and the value is made unavailable on Ethereum before there is definitive verification that the corresponding value has been made available on Wanchain, the user would have no access to either his original Ethereum or the corresponding value that was supposed to be made available on Wanchain - effectively losing their funds for good.
This issue is difficult to solve for, because different blockchains have different protocols for verifying transactions. As a result, it is difficult for any one blockchain to directly read another blockchain and determine if a transaction has been verified, unless those blockchains have been intentionally developed to be compatible.

### Wanchain's Solutions

To solve for Problem Alpha and ensure that the total number of tokens on each blockchain remains static when being transferred from one chain to the next, Wanchain's specialized nodes - also known as Storemen - employ an innovative, [secure multi party computation](../technology/smpc.md) method with a threshold-protected secret key to process cross-chain transactions.

For every cross-chain transaction, Wanchain's Storemen nodes create a locked account that holds the funds being sent from the original blockchain indefinitely, while an equivalent value is made available on the destination blockchain in the form of a corresponding mapping token. The original funds can then only be released when the value of the corresponding mapping tokens is sent back to the original chain, and the mapping tokens themselves are destroyed.

For example, when executing a transaction from Ethereum to Wanchain, the funds being sent from the Ethereum chain will be held in a locked account, while a corresponding amount WETH - Wanchain's Ethereum mapping token - will be made available to the recipient. The locked Ethereum is only made available again when the WETH holder sends that WETH to a HTLC account on Wanchain monitored by the cross chain Storeman nodes. At that point, the WETH is destroyed and the locked Ethereum is released to the target Ethereum account.

When any cross-chain transaction is initiated, the owners of each Storeman node must jointly participate in the generation of the related locked account's public and private keys. The shared account's private key is not actually a static string of numbers and letters, but a series of key fragments that is scattered amongst the account participants present at the time the private key is generated.

In order to release funds from these locked accounts, the operators of a Storemen node must jointly agree to do so by contributing their respective pieces of the node's private key to generate a signature. This is the secure multiparty computation piece of the transaction processing procedure, and it prevents any one bad Storeman actor from being able to carry out a double spend attack by unlocking tokens being held in any locked account generated from a cross-chain transaction.

In order to guarantee that a transaction can be executed at any given time, it is not necessary for all participants to fully engage in the process of releasing funds from a locked account - instead, the number Storemen account participants must be above a set threshold (M out of N participants, M<=N). This is the threshold key-sharing piece of Wanchain's solution. This solution ensures that, for the funds in a locked account to be wrongfully released, a very high percentage of Storemen must agree to collaborate as bad actors. In conjunction with the significant economic incentives to not act in bad faith, this system projects to be an extremely effective system for deterring fraudulent behavior.

To solve for Problem Beta, verifying transactions on the original blockchain in a trustless manner, Wanchain eventually plans to implement a new third-party consensus mechanism called The Voucher. The Voucher will be a consensus group that is economically incentivized to verify the finality of the transactions sent to the destination chain on the original chain.

To ensure Wanchain is viable as a cross-chain solution while the Wanchain team works towards perfecting the implementation of the more elegant and dynamic Voucher mechanism, the system outlined above is executed using atomic swaps - circumventing the need for cross-chain verification. When ETH is transferred to Wanchain as WETH, a user is essentially exchanging their ETH for WETH in an atomic swap, where the other party is the Storeman node group. While atomic swaps are not the most scalable or efficient solution, they are a reliable and effective means of transferring value across chains that will allow Wanchain to be an immediately functional cross-chain platform. 

## WanDex

WanDex is a decentralized exchange framework developed by Wanchain community developers and is based on the principle of "off-chain matching and on-chain settlement".

It provides real utility value for Wanchain's cross-chained assets, and it is the world's first cross-chain DEX platform.

WanDex supports cross-chain transaction feature for a group of major crypto-currencies, such as BTC, ETH, EOS, and various ERC20 tokens on their mainnet.

Developers and operators can easily and freely create their own DEX on Wanchain using WanDex framework.

You can customize your DEX rendering using open source front-end codes, and add any marketing campaign policy or other interesting features to your DEX.

Multiple DEX based on WanDex framework can share a common order book for the same trading pair.

All DEX based on WanDex framework can have their own unique trading pairs.


![UI](./media/01wandex.png)

Main components of WanDex can be categorized into three groups:

- Fully open source [front-end code](https://github.com/wandevs/dex-front-end)
- Fully open source [smart contract source code](https://github.com/wandevs/dex-smart-contract)
- Open-source community controlled [off-chain matching engine](https://demodex.wandevs.org:43001/)

Its workflow and interaction between components are shown in the following:

![workflow and interaction between components](./media/02wandex.png)

User's crypto-asset is controlled by user's own blockchain wallet and can be verified on the blockchain at any time.

Different DEX operators can modify their web interface or mobile application based on open source front-end code for users to browse and place orders.

After the order is completed in the matching engine, it is settled in a smart contract on the chain.

Only orders authorized by user's signature will trigger asset transfer during settlement phase.

Different DEX can leverage from their own unique trading pairs, transaction fee policy and shared order book.

WanDex's API uses restful mode, real-time information feed uses WebSocket interface to trigger notifications.

The back-end components framework is shown in the following figure:

![schematic diagram of back-end frame](./media/03wandex.png)

## Secure Multi Party Computation (SMPC)

SMPC is one of the core technologies underlying much of Wanchain's powerful technology. Essentially, SMPC allows for multiple parties to do some calculation on some data without revealing all the data to any one party. Each party will only receive one piece of the total data. This is especially useful when dealing with the private keys which are used to secure cryptocurrencies such as WAN, BTC, ETH, and others. This allows for a group of decentralized nodes to create cryptocurrency transactions for a certain account without one node owner ever being able to gain access to the private key which controls that account. This is one of the core technologies which make Wanchain's cross chain transactions possible. Click [here](../technology/cross-chain.md) to learn more about how SMPC is used in Wanchain's cross chain transactions. You can also check out the [Wikipedia article](https://en.wikipedia.org/wiki/Secure_multi-party_computation) for an in depth dive into the technology.

## Storeman Nodes  

Wanchain's storeman node system is at the core of its cross chain technology. Storeman nodes are nodes which verify cross chain transactions and ensure the smooth and secure transfer of value between heterogenous blockchains. Through the application of multipary computation and threshold secret sharing, the storeman nodes work together to perform cross chain transactions without revealing the private keys of the accounts involved. To ensure their honest behavior, node operators are required to put up collateral which covers the value of any transactions they process. 



