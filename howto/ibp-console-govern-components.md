---

copyright:
  years: 2019
lastupdated: "2019-10-01"

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

## How the console interacts with your Kubernetes cluster
{: #ibp-console-govern-components-iks-console-interaction}

It is the network operator's responsibility to monitor CPU, memory, and storage usage and ensure that adequate resources are available **before** attempting to create or resize a node.
{:important}

Because your instance of the {{site.data.keyword.blockchainfull_notm}} Platform console and your Kubernetes cluster do not communicate directly about the resources that are available, the process for deploying or resizing components by using the console must follow this pattern:

1. **Size the deployment that you want to make**. The **Resource allocation** panels for the CA, peer, and ordering node in the console offer default CPU, memory, and storage allocations for each node. You may need to adjust these values according to your use case. If you are unsure, start with default allocations and adjust them as you understand your needs. Similarly, the **Resource reallocation** panel displays the existing resource allocations.

  For a sense of how much storage and compute you will need in your cluster, refer to the chart after this list, which contains the current defaults for the peer, orderer, and CA.

2. **Check whether you have enough resources in your Kubernetes cluster**. If you are using a Kubernetes cluster hosted in {{site.data.keyword.cloud_notm}}, we recommend using the [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} tool in combination with your {{site.data.keyword.cloud_notm}} Kubernetes dashboard. If you do not have enough space in your cluster to deploy or resize resources, you need to increase the size of your cluster. For more information about how to increase the size of a cluster, see [Scaling clusters](/docs/containers?topic=containers-ca#ca){: external}. If you are using a cloud provider other than {{site.data.keyword.cloud_notm}}, you will have to consult their documentation to learn how to scale clusters. If you have enough space in your cluster, you can continue with step 3.
3. **Use the console to deploy or resize your node**. If your Kubernetes pod is large enough to accommodate the new size of the node, the reallocation should proceed smoothly. If the worker node that the pod is running on is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1.1           | 2.4                   | 200 (includes 100 GB for peer and 100 GB for CouchDB)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.9                   | 100                    |

If you plan to deploy a five node Raft ordering service, note that the total of your deployment will increase by a factor of five. So a total of 1.75 CPU, 4.5 GB of memory, and 500 GB of storage for the five Raft nodes. A 4 CPU Kubernetes single worker node cluster is minimally recommended to allow plenty of CPU for the Raft cluster and any other nodes you deploy.
{:tip}

For cases when a user wants to minimize charges without bringing a node down completely or deleting it, it is possible to scale a node down to a minimum of 0.001 CPU (1 milliCPU). Note that the node will not be functional when using this amount of CPU.

While the figures in this topic endeavor to be precise, be aware that there are times when a node may not deploy even when it appears that you have enough space in your cluster. Make sure to reference your Kubernetes dashboard to see when components deploy and for error messages when they don't. In cases where a component doesn't deploy for a lack of resources, even if there seems to be enough space in the cluster, you will likely have to deploy additional cluster resources for the component to deploy.
{:important}

## Allocating resources
{: #ibp-console-govern-components-allocate-resources}

While users of a free cluster **must use default sizes** for the containers associated with their nodes, users of paid clusters can set these values during the creation of their nodes.

The **Resource allocation** panel in the console provides default values for the various fields that are involved in creating a node. These values are chosen because they represent a good way to get started. However, every use case is different. While this topic will provide guidance for ways to think about these values, it ultimately falls to the user to monitor their nodes and find sizings that work for them. Therefore, barring situations in which users are certain that they will need values different from the defaults, a practical strategy is to use these defaults at first and adjust them later. For an overview of performance and scale of Hyperledger Fabric, which the {{site.data.keyword.blockchainfull_notm}} Platform is based on, see [Answering your questions on Hyperledger Fabric performance and scale](https://www.ibm.com/blogs/blockchain/2019/01/answering-your-questions-on-hyperledger-fabric-performance-and-scale/){: external}.

All of the containers that are associated with a node have **CPU** and **memory**, while certain containers that are associated with the peer, ordering node, and CA also have **storage**. For more information, see [storage](/docs/services/blockchain-rhos?topic=blockchain-rhos-deploy-ocp#deploy-ocp-storage).

You are responsible for monitoring your CPU, memory and storage consumption in your cluster. If you do happen to request more resources for a blockchain node than are available, the node will not start, but existing nodes are not affected.  For information about how to increase the CPU, memory, and storage in other cloud providers, consult the documentation of your cloud provider.
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

As we noted in our section on [How the console interacts with your Kubernetes cluster](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-govern-components#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these peer containers and adjust them later when it becomes apparent how they are being utilized by your use case.

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

#### Sizing an orderer during creation
{: #ibp-console-govern-components-orderer-sizing-creation}

As we noted in our section on [How the console interacts with your Kubernetes cluster](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-govern-components#ibp-console-govern-components-iks-console-interaction), it is recommended to use the defaults for these orderer containers and adjust them later as it becomes apparent how they are being utilized.

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

As we said above, if your cloud provider is {{site.data.keyword.cloud_notm}}, we recommend using the [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} tool in combination with the OpenShift Web Console to monitor your Kubernetes resource usage. If you determine that a worker node is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.
{:note}

While it takes less effort to deploy enough resources to your Kubernetes cluster from the start and therefore be able deploy and expand resources without having to increase the resources in your cluster, the bigger the deployment, the more it will cost. Users need to consider their options carefully and recognize the tradeoffs that they are making regardless of the option that they choose.

If your cloud provider is not {{site.data.keyword.cloud_notm}}, you can scale your cluster manually, by monitoring your nodes and either adding more nodes or larger nodes. While this process can be labor intensive, it has the advantage of allowing the user to always be certain what is being charged to their cloud account. Follow the instructions available from your cloud provider.

The method you will use to increase storage will depend on the storage class you chose for your cluster. Refer to the documentation of your cloud provider to learn about this. In {{site.data.keyword.cloud_notm}}, this topic is called [storage options](/docs/openshift?topic=openshift-kube_concepts){: external}. Note that in {{site.data.keyword.cloud_notm}}, if you are about to exhaust the storage on your peer or ordering node, you must deploy a new peer or ordering node with a larger file system and let it sync via your other components on the same channels.

In {{site.data.keyword.cloud_notm}}, CPU and memory can be increased using the console (if you have resources available in your {{site.data.keyword.cloud_notm}} Kubernetes Service cluster). However, storage must be increased using the {{site.data.keyword.cloud_notm}} CLI. For a tutorial on how to do this, see [Changing the size and IOPS of your existing storage device](/docs/openshift?topic=openshift-file_storage#file_change_storage_configuration){: external}. If you are using a cloud provider other than {{site.data.keyword.cloud_notm}}, refer to the documentation of that provider for the process on increasing CPU, memory, and storage.

### Monitoring file storage
{: #ibp-console-govern-components-monitor-storage}

If you are using {{site.data.keyword.cloud_notm}}, you can view your consumption of file storage by navigating to your {{site.data.keyword.cloud_notm}} dashboard. Click on the menu button in the upper left hand corner. Then click on **Classic infrastructure**, **Storage**, and then **File Storage**. This will display the capacity and usage for each persistent volume claim (PVC). This usage can be mapped by your {{site.data.keyword.blockchainfull_notm}} Platform nodes by clicking on the cell in the **Notes** column.

You will see something that looks like this:

```
PVC {"plugin":"ibm-file-plugin-77497497cc-5qm58","region":"us-south","cluster":"05ccfca248dc4c389978d074524e0a8e","type":"Endurance","ns":"n4c817f","pvc":"n4c817forg1ca-fabric-ca-pvc","pv":"pvc-63074ac8-8b9b-11e9-88db-222bd59fb4bc","storageclass":"default","reclaim":"Delete"}
```

To see the same information from the command line, login to your cluster and enter this command:

```
ibmcloud sl file volume-list --column id --column notes
```

This will allow you to map the output from the pods to the nodes you have deployed.

## LevelDB vs CouchDB
{: #ibp-console-govern-components-level-couch}

During the creation of a peer, it is possible to choose between two state database options: LevelDB and CouchDB. Recall that the state database keeps the latest value of all of the keys (assets) stored on the blockchain. For example, if a car has been owned by Varad and then Joe, the value of the key representing the ownership of the car would be "Joe".

Because it can be useful to perform rich queries against the state database (for example, searching for every red car with an automatic transmission owned by Joe), users will often choose a Couch database, which stores data as JSON objects. LevelDB, on the other hand, only stores information as key-value pairs, and can therefore not be queried in this way. Users must keep track of block numbers and query the blocks directly (or within a range of block numbers), and parse the information. However, LevelDB is also faster than CouchDB, though it does not support indexing.

This support for rich queries is why **CouchDB is the default database** unless a user selects the **Advanced deployment options** tab in the Create Peer flow and selects **LevelDB**.

Because the data is modeled differently in a Couch database than in a Level database, **the peers in a channel must all use the same database type**. If data written for a Level database is rejected by a Couch database (which can happen, as CouchDB keys have certain formatting restrictions as compared to LevelDB keys), a state fork would be created between the two ledgers. Therefore, **take extreme care when joining a channel to know the database type supported by the channel**. It might be necessary to create a new peer. Note that the database type cannot be changed after a peer has been deployed.
{:important}

## Adding and removing ordering service nodes
{: #ibp-console-govern-components-add-remove-orderer}

Because ordering service nodes can only belong to a single ordering service, if you create an ordering service node from the main **Nodes** panel, you will not be able to add it to an existing ordering service. If you want to add a node to an existing ordering service, the node must be created specifically for that purpose using the process described below.
{:important}

Use case:

There are a few reasons why a user might decide to add or delete ordering service nodes. For example, a corrupted node might need to be deleted and replaced with a new node, or a new organization joining a network might choose to contribute a node to the ordering service to share in the management and costs associated with the ordering service. Whatever the reason, the first step is to click on the ordering service the node is being added to in the **Nodes** panel. Note that before you add a node or remove a node from an ordering service, you must complete these steps:

1. Create a CA. This process is identical to the process described in [Creating your ordering service organization CA](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-build-network#ibp-console-build-network-create-orderer-ca) from the Build a network tutorial. If you have already deployed your own ordering service, you may use the CA you created as part of that process to create this node.
2. Use this CA to create an MSP representing your ordering organization, as well as an admin identity for this organization. As with the CA, you may reuse the MSP and identities you created when creating this node. However, you also have the option to create a dedicated CA, MSP, and identities if you want.
3. If the ordering service was created in a different console, you must export your MSP to the console where the ordering service was created. The administrator of that console must then add the MSP as one of the administrators of the system channel by clicking on the tile for the ordering service and then clicking **Add ordering service administrator**.
4. Similarly, if the ordering service was created in a different console, you must **import** the JSON representing ordering service into your console. Note that if you imported this JSON before September 18, 2019, you will need to re-import it, as the JSON now contains information necessary to add the node and add it to the consenter set of the system channel.

After these steps have been completed, you can delete a node by clicking on the node and then clicking on the "delete" icon (which resembles a trash can). Note that if the node was not created in your console, you can only remove it from your console. A node can only be deleted from the console where it was created.

To add a node, click the **Add another node** tile. This will open a series of panels similar to the process for creating an ordering service. You will need to:

* **Give the node a display name**. A best practice will be to give the node a display name that matches the pattern used for the other nodes in the ordering service.
* **Select a CA**. You may reuse the CA you used to create your own ordering service (if any). If you created the ordering service you are adding to, it is recommended to use the same CA. Do not reuse a CA you have used to create a peer.
* **Enter an enroll ID and secret**. Again, if you created an enroll ID and secret for your existing ordering nodes, you may use the same enroll ID and secret here.
* **Select an MSP**. If you are adding to an ordering service you created in your console, reuse the MSP you used when creating that ordering service. If the new node is being added to an ordering service created elsewhere, use the MSP you exported to that console.
* **Enter a TLS enroll ID and secret**. This should be same enroll ID and secret you entered for the node itself.
* **Allocate resources**. If the original set of ordering nodes was created using a custom allocation, it is a best practice to mimic that allocation when adding new nodes.

After reviewing the **Summary** page, click **Add another node**. This will submit the creation request. To complete the process of creating the node, you need to add it to the consenter set of the system channel. For information about how to do that, proceed to the next section.

### Adding the node to the consenter set
{: #ibp-console-govern-components-add-remove-orderer-consenter-system-channel}

After the ordering node has been successfully added, you will see a "Pending" tile on the ordering service. This pending state reflects the fact that, while the node creation process has been successful, the node is not yet part of the consenter set of the system channel.

Recall that the "consenter set" refers to the ordering service nodes actively participating in the ordering process on a channel, while the "system channel", which is managed by the ordering service, is the foundation of a network and forms the template for application channels.
{:tip}

To add the node you created to the system channel, click on the node. You will see a **Add to system channel** button. Click this button. Then, on the panel, click **Add to system channel**. Because your MSP has already been added to the system channel, you have the permission to update the channel to add your node. After this process has completed, the Pending tile should be disappear and your node should now be part of the system channel.

Note that if you want to add this node to any existing application channels, you will have to add them to consenter set of each of those channels. For information about how to do that, see [Adding and removing ordering service consenters](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-govern#ibp-console-govern-consenters).

Now that the ordering node has been added to the consenter set of the system channel, we need to perform one additional step.

### Export the node to the console where the ordering service was created
{: #ibp-console-govern-components-export-back-to-original-console}

After the node has successfully been created and added to the system channel, export a JSON representing the node back to the console where the ordering service was created.

To do this, click on the **Export** button and send the JSON to the administrator of the console out of band.

### Suggested configurations of ordering nodes
{: #ibp-console-govern-components-suggested-ordering-node-configurations}

While it's possible to use the console to build a configuration of any number of ordering nodes (no configuration is explicitly restricted), some numbers provide a better balance between cost and performance than others. The reason for this lies in satisfying the needs of high availability (HA) and in understanding the Raft concept of the "quorum", the minimum number of nodes that must be available (out of the total number) for the ordering service to process transactions.

In Raft, a **majority of the total number of nodes** is needed to form a quorum. In other words, if you have one node, you need that node available to have a quorum, because the majority of one is one. Similarly, if you have two nodes, you will need both available, since the majority of two is two (for this reason, a configuration of two nodes is discouraged; there is no advantage to a two node configuration). In a similar vein, the majority of three is two, the majority of four is three, the majority of five is three, and so on.

While satisfying the quorum will make sure the ordering service is functioning, production networks also have to think about deployment configurations that are highly available. In other words, configurations in which the loss of a certain number of nodes can be tolerated by the system. Typically, this means surviving two nodes being down: one node going down during a normal maintenance cycle, and another going down for any other reason (such as a power outage or error).

This is why, by default, the console offers two options: one node or five nodes. Recall that the majority of five is three. This means that in a five node configuration, the loss of two nodes can be tolerated. If your configuration features four nodes, only one node can be down for any reason before **another** node going down means a quorum has been lost and the ordering service will stop processing transactions.

For this reason, it is considered a best practice to have an odd number of nodes in an ordering service. There is nothing wrong with an even number of nodes, but they add costs without making the ordering service more highly available.
