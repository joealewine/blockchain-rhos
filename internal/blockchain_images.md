---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-26"

keywords: IBM Blockchain Platform, IBM Cloud Private, Data residency, world state

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Using the {{site.data.keyword.blockchainfull_notm}} images for Hyperledger Fabric
{: #blockchain-images}

{{site.data.keyword.blockchainfull}} provides Docker images for Hyperledger Fabric that are signed and supported by {{site.data.keyword.IBM_notm}}. The images are based on the open Fabric code base and contain a number of enhancements for stability and serviceability.

The {{site.data.keyword.blockchainfull_notm}} Fabric images can be purchased through the {{site.data.keyword.blockchainfull_notm}} 2.1.0 offering and are bundled with support from {{site.data.keyword.IBM_notm}}. {{site.data.keyword.blockchainfull_notm}} does not provide support for blockchain components deployed using the open source Hyperledger Fabric Docker images.

## Supported Platforms

The {{site.data.keyword.blockchainfull_notm}} images for Hyperledger Fabric must be deployed using Docker on x86 Hardware. The images have been tested on the following deployment platforms:

- {{site.data.keyword.IBM_notm}} Kubernetes service


## Supported Fabric versions

The {{site.data.keyword.blockchainfull_notm}} Docker images are based on Hyperledger Fabric v1.4.3. The Fabric images will be updated to support the latest version of Fabric used by the {{site.data.keyword.blockchainfull_notm}} Platform 2.1.0. Because, version 1.4 is the long term support release of Hyperledger Fabric, {{site.data.keyword.blockchainfull_notm}} will support networks deployed on Fabric 1.4 until six months after the {{site.data.keyword.blockchainfull_notm}} Platform 2.1.0 moves to the next long term release of Fabric. However, it is recommend that users upgrade the latest version of the images that are available for downlaod.

## Considerations and limitations

{{site.data.keyword.blockchainfull_notm}} images for Hyperledger Fabric are based on the open source [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}. The images do not include the {{site.data.keyword.blockchainfull_notm}} Platform console or operator.

- This offering is meant for experienced Fabric users. If you are still exploring Hyperledger Fabric, you can get started with [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
- You cannot use the {{site.data.keyword.blockchainfull_notm}} Platform console to deploy or operate the Fabric images.
- {{site.data.keyword.blockchainfull_notm}} provides support for Hyperledger Fabric only if you purchase the {{site.data.keyword.blockchainfull_notm}} 2.1.0 and deploy the {{site.data.keyword.IBM_notm}} signed images. You cannot purchase support for the open source Fabric Docker images. {{site.data.keyword.IBM_notm}} will not support images that have been altered.

## License and pricing

{{site.data.keyword.blockchainfull_notm}} Platform images can be purchased through an entitlement to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0. After purchasing the platform, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key. You can then use the entitlement key to download the images from the {{site.data.keyword.IBM_notm}} Entitled Registry.

For more information on the pricing of the images, to the [Pricing](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-rhos-pricing) topic.

## Get your entitlement key

When you purchase the {{site.data.keyword.blockchainfull_notm}} Platform from PPA, you receive an entitlement key for the software is associated with your MyIBM account. You need to access and save this key to deploy the platform.

1. Log in to [MyIBM Container Software Library](https://myibm.ibm.com/products-services/containerlibrary){: external} with the IBMid and password that are associated with the entitled software.

2. In the Entitlement keys section, select Copy key to copy the entitlement key to the clipboard.

## Downloading the {{site.data.keyword.blockchainfull_notm}} Platform images

You can use your entitlement key to download the {{site.data.keyword.blockchainfull_notm}} Platform images of Hyperledger Fabric from the {{site.data.keyword.IBM_notm}} Entitlement Registry. Use the following command to log in to the {{site.data.keyword.IBM_notm}} Entitlement Registry:
```
docker login --username cp --password <KEY> cp.icr.io
```
{:codeblock}

- Replace `<KEY>` with your entitlement key.

After you log in, use the following command to pull the Fabric images provided by {{site.data.keyword.blockchainfull_notm}}:
```
docker pull cp.icr.io/cp/ibp-peer:1.4.3-20190924-amd64
docker pull cp.icr.io/cp/ibp-orderer:1.4.3-20190924-amd64
docker pull cp.icr.io/cp/ibp-ca:1.4.3-20190924-amd64
docker pull cp.icr.io/cp/ibp-couchdb:2.3.1-20190924-amd64
docker pull cp.icr.io/cp/ibp-utilities:1.4.3-20190924-amd64
docker pull cp.icr.io/cp/ibp-ccenv:1.4.3-20190924-amd64
docker pull cp.icr.io/cp/ibp-goenv:1.4.3-20190924-amd64
docker pull cp.icr.io/cp/ibp-nodeenv:1.4.3-20190924-amd64
```
{:codeblock}

## Getting started

To get started using the Fabric images, you need to download the open source tooling provided by the Hyperledger community. Follow the steps to [download the Fabric Samples, Binaries, and configuration files](https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html){: external} in the Hyperledger Fabric documentation. These steps may download the open source Fabric images as well.

After you have downloaded the Fabric Samples, you will be able to find the configuration files and binaries that you that you need to set up a network in the `fabric-samples\config` and `fabric-samples\bin` folders. You can also find example artifacts and scripts for setting up a network using Docker Compose in the `fabric-samples\first-network` directory. You learn more about these artifacts and steps involved by visiting the accompanying [Build Your First Network](http://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html){: external} tutorial.

When using the open source configuration files, you need to alter the files to use the {{site.data.keyword.IBM_notm}} images instead of the open source Fabric Docker images. For each component, you need to alter the `image` section of the file to use the {{site.data.keyword.IBM_notm}} certified image instead of the open source Fabric Docker image. You also need add the `LICENSE` field to accept the {{site.data.keyword.IBM_notm}} license. If you are deploying a peer node, you need to use the core chaincode variables to instruct the peer to build chaincode using the {{site.data.keyword.IBM_notm}} certified images.

As an example, you can use the `peer-base.yaml` file in `fabric-samples/first-network` to deploy an ordering node or a peer node. You can find the orderer section of the file below:

```
orderer-base:
  image: hyperledger/fabric-orderer:$IMAGE_TAG
  environment:
    - FABRIC_LOGGING_SPEC=INFO
    - ORDERER_GENERAL_LISTENADDRESS=0.0.0.0
    - ORDERER_GENERAL_GENESISMETHOD=file
    - ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
    - ORDERER_GENERAL_LOCALMSPID=OrdererMSP
    - ORDERER_GENERAL_LOCALMSPDIR=/var/hyperledger/orderer/msp
    # enabled TLS
    - ORDERER_GENERAL_TLS_ENABLED=true
    - ORDERER_GENERAL_TLS_PRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_TLS_CERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_TLS_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
    - ORDERER_KAFKA_TOPIC_REPLICATIONFACTOR=1
    - ORDERER_KAFKA_VERBOSE=true
    - ORDERER_GENERAL_CLUSTER_CLIENTCERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_CLUSTER_CLIENTPRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_CLUSTER_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
  working_dir: /opt/gopath/src/github.com/hyperledger/fabric
  command: orderer
```

To use deploy an ordering node using the {{site.data.keyword.IBM_notm}} image, change the `image` field to `ibmblockchain/fabric-orderer:TAG`. Specify the tag of the release that you are using and drop the architecture (x86_64, ppc64le, s390x). Accept the license by adding a field of `LICENSE=accept`. After your changes, the section would look like the example below:

```
orderer-base:
  image: cp/ibp-orderer:1.4.3-20190924-amd64
  environment:
    - LICENSE=accept
    - FABRIC_LOGGING_SPEC=INFO
    - ORDERER_GENERAL_LISTENADDRESS=0.0.0.0
    - ORDERER_GENERAL_GENESISMETHOD=file
    - ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
    - ORDERER_GENERAL_LOCALMSPID=OrdererMSP
    - ORDERER_GENERAL_LOCALMSPDIR=/var/hyperledger/orderer/msp
    # enabled TLS
    - ORDERER_GENERAL_TLS_ENABLED=true
    - ORDERER_GENERAL_TLS_PRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_TLS_CERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_TLS_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
    - ORDERER_KAFKA_TOPIC_REPLICATIONFACTOR=1
    - ORDERER_KAFKA_VERBOSE=true
    - ORDERER_GENERAL_CLUSTER_CLIENTCERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_CLUSTER_CLIENTPRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_CLUSTER_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
  working_dir: /opt/gopath/src/github.com/hyperledger/fabric
  command: orderer
```

If you are using `peer-base.yaml` deploy a peer, make the same changes to the peer section of the file that you made to the orderer section. However, you also need add the core chaincode variables and specify the `fabric-ccenv`, `fabric-baseos`, `fabric-baseimage` images that you downloaded from {{site.data.keyword.IBM_notm}}. As an example, the peer section of `peer-base.yaml` would become:

```
services:
  peer-base:
    image: cp.icr.io/cp/ibp-peer:1.4.3-20190924-amd64
    environment:
      - LICENSE=accept
      - CORE_CHAINCODE_BUILDER=cp/ibp-ccenv:1.4.3-20190924-amd64
      - CORE_CHAINCODE_GOLANG_RUNTIME=cp/ibp-goenv:1.4.3-20190924-amd64
      - CORE_CHAINCODE_NODE_RUNTIME=cp/ibp-nodeenv:1.4.3-20190924-amd64
      - CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
      # the following setting starts chaincode containers on the same
      # bridge network as the peers
      # https://docs.docker.com/compose/networking/
      - CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=${COMPOSE_PROJECT_NAME}_byfn
      - FABRIC_LOGGING_SPEC=INFO
      #- FABRIC_LOGGING_SPEC=DEBUG
      - CORE_PEER_TLS_ENABLED=true
      - CORE_PEER_GOSSIP_USELEADERELECTION=true
      - CORE_PEER_GOSSIP_ORGLEADER=false
      - CORE_PEER_PROFILE_ENABLED=true
      - CORE_PEER_TLS_CERT_FILE=/etc/hyperledger/fabric/tls/server.crt
      - CORE_PEER_TLS_KEY_FILE=/etc/hyperledger/fabric/tls/server.key
      - CORE_PEER_TLS_ROOTCERT_FILE=/etc/hyperledger/fabric/tls/ca.crt
    working_dir: /opt/gopath/src/github.com/hyperledger/fabric/peer
    command: peer node start
```

The Fabric Samples published by the Hyperledger community are intended to be used only as examples. You should not use these samples as templates for deploying production networks.

## Getting support

If you encounter issues that relate to Hyperledger Fabric or the underlying images, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/support-ibm-blockchain-platform-v210){: external} page. When you open a case you will need to select your product, {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0. {{site.data.keyword.blockchainfull_notm}} does not provide support for issues you may encounter related to the deployment and management of your network. If you need assistance with deployment, see [{{site.data.keyword.blockchainfull_notm}} Platform 2.1.0](/docs/services/blockchain?topic=blockchain-console-ocp-about#console-ocp-about).

You can take advantage of free blockchain developer resources and support forums in order to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Fabric community. For more information, see [Resources and support forums](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support-resources).
