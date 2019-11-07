---

copyright:
  years: 2019
lastupdated: "2019-11-07"

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

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1
{: #get-started-console-ocp}

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components on any x86_64 Kubernetes v1.11 or higher container platform.  This offering is ideal for the customers who want to deploy their components, store their data, or run their workloads on their own infrastructure or across public and private clouds for security, risk mitigation, preference, or compliance reasons.
{:shortdesc}

Your entitlement includes the flexible management console for deploying and managing your blockchain network. Use the console or {{site.data.keyword.blockchainfull_notm}} Platform APIs to build a consortium of organizations to easily transact on the same network, regardless of each client's cloud preference. Clients can build, operate, and grow their blockchain networks with an offering that can be used from development through production.  

Experienced Hyperledger Fabric customers who prefer to deploy and manage their containers can download and use the peer, CA, orderer, and smart contract container images without the management console.

For more information on where you can run {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1, see [Supported Platforms](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-prerequisites). Users can create components in their clusters and connect them to components deployed on other clusters, forming a distributed, multi-organizational blockchain network, by installing the {{site.data.keyword.blockchainfull_notm}} Platform console.

**{{site.data.keyword.blockchainfull_notm}} Platform v2.1.1** is purchased as a stand-alone entitlement. After purchasing, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release or download the container images.

With this offering, you are responsible for provisioning your own Kubernetes cluster.
{: important}

## Is {{site.data.keyword.blockchainfull_notm}} Platform 2.1.1 suitable for you?
{: #get-started-console-ocp-suitable}

## Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.1) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distrubtions on a private, public or hybrid multicloud | An {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}} |  
| How is it billed? |Contact us for pricing |[$0.29 USD per allocated CPU hour](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be on-premises and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes|
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | Yes | No |
| Where can I learn more? |See [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-offers)  | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |

### Developer Tools
{: ##get-started-console-ocp-dev-tools}

Developers can start with the [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain-rhos?topic=blockchain-rhos-develop-vscode#develop-vscode), a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly.

## Before you begin
{: #get-started-console-ocp-set-up-ocp}

1. The {{site.data.keyword.blockchainfull_notm}} Platform can be installed only on the [Supported Platforms](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-prerequisites).

2. You need to install a Kubernetes cluster on your Kubernetes container platform and login to your cluster. If you are using OpenShift, see the [OpenShift Container Platform CLI](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html#installing-the-cli){: external} to deploy the CLI. If you are using an OpenShift cluster that was deployed with the {{site.data.keyword.IBM_notm}} Kubernetes Service, use these instructions to [Install the OpenShift Origin CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).

3. If you are not running the platform on Red Hat OpenShift Container Platform, Red Hat Open Kubernetes Distribution, or {{site.data.keyword.cloud_notm}} Private then you need to setup the nginx ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.

4. You need get the entitlement key from your MyIBM account in order to install the platform. For more information, see [Get your entitlement key](/docs/services/blockchain-rhos?topic=blockchain-rhos-deploy-ocp#deploy-ocp-entitlement-key).

## Step one: Install the {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-ocp-step-two-deploy-console}

The {{site.data.keyword.blockchainfull_notm}} Platform uses a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment and your blockchain nodes. A cluster administrator can use your entitlement key to deploy the platform on any Kubernetes Cluster

-  If you are deploying the {{site.data.keyword.blockchainfull_notm}} Platform on the OpenShift Container Platform, you can deploy the platform by using the steps in the [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1 on the OpenShift Container Platform](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp).

- If you are deploying the {{site.data.keyword.blockchainfull_notm}} Platform on another Kubernetes distribution, such as Rancher or {{site.data.keyword.cloud_notm}} Private, use the steps described in the [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1 on Kubernetes](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-k8#deploy-k8).

## Step two: Grant console access to other users
{: #get-started-console-ocp-step-four-add-console-admin}

After the platform is deployed, the console administrator can log in to the console with the email address and password that was provided during console deployment. The password that was provided becomes the default password of the console, and is used by all new users to log in to the console for the first time. The administrator can then add new users to the console, allowing others to log in and start working with {{site.data.keyword.blockchainfull_notm}} nodes. The administrator can also set a new default password. To learn more, see [Managing users from the console](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-icp-manage#console-icp-manage-users).

## Step three: Use the console to create your components
{: #get-started-console-ocp-build-network}

After you deploy the console, you can use it to create, operate, and govern {{site.data.keyword.blockchainfull_notm}} components on your Kubernetes cluster. To get started with building a blockchain network by using the management console, go to the [Build a network tutorial](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-console-build-network#ibp-console-build-network).

## Step four: Connect networks across clouds
{: #get-started-console-ocp-import-nodes}

You can use your console to operate components that are running on other clusters. First, you need to export the component information to a JSON file from the console where the component was originally deployed. Then, you can import the node JSON file into the console that is deployed on your local cluster and manage the components across clouds. For more information, see [Importing nodes](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-console-import-nodes#ibp-console-import-nodes).
