---

copyright:
  years: 2019
lastupdated: "2019-10-31"

keywords: network components, Kubernetes, OpenShift, allocate resources, batch timeout, reallocate resources, LevelDB, CouchDB, ordering nodes, ordering, add and remove, governance

subcollection: blockchain-rhos

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Component governance
{: #ibp-console-govern-components}

After creating CAs, peers, and ordering nodes, you can use the console to update these components in a variety of ways.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing their components in the blockchain network.

## LevelDB vs CouchDB
{: #ibp-console-govern-components-level-couch}

During the creation of a peer, it is possible to choose between two state database options: LevelDB and CouchDB. Recall that the state database keeps the latest value of all of the keys (assets) stored on the blockchain. For example, if a car has been owned by Varad and then Joe, the value of the key representing the ownership of the car would be "Joe".

Because it can be useful to perform rich queries against the state database (for example, searching for every red car with an automatic transmission owned by Joe), users will often choose a Couch database, which stores data as JSON objects. LevelDB, on the other hand, only stores information as key-value pairs, and can therefore not be queried in this way. Users must keep track of block numbers and query the blocks directly (or within a range of block numbers), and parse the information. However, LevelDB is also faster than CouchDB, though it does not support indexing.

This support for rich queries is why **CouchDB is the default database** unless a user selects the **State database selection** box during the process of adding a peer selects **LevelDB** on the subsequent tab.

Because the data is modeled differently in a Couch database than in a Level database, **the peers in a channel must all use the same database type**. If data written for a Level database is rejected by a Couch database (which can happen, as CouchDB keys have certain formatting restrictions as compared to LevelDB keys), a state fork would be created between the two ledgers. Therefore, **take extreme care when joining a channel to know the database type supported by the channel**. It might be necessary to create a new peer. Note that the database type cannot be changed after a peer has been deployed.
{:important}

## Using certificates from an external CA
{: #ibp-console-govern-components-external-CA}

Instead of using an {{site.data.keyword.blockchainfull_notm}} Platform Certificate Authority as your peer or ordering service's CA, you can use certificates from an external CA, one that is not hosted by {{site.data.keyword.IBM_notm}}, as long as the CA issues certificates in [X.509](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html#digital-certificates){: external} format.

For more information about this process, including a look at the certificates you will need, see [Using certificates from an external CA with your peer or ordering service](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-build-network#ibp-console-build-network-third-party-ca).

## Ordering node configurations
{: #ibp-console-govern-components-suggested-ordering-node-configurations}

While it's possible to use the console to build a configuration of any number of ordering nodes (no configuration is explicitly restricted), some numbers provide a better balance between cost and performance than others. The reason for this lies in satisfying the needs of high availability (HA) and in understanding the Raft concept of the "quorum", the minimum number of nodes that must be available (out of the total number) for the ordering service to process transactions.

In Raft, a **majority of the total number of nodes** is needed to form a quorum. In other words, if you have one node, you need that node available to have a quorum, because the majority of one is one. Similarly, if you have two nodes, you will need both available, since the majority of two is two (for this reason, a configuration of two nodes is discouraged; there is no advantage to a two node configuration). In a similar vein, the majority of three is two, the majority of four is three, the majority of five is three, and so on.

While satisfying the quorum will make sure the ordering service is functioning, production networks also have to think about deployment configurations that are highly available (in other words, configurations in which the loss of a certain number of nodes can be tolerated by the system). Typically, this means tolerating two nodes failing: one node going down during a normal maintenance cycle, and another going down for any other reason (such as a power outage or error).

This is why, by default, the console offers two options: one node or five nodes. Recall that the majority of five is three. This means that in a five node configuration, the loss of two nodes can be tolerated. If your configuration features four nodes, only one node can be down for any reason before **another** node going down means a quorum has been lost and the ordering service will stop processing transactions.






## How the console interacts with your Kubernetes cluster
{: #ibp-console-govern-components-iks-console-interaction}

It is the network operator's responsibility to monitor CPU, memory, and storage usage and ensure that adequate resources are available **before** attempting to create or resize a node.
{:important}

Because your instance of the {{site.data.keyword.blockchainfull_notm}} Platform console and your Kubernetes cluster do not communicate directly about the resources that are available, the process for deploying or resizing components by using the console must follow this pattern:

1. **Size the deployment that you want to make**. The **Resource allocation** panels for the CA, peer, and ordering node in the console offer default CPU, memory, and storage allocations for each node. You may need to adjust these values according to your use case. If you are unsure, start with default allocations and adjust them as you understand your needs. Similarly, the **Resource reallocation** panel displays the existing resource allocations.

  For a sense of how much storage and compute you will need in your cluster, refer to the chart after this list, which contains the current defaults for the peer, orderer, and CA.

2. **Check whether you have enough resources in your Kubernetes cluster**.  If you do not have enough space in your cluster to deploy or resize resources, you will need to increase the size of your cluster. Check the documentation of your cloud provider to learn how to scale clusters. If you have enough space in your cluster, you can continue with step 3.
3. **Use the console to deploy or resize your node**. If your Kubernetes pod is large enough to accommodate the new size of the node, the reallocation should proceed smoothly. If the worker node that the pod is running on is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1.1           | 2.2                   | 200 (includes 100GB for peer and 100GB for state database)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.7                   | 100                    |
** These values can vary slightly if you are using {{site.data.keyword.cloud_notm}} Private. Actual VPC allocations are visible in the blockchain console when a node is deployed.  

If you plan to deploy a five node Raft ordering service, note that the total of your deployment will increase by a factor of five. So a total of 1.75 CPU, 3.5 GB of memory, and 500 GB of storage for the five Raft nodes. A 4 CPU Kubernetes single worker node cluster is minimally recommended to allow plenty of CPU for the Raft cluster and any other nodes you deploy.
{:tip}

For cases when a user wants to minimize charges without bringing a node down completely or deleting it, it is possible to scale a node down to a minimum of 0.001 CPU (1 milliCPU). Note that the node will not be functional when using this amount of CPU.

While the figures in this topic endeavor to be precise, be aware that there are times when a node may not deploy even when it appears that you have enough space in your cluster. Make sure to reference your Kubernetes dashboard to see when components deploy and for error messages when they don't. In cases where a component doesn't deploy for a lack of resources, even if there seems to be enough space in the cluster, you will likely have to deploy additional cluster resources for the component to deploy.
{:important}

## Allocating resources
{: #ibp-console-govern-components-allocate-resources}

While users of a free cluster **must use default sizes** for the containers associated with their nodes, users of paid clusters can set these values during the creation of their nodes by clicking on the **Resource allocation** box during the creation of their nodes. If this box is not checked, the default resource allocations, which can be seen below, will be used.

The **Resource allocation** panel in the console provides default values for the various fields that are involved in creating a node. These values are chosen because they represent a good way to get started. However, every use case is different. While this topic will provide guidance for ways to think about these values, it ultimately falls to the user to monitor their nodes and find sizings that work for them. Therefore, barring situations in which users are certain that they will need values different from the defaults, a practical strategy is to use these defaults at first and adjust them later. For an overview of performance and scale of Hyperledger Fabric, which the {{site.data.keyword.blockchainfull_notm}} Platform is based on, see [Answering your questions on Hyperledger Fabric performance and scale](https://www.ibm.com/blogs/blockchain/2019/01/answering-your-questions-on-hyperledger-fabric-performance-and-scale/){: external}.

All of the containers that are associated with a node have **CPU** and **memory**, while certain containers that are associated with the peer, ordering node, and CA also have **storage**. For more information about storage, see [storage](/docs/services/blockchain-rhos?topic=blockchain-rhos-deploy-ocp#deploy-ocp-storage). 

You are responsible for monitoring your CPU, memory and storage consumption in your cluster. If you do happen to request more resources for a blockchain node than are available, the node will not start. However, existing nodes will not be affected.  For information about how to increase the CPU, memory, and storage, consult the documentation of your cloud provider.
{:note}

Every node has a gRPC web proxy container that bootstraps the communication layer between the console and a node. This container has fixed resource values and is included on the Resource allocation panel to provide an accurate estimate of how much space is required on your Kubernetes cluster in order for the node to deploy. Because the values for this container cannot be changed, we will not discuss the gRPC web proxy in the following sections.

### Certificate Authorities (CAs)
{: #ibp-console-govern-components-CA}

Unlike peers and ordering nodes, which are actively involved in the transaction process, CAs are involved only in the registration and enrollment of identities, and in the creation of an MSP. This means that they require less CPU and memory. To stress a CA, a user would need to overwhelm it with requests (likely using APIs and a script), or have issued so many certificates it runs out of storage. Under typical operations, neither of these things should happen, though as always, these values should reflect the needs of a particular use case.

The CA has only one associated container that we can adjust:

* **The CA itself**: Encapsulates the internal CA processes, such as registering and enrolling nodes and users, as well as storing a copy of every certificate it issues.

#### Sizing a CA during creation
{: #ibp-console-govern-components-CA-sizing-creation}

The CA has only a single container with values that you need to worry about because the values of the gRPC web proxy server cannot be changed.

| Resources | Condition to increase |
|-----------------|-----------------------|
| **CA container CPU and memory** | When you expect that your CA will be bombarded with registrations and enrollments. |
| **CA storage** | When you plan to use this CA to register a large number of users and applications. |



#### Creating highly available CAs
{: #ibp-console-govern-components-CA-HA}

For information about creating highly available CAs through the use of replica sets, see [how to configure CA replica sets](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-console-ha-ca).


### Peers
{: #ibp-console-govern-components-peers}

The peer has five associated containers:

- **Peer container**: Encapsulates the internal peer processes (such as validating transactions) and the blockchain (in other words, the transaction history) for all of the channels it belongs to. Note that the storage of the peer also includes the smart contracts installed on the peer.
- **CouchDB container**: Where the state databases of the peer are stored. Recall that each channel has a distinct state database.
- **Smart contract container**: Recall that during a transaction, the relevant smart contract is "invoked" (in other words, run). Note that all smart contracts that you install on the peer will run in a separate container inside your smart contract container, which is known as a Docker-in-Docker container.
- **gRPC proxy container**: Contains the processes that allow the console to communicate with the peer. Under normal circumstances you should not need to change the resource allocations for this container.
- **Log collector**: Pipes the logs from the smart contract container to the peer container. The CPU and memory allocations are fixed for this container.

The peer also includes a container for the **Log Collector** that pipes the logs from the smart contract container to the peer container. Similar to the gRPC web proxy container, you cannot adjust the compute for this container.

#### Sizing a peer during creation
{: #ibp-console-govern-components-peers-sizing-creation}

As we noted in our section on [How the console interacts with your Kubernetes cluster](#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these peer containers and adjust them later when it becomes apparent how they are being utilized by your use case.

| Resources | Condition to increase |
|-----------------|-----------------------|
| **Peer container CPU and memory** | When you anticipate a high transaction throughput right away. |
| **Peer storage** | When you anticipate installing many smart contracts on this peer and to join it to many channels. Recall that this storage will also be used to store smart contracts from all channels the peer is joined to. Keep in mind that we estimate a "small" transaction to be in the range of 10,000 bytes (10k). As the default storage is 100G, this means as many as 10 million total transactions will fit in peer storage before it will need to be expanded (as a practical matter, the maximum number will be less than this, as transactions can vary in size and the number does not include smart contracts). While 100G might therefore seem like much more storage than is needed, keep in mind that storage is relatively inexpensive, and that the process for increasing it is more difficult (require command line) than increasing CPU or memory. |
| **CouchDB container CPU and memory** | When you anticipate a high volume of queries against a large state database. This effect can be mitigated somewhat through the use of [indexes](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html#couchdb-indexes){: external}. Nevertheless, high volumes might strain CouchDB, which can lead to query and transaction timeouts. |
| **CouchDB (ledger data) storage** | When you expect high throughput on many channels and don't plan to use indexes. However, like the peer storage, the default CouchDB storage is 100G, which is significant. |
| **Smart contract container CPU and memory** | When you expect a high throughput on a channel, especially in cases where multiple smart contracts will be invoked at once. You should also increase the resource allocation of your peers if your smart contracts are written in JavaScript or TypeScript.|

### Ordering nodes
{: #ibp-console-govern-components-ordering-nodes}

Because ordering nodes neither maintain the State DB nor host smart contracts, they require fewer containers than peers do. But they do host the blockchain (the transaction history) because the blockchain is where the channel configuration is stored, and the ordering service must know the latest channel configuration to perform its role.

Similar to the CA, an ordering node has only one associated container that we can adjust (if you are deploying a five-node ordering service, five separate sets of ordering node containers):

* **The ordering node itself**: Encapsulates the internal orderer processes (such as validating transactions) and the blockchain for all of the channels it hosts.

#### Sizing an ordering node during creation
{: #ibp-console-govern-components-orderer-sizing-creation}

As we noted in our section on [How the console interacts with your Kubernetes cluster](#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these orderer containers and adjust them later as it becomes apparent how they are being utilized.

| Resources | Condition to increase |
|-----------------|-----------------------|
| **Ordering node container CPU and memory** | When you anticipate a high transaction throughput right away. |
| **Ordering node storage** | When you anticipate that this ordering node will be part of an ordering service on many channels. Recall that the ordering service keeps a copy of the blockchain for every channel they host. The default storage of an ordering node is 100G, same as the container for the peer itself. |

Ensuring that your ordering nodes have enough CPU and memory is considered a best practice. If an ordering service is overstressed, it might hit timeouts and start dropping transactions, requiring transactions to be resubmitted. This causes much greater harm to a network than a single peer struggling to keep up. In a Raft ordering service configuration, an overstressed leader node might stop sending heartbeat messages, triggering a leader election, and a temporary cessation of transaction ordering. Likewise, a follower node might miss messages and attempt to trigger a leader election where none is needed.
{:important}

## Reallocating resources
{: #ibp-console-govern-components-reallocate-resources}

After resizing a node, you might see a delay before it takes effect because containers are being rebuilt.
{:important}



Third party tools such as [Sysdig](https://sysdig.com){: external} can be used to help monitor the usage in your cluster. If you determine that a worker node is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.
{:note}


While it takes less effort to deploy enough resources to your Kubernetes cluster from the start and therefore be able deploy and expand resources without having to increase the resources in your cluster, the bigger the deployment, the more it will cost. Users need to consider their options carefully and recognize the tradeoffs that they are making regardless of the option that they choose.




You can scale your cluster by monitoring your nodes and following the instructions available from your cloud provider to add more nodes, larger nodes, or increasing the size of the nodes, depending on the options available in your cloud provider. The method you will use to increase storage will depend on the storage class you chose for your cluster. Note that if you are about to exhaust the storage on your peer or ordering node, you might need to deploy a new peer or ordering node with more storage and let it sync via your other components on the same channels.




