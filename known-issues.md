---

copyright:
  years: 2019
lastupdated: "2019-12-13"

keywords: catalina, chrome

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

# Known issues
{: #sw-known-issues}

## Chrome browser on Mac OS Catalina
{: #sw-known-issues-catalina}

The Chrome browser is currently not supported on Mac OS Catalina when the {{site.data.keyword.blockchainfull_notm}} Platform  v2.1.x is deployed with the default configuration that uses self-signed certificates. To avoid this problem, use a different [supported browser](docs/blockchain-rhos?topic=blockchain-rhos-deploy-ocp#deploy-ocp-browsers) with Catalina, or use your own [TLS certificates when deploying on OpenShift Contain Platform](/docs/blockchain-rhos?topic=blockchain-rhos-deploy-ocp#use-your-own-tls-certificates-optional-) or [TLS certificates when deploying on Kubernetes or {{site.data.keyword.cloud_notm}} Private](https://test.cloud.ibm.com/docs/services/blockchain-rhos?topic=blockchain-rhos-deploy-k8#use-your-own-tls-certificates-optional-).
