---

copyright:
  years: 2019
lastupdated: "2019-10-30"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# Importing nodes
{: #ibp-console-import-nodes}

The console includes the option to import nodes that were created in another {{site.data.keyword.blockchainfull}} Platform console.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing the blockchain network.

## Why import a node?
{: #ibp-console-import-nodes-why}

For cases when components have been deployed by one console and need to be operated from other, as well as for cases when certain actions are not possible unless nodes and MSPs and identities are "known" to a console (that is, unless those components have been created in the console or imported into it), the {{site.data.keyword.blockchainfull_notm}} Platform allows nodes, identities, and MSPs to be exported from one console and imported into another.

There are two main reasons to import components:

  1. A node will be deployed in one console and operated from another. **This will be most common for CAs and peers**. In these cases, **the relevant admin identity must be exported and imported, or a new admin identity for the node must be created**.
  2. A node or MSP simply has to be known by a console so that it can be selected from a drop down list. **This will be most common for ordering services**, though in the case of the MSP it will also be important when adding organizations to a channel. It will not typically be necessary to import an admin identity in this case, though in the case of the ordering service, it can be useful to export an MSP to the console where the ordering service was created. More on this later.

Among the functionalities that become available after components have been imported include:

- Add new organizations to the consortium
- Create new channels
- Join peers to channels
- Install smart contracts on peers regardless of where they were deployed
- Instantiate smart contracts on channels
- Upgrade smart contracts on channels

Additional steps might be required after a node has been imported before some of these tasks can be performed.
{: note}

Note that when you import a component, you do not actually import the physical component into your cloud provider. Instead, the console uses the information in the JSON to build a representation of the component that can be operated from the console. Likewise, when you delete an imported node from the console, the node itself, which is still running at the location where it was deployed, is not deleted. It is simply removed from the console where it was imported.

After you import a node into the console, you can also modify its connection information by using the node's **Settings** tab.



## Limitations
{: #ibp-console-import-limitations}

While importing nodes provides the ability to perform many of the actions that can be performed on the console where a node or identity was created, there are a few limitations on the ability to administer imported components:

- You cannot import nodes from Starter plan or Enterprise plan networks.
- All nodes to be imported must have been deployed by using the {{site.data.keyword.blockchainfull_notm}} Platform console or [{{site.data.keyword.blockchainfull_notm}} APIs](https://cloud.ibm.com/apidocs/blockchain){: external}.
- You cannot patch nodes that have been imported into the console.
- You cannot delete nodes that you imported into the console from the cluster where they were deployed. You can only remove the node from the console it was imported to.
- If you are importing a node that is deployed on a network deployed locally, you must ensure that the gRPC web proxy port used by the component is externally exposed to the console. For more information, see [Importing nodes from a locally deployed network](#ibp-console-import-icp).
- When you open the tile of an imported node, the Fabric version is not visible and the **Usage and info** tab is not available, even if you have also imported the admin identity.



## Gathering certificates or credentials
{: #ibp-console-import-start-here}

Because identities contain private keys, be careful when exporting them to ensure they are handled securely. If a private key is compromised, it can be used to perform malicious actions.
{: important}

Each {{site.data.keyword.blockchainfull_notm}} Platform component is deployed with the signing certificate of an administrator inside. When actions requiring the permission level of an admin are performed against the component, the singing certificate of the entity attempting the action is checked against the signing certificate inside the node. If they don't match, the action is denied. In this way, these certificates, which are also known as "keys", allow the administrator to operate their components.

If you intend to operate an imported node, you have two options:

1. Import the admin identity into your wallet before importing the node itself (the administrator of the console where the node was created will need to export the node).
2. If the admin of the node does not want to share their admin identity, there are two other ways to become an admin of the node:
     * The administrator of the console where the node was created can create a new admin identity, associate it with the node as an additional admin identity, and export that identity to your console along with the node.
     * The other option has five steps:
       1. The admin of the CA that was used to create the relevant identities for the peer and peer organization can register a new identity.
       2. Then, they can send the enroll ID and secret of that identity to the importing console, as well as exporting the CA.
       3. The importing console can then take the imported CA and use the enroll ID and secret to create a new CA admin identity.
       4. This new CA admin can then create a new admin identity for the peer. This new peer admin identity would be sent back to the console where the peer was created and added as an admin of the peer.
       5. Then the peer could be exported to the console where the new identity was created, allowing the new identity to operate the imported peer.

In either case, because this new admin identity would only be an admin of the peer and not the organization that owns the peer, it preserves a separation of roles between the organization admin (the sign cert listed in the MSP, which might also be the peer admin in the console where the peer was created), and the new admin of the peer which was just created and associated. This peer admin would not be an organization admin by default. However, if it fits a use case to make an identity the admin of both the organization and an imported peer, this is achievable, see [Updating an organization MSP definition](docs/services/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-govern-update-msp).

Note that this flow would be most practical for a peer, since the admins of ordering nodes do not have many responsibilities in the {{site.data.keyword.blockchainfull_notm}} Platform.

If you don't intend to operate the node (or in the case of a multi-node ordering service, the nodes), you do not have to import and associate an admin identity. Note that you will still be required to associate **some** identity with the node during the import process. In this case, choose any identity that you have in your wallet. If this associated identity is not an admin of the node, you will not be able to perform privileged actions on the node. However, because there is little that can be achieved with the node without admin status, importing peers and CAs without associating a relevant admin identity would be a waste of time. The same is not true of ordering nodes and the ordering service, as we will discuss later.

### Exporting and importing admin identities into the wallet
{: #ibp-console-import-nodes-admin-identities}

To export an identity, open the **Wallet** tab and click on the identity you want to export. In the **Identity details** panel that pops up, you have two options. You can click the **Export identity** button, which will bundle your signing certificate and private key into a JSON file and download it to your local filesystem (note that you should always keep a copy of all your identities locally) or you can download `.pem` certificates for both your signing certificate and private keys.

To import a new identity, open the **Wallet** tab and click **Add identity**. If you want to upload `.pem` certificates manually, click **New identity** and use the fields under **Manual entry** to give the identity a name and to upload the signing certificate and private key of the identity. If you have a JSON that was exported from another console, click **Upload JSON** and then **Add files**, then select the appropriate JSON from your local file system and click **Add identity**. While you will have this option between entering certificates manually or using a JSON for each node, in this topic it is assumed that you have the relevant JSON handy.

After you complete the **Add identity** panel, you can view the new admin identity in your wallet and select to this identity when importing any nodes it is associated with. You will also be able to use this identity when creating new MSPs and components.

### Importing an organization MSP definition
{: #ibp-console-import-msp}

Because MSPs define an organization, they are used when creating channels, creating nodes (to identify which organization the node will belong to), and validating signatures. For the latter reason, your MSP must be known (in other words, exported and then imported) to every member of every channel you belong to unless your organization will not be involved in validation or endorsement.

To act as an administrator for an organization, you must have an identity in your wallet listed in the MSP as an admin of the organization. This means either importing the identity listed in the MSP or by following the process listed in [Updating an organization MSP definition](docs/services/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-govern-update-msp). This MSP can then be exported to the console where the added identity was created, making the exported identity able to act as an admin of the organization.

The exports and imports of MSPs are performed in the **Organizations** tab. To export an MSP, click on it, then click on the export button. To import an MSP, click **Import MSP definition**. If you have a certificate of the MSP admin in your wallet, check the checkbox `I have an administrator identity for the MSP definition`. If you do not check this box and subsequently try to create a peer or ordering service, this organization MSP definition will not be listed in the MSP drop-down list, even if you have an admin certificate in your wallet.

## Importing a peer
{: #ibp-console-import-peer}

A peer node is the blockchain component that maintains a ledger and runs a smart contract to perform query and update operations on the ledger. Organization members own and maintain peers. Each organization that joins a consortium should deploy at least one peer and minimally two for High Availability (HA) on each channel. You can learn more about peers in the [overview of blockchain components](/docs/services/blockchain-rhos?topic=blockchain-rhos-blockchain-component-overview#blockchain-component-overview-peer).

After you import a peer into the console, you can install smart contracts on the peer and join the peer to channels.

**Note:** If you want to add more peers or create additional admin identities for a peer or peer organization, you need to import the relevant CA (the one that was used to create the MSP and identities for the peer organization) and then use that CA to register and enroll identities.

### Before you begin
{: #ibp-console-import-peer-before-you-begin}

Before you can import a peer, you need to decide on the method you will use to become an admin of the peer (there is little that can be accomplished with an imported peer unless you also are an admin of the peer). See, [Gathering certificates or credentials](#ibp-console-import-start-here) and follow the path that best suits your use case. Also, ensure that the JSON representing the peer is available.

### How to import a peer
{: #ibp-console-import-peer-howto}

Importing a peer is performed from the **Nodes** tab.

1. Click **Add Peer**, followed by **Import an existing peer** and then click **Next**.
2. Click **Add file** to upload the peer JSON file that was exported from the console where it was originally deployed.
3. Set the admin identity for the peer by clicking **Existing identity** and selecting the peer admin identity that you imported into your wallet.



After you import the peer into the console, you can install smart contracts on the peer and join the peer to channels in your blockchain.

## Importing a CA
{: #ibp-console-import-ca}

A CA node is the blockchain component that issues certificates to all network entities (peers, ordering services, clients, and so on) so that these entities can communicate, authenticate, and ultimately transact. Each organization has their own CA that acts as their root of trust. You should add your organizations whether you are joining or building a blockchain consortium. You can learn more about CAs in the [overview of blockchain components](/docs/services/blockchain-rhos?topic=blockchain-rhos-blockchain-component-overview#blockchain-component-overview-ca).

After you import a CA, you can use it to register and enroll users or create organization definitions exactly as you would with any other CA.

To import a CA to the {{site.data.keyword.blockchainfull_notm}} Platform console and operate it, the network operator must have already exported the CA from the {{site.data.keyword.blockchainfull_notm}} Platform where it was deployed. Importing a CA allows you to register new users and [enroll identities](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-identities#ibp-console-identities-enroll).

### Before you begin
{: #ibp-console-import-ca-before-you-begin}

Before you can import a CA, you need to decide on the method you will use to become an admin of the CA (there is little that can be accomplished with an imported CA unless you also are an admin of the CA). See, [Gathering certificates or credentials](#ibp-console-import-start-here) and follow the path that best suits your use case. Also, ensure that the JSON representing the CA is available.

### How to import a CA
{: #ibp-console-import-nodes-howto-ca}

Navigate to the **Nodes** tab.

1. Click **Add Certificate Authority**, followed by **Import an existing Certificate Authority**, and click **Next**.
2. Click **Add file** to upload the CA JSON file that was exported from the console where it was originally deployed.
3. After the CA has been added, click on it in the **Nodes** panel. Then click **Associate identity** and select the CA admin identity from your wallet.



After you have imported the CA into the console, you can use your CA to create new identities and generate the necessary certificates to operate your components and submit transactions to the network. To learn more, see [Managing certificate authorities](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-identities#ibp-console-identities-manage-ca).

## Importing an ordering service
{: #ibp-console-import-orderer}

An ordering service is the blockchain component that collects transactions from network members, orders the transactions, and bundles them into blocks. It is the common binding of blockchain consortiums and needs to be deployed if you are founding a consortium that other organizations will join. You can learn more about ordering services in the [overview of blockchain components](/docs/services/blockchain-rhos?topic=blockchain-rhos-blockchain-component-overview#blockchain-component-overview-orderer).

Importing an ordering service into the console allows you to create new channels for peers to transact privately.

### Before you begin
{: #ibp-console-import-orderer-before-you-begin}

Even in the world of Hyperledger Fabric, ordering node admins do not have many responsibilities. In the {{site.data.keyword.blockchainfull_notm}} Platform, these responsibilities are reduced, and because of the [Limitations](#ibp-console-import-limitations) imposed on imported nodes, these responsibilities are reduced to nothing. As a result, there is not much point in becoming an additional admin of an ordering node. Despite this, the console will ask you to associate an identity. Feel free to associate any identity in your wallet.

Far more useful than becoming the admin of an ordering node is becoming an admin of the ordering organization itself. This role gives you administrative rights over the ordering service (allowing you to add organizations to the consortium or nodes to the ordering service) as well as over the system channel, giving you the permission to modify system channel [capabilities](docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-update-channel-available-parameters-advanced).

Note that it is not necessary for your organization to become an ordering service administrator. You will still need to import the ordering service if you want to create or edit a channel (your organization must be added to the consortium by an ordering service administrator first), as your console must know about the ordering service before you can select it from the drop down list.

For information about how to add your organization to the list of ordering service administrator organizations, proceed to the next section.

### How to import an ordering service
{: #ibp-console-import-orderer-process}



Navigate to the **Nodes** tab.

1. If you have not already done so, navigate to the **Organizations** tab and export the MSP of one or more peer organizations. Then, send this MSP, which represents your organization, to an administrator of the ordering service (which in this case is not necessarily an admin of any ordering nodes, but an organization with administrative control over the system channel). This ordering service administrator can make your organization one of the ordering service organizational admins or add your MSP to the consortium, giving your organization the ability to create channels, or both. Note that if your organization has been made one of the ordering service organizational admins, you have the ability to add your own organization to the consortium.
2. After your MSP has been added as an ordering service administrator or to the consortium (or both), import the JSON representing the ordering service (as with the other nodes, this ordering service must be exported from the console where the ordering service was created). Then, navigate to the **Nodes** panel. Click **Add ordering service**, followed by **Import an existing ordering service**. Then click **Next**.
3. Click **Add file** to upload the ordering service JSON. Note that regardless of how many nodes are in this ordering service, the JSON representing this ordering service will be one file.
4. Set the admin identity for the ordering nodes by clicking **Existing identity**. Recall from the [Before you begin section](#ibp-console-import-orderer-before-you-begin) that you can effectively skip this step if you want by selecting any identity in your wallet. However, if you want to have the correct node admin identity exported from the console where it was created and imported into your console and select it in this step, you have the ability to do that.



After you have imported the ordering service into the console, you can add new organization members to the consortium (if your MSP was added as an admin of the ordering service) and select the ordering service when creating new channels (if your organization has been added to the consortium).

## Importing nodes from a locally deployed network
{: #ibp-console-import-icp}

You can import nodes that were created through Red Hat OpenShift or {{site.data.keyword.cloud_notm}} Private into consoles that have been deployed on other clusters or on {{site.data.keyword.cloud_notm}}. However, you need to ensure that the port used by the gRPC URL of your nodes are exposed from outside the cluster. If you are deploying your network behind a firewall, you need to enable a passthru, for example by using white listing, to allow the console outside the cluster to communicate with your nodes.

As an example, you can find the JSON file of a peer below. To communicate with the peer from another console, you need to ensure that the `grpcwp_url` port, port 32403 in this example, is open to external traffic.

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\ensure that port 32403 is externally exposed
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```

