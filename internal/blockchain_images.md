---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-15"

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

The {{site.data.keyword.blockchainfull_notm}} images cannot be deployed on Mac OS or Windows for testing purposes.

## Supported Fabric versions

The {{site.data.keyword.blockchainfull_notm}} Docker images are based on Hyperledger Fabric v1.4.3. The Fabric images will be updated to support the latest version of Fabric used by the {{site.data.keyword.blockchainfull_notm}} Platform 2.1.0. Because, version 1.4 is the long term support release of Hyperledger Fabric, {{site.data.keyword.blockchainfull_notm}} will support networks deployed on Fabric 1.4 until six months after the {{site.data.keyword.blockchainfull_notm}} Platform 2.1.0 moves to the next long term release of Fabric. However, it is recommend that users upgrade to the latest version of the images that are available.

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

When using the open source configuration files, you need to alter the files to be able to use the {{site.data.keyword.IBM_notm}} images instead of the open source Fabric Docker images:

1. For each component, you need to alter the `image` section of the file to use the {{site.data.keyword.IBM_notm}} image instead of the open source Fabric Docker image.

2. You need add the `LICENSE` field to accept the {{site.data.keyword.IBM_notm}} license:
  ```
  LICENSE=accept
  ```

3. If you are deploying a peer node, you need to use the core chaincode variables to instruct the peer to build chaincode using the {{site.data.keyword.IBM_notm}} certified images.

4. You need to mount the configuration files, `core.yaml` and `orderer.yaml`, inside the node container. Add the `FABRIC_CFG_PATH` environment variable and set it to the path where you mounted the configuration files.

  If you are deploying a peer node, you need to comment out or remove the `PKCS#11` section of the file unless you are using an HSM. You can find the section of `core.yaml` that needs to be removed below:
  ```
   Settings for the PKCS#11 crypto provider (i.e. when DEFAULT: PKCS11)
   PKCS11:
       # Location of the PKCS11 module library
       Library:
       # Token Label
      Label:
      # User PIN
      Pin:
      Hash:
      Security:
      FileKeyStore:
          KeyStore:
  ```

5. For security reasons, the {{site.data.keyword.IBM_notm}} images have different permissions to access files than the open source images. You need to change the access of the folder that is mounted to store your ledger data by using the following command:
  ```
  chmod -R 775 <folder_name>
  ```

### Example

You can use the `peer-base.yaml` file in `fabric-samples/first-network` to deploy an ordering or a peer node. You can find the orderer section of the file below:

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

To use deploy an ordering node using the {{site.data.keyword.IBM_notm}} image, change the `image` field to the tag of the {{site.data.keyword.IBM_notm}} image, `cp.icr.io/cp/ibp-orderer:1.4.3-20190924-amd64`. Specify the tag of the release that you are using and drop the architecture (x86_64, ppc64le, s390x). Accept the license by adding the field of `LICENSE=accept`. Add the `FABRIC_CFG_PATH` environment variable and set the path to the folder where you will mount the configuration files. Set the `working_dir` variable to the same path. After your changes, the orderer section would look like the example below:

```
orderer-base:
  image: cp.icr.io/cp/ibp-orderer:1.4.3-20190924-amd64
  environment:
    - LICENSE=accept
    - FABRIC_CFG_PATH=/etc/hyperledger/fabric
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
  working_dir: /etc/hyperledger/fabric
  command: orderer
```

If you are deploying a peer, you can make the same changes to the peer section of the file. However, you also need add the core chaincode variables and specify the `fabric-ccenv`, `fabric-baseos`, `fabric-baseimage` images that you downloaded from {{site.data.keyword.IBM_notm}}. After your changes, the peer section of `peer-base.yaml` would look like the example below:

```
services:
  peer-base:
    image: cp.icr.io/cp/ibp-peer:1.4.3-20190924-amd64
    environment:
      - LICENSE=accept
      - FABRIC_CFG_PATH=/etc/hyperledger/fabric
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
    working_dir: /etc/hyperledger/fabric
    command: peer node start
```
You specify which files are mounted into the containers with volumes using the `docker-compose-base.yaml` file. In the `volumes:` section, you can add the following line ``../../config:/etc/hyperledger/fabric`` to mount the configuration files in the `fabric-samples` directory.

```
peer0.org1.example.com:
  container_name: peer0.org1.example.com
  extends:
    file: peer-base.yaml
    service: peer-base
  environment:
    - CORE_PEER_ID=peer0.org1.example.com
    - CORE_PEER_ADDRESS=peer0.org1.example.com:7051
    - CORE_PEER_LISTENADDRESS=0.0.0.0:7051
    - CORE_PEER_CHAINCODEADDRESS=peer0.org1.example.com:7052
    - CORE_PEER_CHAINCODELISTENADDRESS=0.0.0.0:7052
    - CORE_PEER_GOSSIP_BOOTSTRAP=peer1.org1.example.com:8051
    - CORE_PEER_GOSSIP_EXTERNALENDPOINT=peer0.org1.example.com:7051
    - CORE_PEER_LOCALMSPID=Org1MSP
  volumes:
      - /var/run/:/host/var/run/
      - ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp:/etc/hyperledger/fabric/msp
      - ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls:/etc/hyperledger/fabric/tls
      - ../peer0.org1.example.com:/var/hyperledger/production
      - ../../config:/etc/hyperledger/fabric
  ports:
    - 7051:7051
```

The peer ledger is stored in the `/var/hyperledger/production` folder. The folder `/crypto-config/peer0.org1.example.com:` is created and then mounted to store the ledger data. You need to change the permissions of this folder before you start the peer node:
```
mkdir peer0.org1.example.com
chmod -R 775 peer0.org1.example.com
```

The Fabric samples published by the Hyperledger community are intended to be used only as examples. You should not use the samples as templates for deploying production networks.

## Interoperability

Nodes that are deployed by using the {{site.data.keyword.IBM_notm}} images can join the channels of other {{site.data.keyword.blockchainfull_notm}} offerings, such as {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0 and {{site.data.keyword.blockchainfull_notm}} Platform for IBM Cloud. However, because you cannot use the console to operate the images, you need to use fabric tooling to create new channels or join the channels that have been created using the other offerings.

## Getting support

If you encounter issues that are related to Hyperledger Fabric or the underlying images, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/support-ibm-blockchain-platform-v210){: external} page. When you open a case you will need to select your product, {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0.

{{site.data.keyword.IBM_notm}} provides support for issues that you may encounter related to Hyperledger Fabric or the steps to pull and deploy the images that you can find in this documentation. {{site.data.keyword.IBM_notm}} does not provide support for support for issues that relate to management or deployment of the images or your underlying infrastructure. If you need assistance with the deployment and management of a Fabric network, use the [{{site.data.keyword.blockchainfull_notm}} Platform 2.1.0](/docs/services/blockchain?topic=blockchain-console-ocp-about#console-ocp-about) offering.

You can take advantage of free blockchain developer resources and support forums in order to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Fabric community. For more information, see [Resources and support forums](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support-resources).
