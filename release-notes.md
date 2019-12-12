---

copyright:
  years: 2019
lastupdated: "2019-12-12"


keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain-rhos

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# Release notes
{: #release-notes-saas-20}

Use these release notes that are grouped by date to learn about the latest changes to {{site.data.keyword.blockchainfull}} Platform v2.1
{:shortdesc}

## 17 December 2019
{: #12-17-2019}

### Release of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2
{: #12-17-2019-212}

#### Hyperledger Fabric v1.4.4
{: #12-17-2019-hlf144}

All new nodes are deployed using Hyperledger Fabric v1.4.4.

#### Support for OpenShift Container Platform (OCP) 4.1 and 4.2
{: #12-17-2019-ocp4.x}

In addition to supporting deployment on OCP 3.11, you can now also deploy the platform on OCP 4.1 and 4.2.

#### Simplified component creation flows
{: #12-17-2019-flow}

Peer, CA, and ordering service creation has been streamlined. Now, when you create a node, you can select a checkbox to configure advanced deployment options.

#### Zone selection for ordering nodes
{: #12-17-2019-oszone}

If your Kubernetes cluster is configured across multiple zones, you now have the ability to select which zone an ordering node is deployed to.

#### Add peer to a channel from Channels tab
{: #12-17-2019-add-peer}

In addition to joining a channel from the peer node panel, a peer can now be joined to a channel directly from the Channels tab.

#### Anchor peer during join
{: #12-17-2019-add-anchor-peer}

When a peer is joined to a channel, you can now designate that peer as an anchor peer which bootstraps automatic communication between organizations. Anchor peers on your channel are a requirement if your client application uses Service Discovery or if your smart contract uses private data collections.

#### Export/Import all
{: #12-17-2019-export-import}

Allows for the bulk export and import of nodes, MSPs, and identities, rather than having to export and import these components one at a time. See this topic on [Exporting and importing in bulk](/docs/blockchain-rhos?topic=blockchain-rhos-ibp-console-import-nodes#ibp-console-import-bulk-export-import) for more information.

#### Support for Fabric Node OU features
{: #12-17-2019-nodeOU}

In addition to the existing `client` and `peer` Node OU types that are available when you deploy a peer or ordering node, you now have two additional Node OU types to choose from: `admin` and `orderer`. Follow the [Build a network tutorial](/docs/blockchain-rhos?topic=blockchain-rhos-ibp-console-build-network) for examples of which type to use when you deploy your nodes.

## 8 November 2019
{: #11-08-2019}


### Release of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1


#### {{site.data.keyword.blockchainfull_notm}} Platform images
{: #11-08-2019-images}


A purchase of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 now includes an entitlement to peer, CA, ordering service, and smart contract container images that are signed and supported by {{site.data.keyword.IBM_notm}}. The images are based on the open source Fabric code base and contain a number of enhancements for stability and serviceability.

## 24 September 2019
{: #09-24-2019}

### Release of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0
{: #09-24-2019-210}

{{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 allows you to deploy the platform on the Red Hat OpenShift Container Platform. {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 can be installed on any x86_64 architecture where Red Hat OpenShift Container Platform 3.11 runs and includes the console UI.
