---

copyright:
  years: 2018, 2019
lastupdated: 2019-10-22

keywords: blockchain components, ca, certificate authorities, peer, ordering service, orderer, channel, smart contract, applications

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Blockchain component overview
{: #blockchain-component-overview}

The components and structure of the {{site.data.keyword.blockchainfull}} Platform are based on the underlying infrastructure and tools of [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}, an open-source permissioned blockchain solution to which {{site.data.keyword.IBM_notm}} is a major contributor. Networks based on Fabric include several standard components that can be deployed in a number of configurations to support a wide variety of use cases.

For a more comprehensive view of Fabric networks, and the interrelation of the components that comprise it, check out [this document on the structure of a blockchain network](https://hyperledger-fabric.readthedocs.io/en/release-1.4/network/network.html) from the Fabric community documentation, which shows how a network can be started and matured.

For the purposes of this overview, we'll focus just on certificate authorities (CAs), orderers, peers, smart contracts, and applications. As you can see from the [Build a network tutorial](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-build-network#ibp-console-build-network), this sequence is not arbitrary; it reflects the sequence in which components in a network based on Fabric will be deployed.

## Peers
{: #blockchain-component-overview-peer}

At a physical level, a blockchain network is comprised primarily of peer nodes (or, simply, peers). Peers are the fundamental elements of the network because they host ledgers and smart contracts (which are contained in ["chaincode"](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/chaincodenamespace.html){: external}). More accurately, the peer hosts **instances** of the ledger, and **instances** of smart contracts. Because smart contracts and ledgers are used to encapsulate the shared processes and shared information in a network, respectively, these aspects of a peer make them a good starting point to understand what a Fabric network actually does.

To learn more about peers specifically, check out [Peers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html){: external} from the Fabric community documentation.

The {{site.data.keyword.blockchainfull_notm}} Platform console allows peers to be created, joined to channels, have smart contracts installed on them, made into anchor peers, and be seamlessly upgraded.

## Certificate Authorities (CAs)
{: #blockchain-component-overview-ca}

The underpinning of a blockchain network based on Fabric is identities and permissions. Identities take the form of x.509 certificates issued by a CA and can be compared to a credit card in that they *identify* someone within a particular context (credit cards identify an individual in terms of banking transactions, while Fabric identities allow users to be identified in a blockchain context). This identity can include attributes about them, for example linking them to a particular organization or organizational unit (OU). These certificates are then linked to the ability to perform actions on a network through their inclusion in a Membership Service Provider (MSP), folders containing certificates that exist at the component and channel level.

A peer MSP, for example, will have an MSP subfolder called **admins**. Any user whose certificate is inside that admin folder is an admin of the peer. A validation system inside the peer performs a check whenever a user, identified by their signing certificate, tries to perform an administrative action. Does the certificate match the one in the "admin" folder? If it does, then the action can be performed. If not, the request to do the action will be rejected.

{{site.data.keyword.blockchainfull_notm}} Platform CAs are based on the [Hyperledger Fabric CA](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/){: external}, though it is possible to use another CA as long as it uses a PKI based on x.509 certificates. Because non-Fabric CAs are not configured to create properly formatted MSPs, users who want to use this kind of CA will have to create the MSP for themselves.

For more details about how certificate authorities are used to establish identity and membership, see [Hyperledger Fabric documentation on identity](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html){: external} and on [membership](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external}.

## Ordering services
{: #blockchain-component-overview-orderer}

Many distributed blockchains, such as Ethereum and Bitcoin, are not permissioned, which means that any node can participate in the consensus process where transactions are ordered and bundled into blocks. Because of this fact, these systems rely on probabilistic consensus algorithms which eventually guarantee ledger consistency to a high degree of probability, but which are still vulnerable to divergent ledgers (also known as a ledger “fork”), where different participants in the network have a different view of the accepted order of transactions.

Hyperledger Fabric works differently. It features a kind of a node called an orderer (it’s also known as an “ordering node”) that does this transaction ordering, which along with other nodes forms an ordering service. Because Fabric’s design relies on deterministic consensus algorithms, any block a peer receives from the ordering service and validates is guaranteed to be final and correct. Ledgers cannot fork the way they do in many other distributed blockchains.

In addition to promoting finality, separating the endorsement of smart contract execution (which happens at the peers) from ordering gives Fabric advantages in performance and scalability, eliminating bottlenecks which can occur when execution and ordering are performed by the same nodes.

The ordering service performs one other key function: it maintains what is known as the "system channel", a default channel which hosts the "consortium", the list of organizations which can create and join application channels. Because some aspects of the configuration of application channels are inherited by default from the system channel (certain capability levels, for example), it is the job of ordering service admins to establish these defaults.

The {{site.data.keyword.blockchainfull_notm}} Platform uses an implementation of the Raft protocol, in which a leader node is dynamically elected among the ordering nodes in a channel (this collection of nodes is known as the “consenter set”), and that leader replicates messages to the follower nodes. Because the system can sustain the loss of nodes, including leader nodes, as long as there is a majority of ordering nodes (what’s known as a “quorum”) remaining, Raft is said to be “crash fault tolerant” (CFT). In the {{site.data.keyword.blockchainfull_notm}} Platform, users have the ability to select a single node ordering service or a five node ordering service, and then to add or remove nodes as needed. Ordering services can also be administered by several organizations.

For more information about the ordering service, see [The Ordering Service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html){: external}.

## Channels
{: #blockchain-component-overview-channels}

A channel is a mechanism that provides an open layer of communication between members in the network. Multiple channels can be created between subsets of members, thus supporting one of the [many mechanisms to implement privacy](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}. Data on the blockchain network is stored on a ledger, with each channel having its own separate ledger. These ledgers are hosted on the peers that have joined the channel. The {{site.data.keyword.blockchainfull_notm}} Platform allows channels to be easily created and managed. Channel configuration updates allow the members of a channel to edit parameters as they see fit. For example, more members can be added to a channel, or the capabilities of a channel can be changed. Because changes to a channel must be approved by channel members, the {{site.data.keyword.blockchainfull_notm}} Platform has a mechanism for the collection of necessary signatures.

For more information about channels and how to use them, see the [Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-1.4/channels.html){: external}.

## Smart contracts
{: #blockchain-component-overview-smart-contracts}

Before businesses can transact with each other, a common understanding about rules and processes must be reached and defined in some sort of contract. Taken together, these contracts lay out the "business model" that governs all of the interactions between business partners.

The same need exists in blockchain networks, and where the industry term for these business models is "smart contracts", Fabric and {{site.data.keyword.blockchainfull_notm}} Platform contain these contracts in a larger structure known as "chaincode", which includes not just the business logic but the underlying infrastructure that validates the identities of the users attempting to invoke the smart contract.

Where contracts in the business world are signed and filed with law firms, smart contracts are installed on peers and "instantiated" on a channel.

For more information about smart contracts, see [Smart contracts](https://hyperledger-fabric.readthedocs.io/en/release-1.4/smartcontract/smartcontract.html){: external}.

## Applications
{: #blockchain-component-overview-applications}

Client applications in a Fabric-based network like {{site.data.keyword.blockchainfull_notm}} Platform leverage underlying infrastructures such as APIs, SDKs, and smart contracts to allow client interactions (invokes and queries) at a higher level of abstraction.

For a look at how applications interact with a network based on Fabric, check out the [Developing Applications](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} topic in the Hyperledger Fabric documentation. You can also visit the [creating applications](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-app#ibp-console-app) topic to learn how to connect your applications to {{site.data.keyword.blockchainfull_notm}} Platform.

## An example network
{: #blockchain-component-overview-example-network}

**Figure 1** depicts an example of a deployed blockchain network that consists of four members (each owning two peers), Certificate Authorities that are responsible for distributing cryptographic identity material, and an ordering service that defines policies and network participants. The blue channel contains all four network members and the yellow channel is restricted to only three members: Banks B, C, and D. You can also see that Bank A plays the role of network initiator and that Bank D exists only as a member in the context of the yellow channel. Lastly, a user or application in possession of a properly signed x509 certificate can send calls to peers on the network.

![Blockchain Network](images/blockchain_network_2-01.png "Example blockchain network"){: caption="Figure 1. An example blockchain network with four members that leverage channels to isolate data" caption-side="bottom"}

