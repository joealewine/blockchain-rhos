---

copyright:
years: 2018, 2019
lastupdated: "2019-10-02"

keywords: IBM Blockchain Platform, system requirements, Kubernetes, behind a firewall

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

# About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0
{: #console-ocp-about}

The {{site.data.keyword.blockchainfull}} Platform v2.1.0 enables a consortium of organizations to easily build and join a blockchain network on-premises, or on any private, public, or hybrid multicloud using Red Hat OpenShift. Customers can deploy their nodes on the cloud platform of their choice and connect to any {{site.data.keyword.blockchainfull_notm}} Platform network, whether it is deployed on OpenShift or with the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 utilizes Hyperledger Fabric v1.4.3 and supports deployment on the OpenShift Container Platform 3.11.
{:shortdesc}

## Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.0) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Any x86_64 Red Hat OpenShift Container Platform 3.11 Kubernetes cluster -– private, public or hybrid multicloud | An {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}} |  
| How is it billed? |Contact us for pricing |[$0.29 USD per allocated CPU hour](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be on-premises and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes|
| Where can I learn more? |See [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-offers)  | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |

## What {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 offers
{: #console-ocp-about-offers}

The {{site.data.keyword.blockchainfull}} Platform provides a flexible management platform that runs on Red Hat OpenShift Container Platform 3.11 clusters. The offering includes an award-winning user interface that allows you to easily deploy blockchain components, build a multicloud blockchain network, and perform network management and maintenance. For more information about the OpenShift Container Platform, see the documentation for [OpenShift 3.11](https://docs.openshift.com/container-platform/3.11/welcome/index.html){: external}.

The {{site.data.keyword.blockchainfull}} Platform console UI can be used to create all of the fundamental components of a Hyperledger Fabric blockchain: a Certificate Authority, an ordering service, and peers, on your local cluster. You can also use your console to operate a distributed multicloud network by importing nodes deployed by other consoles. For more information about the building blocks of Hyperledger Fabric networks, see the [blockchain component overview](/docs/services/blockchain-rhos?topic=blockchain-blockchain-component-overview#blockchain-component-overview).

This {{site.data.keyword.blockchainfull_notm}} Platform release includes the following key features:

**BUILD ---- Integrated developer experience**
- **Easily code** your smart contracts in Node.js, Golang, or Java, write client applications using the new {{site.data.keyword.blockchainfull_notm}} VS Code extension, leverage **SDK integration** with the user interface console, and learn from our rich tutorials and samples.
- **Simplified DevOps** allows you to move from development to test to production in a single environment by scaling up your Kubernetes resources to add more components.
- **Kubernetes service integration.** Leverage services such as Grafana and Prometheus for logging and Kibana for monitoring.
- **Up-to-date Fabric key features**. Leverage the latest features of Hyperledger Fabric v1.4.3:
  - [Raft ordering service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Private data collections](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) that provide increased data privacy by ensuring that ledger data is shared to only authorized peers via the gossip protocol.
  - [Service discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, allowing you to dynamically discover and update how your application interacts with your network.
  - [Channel access control lists](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} that allow you additional control the governance of your channels and smart contracts.

**OPERATE --- Total control of your deployments**
- **Deploy only the components you need**. Connect a peer to multiple channels and networks, or host an ordering service that business partners can connect to.
- **Maintain complete control of your identities**. Store and manage the keys that are used to administer your nodes in your own secure environment.
- **Unified operation**. The {{site.data.keyword.blockchainfull_notm}} Platform console allows you to deploy and manage all of your organizations and nodes in **one console** without having to rely on {{site.data.keyword.IBM_notm}} or other vendors to manage your ordering nodes or Certificate Authority. You can also add or remove members from a blockchain consortium, create and join channels, and install and instantiate smart contracts from your console.
- **Host or join a network**. Deploy peers that are hosted in your cluster to multiple channels on multiple clouds, or invite other organizations to join your consortium or channels while the organizations manage their nodes independently across infrastructures.
- **Manage access** of the users who can administer or monitor your nodes.
- **Direct access to the logs** of your nodes from your Kubernetes service. Use any supported third-party service to extract and analyze your logs.
- **Interact directly with your pods** using your Kubernetes service.
- **Dynamic signature collection** that allows better control over collaborative governance over channel configurations.

**GROW --- Scalability and flexibility**
- **Choose your compute**. You have the flexibility to decide the amount of CPU, memory, and storage you want to provision in your Kubernetes cluster. For more information, see [How the console interacts with your Kubernetes cluster](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Scale** up and down the resources in your Kubernetes cluster, paying for only what you need. For more information, see [Pricing](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-rhos-pricing).
- **Disaster recovery and multizone high availability.** This ability duplicates your Kubernetes deployment across zones, enabling high availability (HA) of your components and disaster recovery (DR).
- **Run Anywhere**. Thanks to the **unified codebase** of the {{site.data.keyword.blockchainfull_notm}} Platform console, it is possible to run your components on any environment supported by the OpenShift Container Platform.

## Considerations and limitations
{: #console-ocp-about-considerations}

Users of this offering must manage their own security and infrastructure. The {{site.data.keyword.blockchainfull_notm}} Platform does not provision or provide those services. Review these Considerations and limitations before you begin:

- This offering does not include an entitlement to Red Hat OpenShift.
- {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 is only supported on OpenShift Container Platform 3.11, including OpenShift Dedicated. OpenShift version 4.1 and versions before 3.11 are not supported. Also, Red Hat OpenShift Online is not supported.
- Persistent storage is required, host-local storage volumes are not supported.  
- You must have the cluster admin role in order to deploy the product.
- The console creates nodes based on the Hyperledger Fabric v1.4.3 node images.
- You can deploy only one {{site.data.keyword.blockchainfull_notm}} Platform console per OpenShift project and Kubernetes namespace. If you plan to create multiple blockchain networks, for example to create different environments for development, staging, and production, you should create a unique project for each environment.
- You can only import nodes that are exported from other {{site.data.keyword.blockchainfull_notm}} Platform consoles. In order to be able to operate an imported peer or ordering node from the console, you also need to import the associated node's organization MSP definition and administrator identity into your console.
- {{site.data.keyword.blockchainfull_notm}} Platform is not supported on OpenShift Online.
- There is no free trial at this time. Customers who are interesting in exploring the functionality should try the offering on [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-overview){: external}.
- You are responsible for the management of health monitoring, security, logging, and resource usage of your blockchain components.

## System prerequisites
{: #console-ocp-about-prerequisites}

The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 can be deployed on any x86_64 architecture that is supported by the Red Hat OpenShift Container Platform 3.11 and meets the [system and environment requirements](https://docs.openshift.com/container-platform/3.11/install/prerequisites.html){: external}. See this article on [OpenShift Container Platform 3.x Tested Integrations](https://access.redhat.com/articles/2176281){: external}. Scroll down to the bottom of the article to the section titled 'Tested Platforms' to view the list of cloud providers tested on OpenShift Container Platform 3.11.
The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 is not supported on Linux on Power (ppc64le) POWER8 systems, LinuxONE, or {{site.data.keyword.IBM_notm}} Z.

## License and pricing
{: #console-ocp-about-license}

{{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 is purchased as a stand-alone entitlement. After purchasing, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release. Note that if you choose this option you are responsible for provisioning your own Kubernetes cluster compatible with the OpenShift Container Platform 3.11.

For more information, see [Pricing](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-rhos-pricing).

## Installing {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0
{: #console-ocp-about-install}

The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 uses a [Kubernetes Operator](https://www.openshift.com/learn/topics/operators){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment of your nodes. When you purchase the {{site.data.keyword.blockchainfull_notm}} Platform license from Passport Advantage Online, you receive a token that provides you access to {{site.data.keyword.IBM_notm}} Entitlement registry. You can use your token with the commands and files that are provided in the installation guide to automatically download the Docker images and start the operator and console on your cluster. When you are ready to get started, got to [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/services/blockchain-rhos?topic=blockchain-rhos-deploy-ocp).

You can also deploy the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall, without having access to the public internet.

## Security Considerations
{: #console-ocp-about-security}

Because these components are deployed on your own infrastructure, you are responsible for managing their security. This includes important areas of security, such as Identity and Access Management, key management, and data encryption. Review the following topic on [Security](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-security) for the list of considerations.

## Getting support
{: #console-ocp-about-support}

For more information about how to get support on {{site.data.keyword.blockchainfull_notm}} Platform, in addition to free blockchain developer resources and support forums that you can use to troubleshoot problems, see [Getting support](/docs/services/blockchain-rhos?topic=blockchain-rhos-blockchain-support#blockchain-support).
