---

copyright:
  years: 2019
lastupdated: "2019-09-24"

keywords: pricing model, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

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

# Pricing
{: #ibp-rhos-pricing}

This guide helps you understand the pricing model for {{site.data.keyword.blockchainfull}} Platform v2.1.0, and how much you will pay when you develop and grow your blockchain network of peers, ordering service, and Certificate Authorities components, which are based on Hyperledger Fabric v1.4.3.
{:shortdesc}

## License
{: #ibp-software-pricing-license}

{{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 delivers the components that are needed to run a blockchain network on your own infrastructure and is deployed on Red Hat OpenShift Container Platform.  You can access your [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering which is required in order to deploy the release. With this offering, you are responsible for procuring and provisioning your own Kubernetes cluster on Red Hat OpenShift Container Platform 3.11. Technical support from IBM Blockchain is included in the offering.

## Pricing
{: #ibp-software-pricing-pricing}

The pricing of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 is based on the number of Virtual Processor Cores (VPCs) used. A VPC, or vCPU, can be either a virtual core that is assigned to a virtual server, or a physical processor core in a non-partitioned server. You must obtain a licensed entitlement for each VPC made available to the {{site.data.keyword.blockchainfull_notm}} Platform. The number of vCPU or CPUs that are required can vary depending on your infrastructure, network design and performance requirements.

For more information about VPCs (CPUs) in Red Hat OpenShift, see this topic on [Allocating Node Resources](https://docs.openshift.com/container-platform/3.11/admin_guide/allocating_node_resources.html){: external}.
Note that 1 `core` in OpenShift is equivalent to 1 vCPU in {{site.data.keyword.blockchainfull_notm}} Platform.

Various licensing options are available. For more information, please [Contact us](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.
