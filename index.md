---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-24"

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

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0
{: #get-started-console-ocp}

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components on any x86_64 architecture [supported by the Red Hat OpenShift Container Platform 3.11](https://docs.openshift.com/container-platform/3.11/install/prerequisites.html){: external}. For a list of platforms that are tested with OpenShift Container Platform 3.11, see [Tested Integrations](https://access.redhat.com/articles/2176281){: external}. Users can create components in their clusters and connect them to components deployed on other clusters, forming a distributed, multi-organizational blockchain network, by installing the {{site.data.keyword.blockchainfull_notm}} Platform console.
{:shortdesc}

**{{site.data.keyword.blockchainfull_notm}} Platform v2.1.0** is purchased as a stand-alone entitlement. After purchasing, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release.

With this option, you are responsible for provisioning your own Kubernetes cluster compatible with the OpenShift Container Platform 3.11.
{: important}

To learn more about what the product offers, see [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about). For more information about pricing, see [Pricing](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-rhos-pricing).

## Step one: Set up OpenShift Container Platform 3.11
{: #get-started-console-ocp-set-up-ocp}

The {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 can be installed only on the [OpenShift Container Platform 3.11](https://docs.openshift.com/container-platform/3.11/welcome/index.html). For more information on the cloud platforms that you can use, see [System prerequisites](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-prerequisites).

## Step two: Install the {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-ocp-step-two-deploy-console}

The {{site.data.keyword.blockchainfull_notm}} Platform uses a [Kubernetes Operator](https://www.openshift.com/learn/topics/operators){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment and your blockchain nodes. A cluster administrator can use your entitlement key to deploy the platform by following the steps described in the [Deploying {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp) topic:

  1. [Log in to your OpenShift cluster](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-login).
  2. [Create a new project](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-project).
  3. [Add security and access policies](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-scc).
  4. [Create a secret for your entitlement key](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-docker-registry-secret).
  5. [Deploy the {{site.data.keyword.blockchainfull_notm}} Platform operator](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-operator).
  6. [Deploy the {{site.data.keyword.blockchainfull_notm}} Platform console](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-console).
  7. [Log in to the console](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-log-in).

## Step three: Grant console access to other users
{: #get-started-console-ocp-step-four-add-console-admin}

The console administrator can log in to the console with the email address and password that was provided during console deployment. The password that was provided becomes the default password of the console, and is used by all new users to log in to the console for the first time. The administrator can then add new users to the console, allowing others to log in and start working with {{site.data.keyword.blockchainfull_notm}} nodes. The administrator can also set a new default password. To learn more, see [Managing users from the console](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-icp-manage#console-icp-manage-users).

## Step four: Use the console to create your components
{: #get-started-console-ocp-build-network}

After you deploy the console, you can use it to create, operate, and govern {{site.data.keyword.blockchainfull_notm}} components on your Kubernetes cluster. To get started with the console UI, go to the [Build a network tutorial](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-build-network#ibp-console-build-network).

## Step five: Connect networks across clouds
{: #get-started-console-ocp-import-nodes}

You can use your console to operate components that are running on other clusters. First, you need to export the component information to a JSON file from the console where the component was originally deployed. Then, you can import the node JSON file into the console that is deployed on your local cluster and manage the components across clouds. For more information, see [Importing nodes](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-ibp-console-import-nodes#ibp-console-import-nodes).
