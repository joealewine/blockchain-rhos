---

copyright:
  years: 2019
lastupdated: "2019-11-15"

keywords: IBM Blockchain Platform, images

subcollection: blockchain-rhos

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Using the {{site.data.keyword.blockchainfull_notm}} images
{: #blockchain-images}

For experienced Hyperledger fabric customers, the {{site.data.keyword.blockchainfull}} Platform provides images for peer, CA, ordering service, and smart contract containers that are signed and supported by {{site.data.keyword.IBM_notm}}. These images are based on the open source [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/) code base and contain a number of enhancements for stability and serviceability.

{{site.data.keyword.blockchainfull_notm}} Platform images can be purchased through an entitlement to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1. The images are bundled with support from {{site.data.keyword.IBM_notm}}. {{site.data.keyword.blockchainfull_notm}} does not support blockchain components that are deployed using the open source Hyperledger Fabric Docker images.

## Supported Platforms

The IBM Blockchain images must be deployed using a container environment on x86_64 Hardware. The images have been tested on the following deployment platforms:

- Ubuntu Linux version 18.04.2
- Red Hat OpenShift 3.11
- OKD 3.11
- {{site.data.keyword.cloud_notm}} Private 3.2.1
- Azure Kubernetes Service
- Rancher 2.x

Although you can deploy the {{site.data.keyword.blockchainfull_notm}} images on Mac OS for testing purposes, the permissions on Mac OS might prevent you from instantiating a chaincode.

## Supported Fabric versions

The {{site.data.keyword.blockchainfull_notm}} Docker images are based on Hyperledger Fabric v1.4.3. The documentation to install and deploy the images will be updated with the latest version of Fabric used by the {{site.data.keyword.blockchainfull_notm}} Platform 2.1.1.

Hyperledger Fabric v1.4 is the long term support release of Hyperledger Fabric. As a result, {{site.data.keyword.blockchainfull_notm}} will provide support networks deployed on Fabric v1.4.3 or higher until six months after the {{site.data.keyword.blockchainfull_notm}} Platform 2.1.1 moves to the next long term support release.

## Considerations and limitations

The images do not include the {{site.data.keyword.blockchainfull_notm}} Platform console or operator. This offering is meant for experienced Fabric users with existing deployments. If you are still exploring Hyperledger Fabric, you can get started with [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
{:important}

- {{site.data.keyword.blockchainfull_notm}} provides support for Hyperledger Fabric only if you purchase {{site.data.keyword.blockchainfull_notm}} 2.1.1 and deploy the {{site.data.keyword.blockchainfull_notm}} images. You cannot purchase support for the Hyperledger Fabric Docker images that are provided by the Hyperledger community. {{site.data.keyword.blockchainfull_notm}} does not support images that have been altered.
- You cannot use the {{site.data.keyword.blockchainfull_notm}} Platform console to deploy or operate the images.

## License and pricing

{{site.data.keyword.blockchainfull_notm}} Platform images can be purchased through an entitlement to the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1. After you purchase the platform, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key. You can then use the entitlement key to download the images from the {{site.data.keyword.IBM_notm}} Entitled Registry.

For more information on the pricing of the images, to the [Pricing](/docs/services/blockchain-rhos?topic=blockchain-rhos-ibp-rhos-pricing) topic.

## Get your entitlement key

When you purchase the {{site.data.keyword.blockchainfull_notm}} Platform from PPA, you receive an entitlement key for the software is associated with your MyIBM account. You need to access and save this key to deploy the platform.

1. Log in to [MyIBM Container Software Library](https://myibm.ibm.com/products-services/containerlibrary){: external} with the IBMid and password that are associated with the entitled software.

2. In the Entitlement keys section, select **Copy key** to copy the entitlement key to the clipboard.

## Downloading the {{site.data.keyword.blockchainfull_notm}} Platform images

You can use your entitlement key to download the {{site.data.keyword.blockchainfull_notm}} Platform images from the {{site.data.keyword.IBM_notm}} Entitlement Registry. Use the following command to log in to the {{site.data.keyword.IBM_notm}} Entitlement Registry:
```
docker login --username cp --password <KEY> cp.icr.io
```
{:codeblock}

- Replace `<KEY>` with your entitlement key.

After you log in, use the following command to pull the {{site.data.keyword.blockchainfull_notm}} images:
```
docker pull cp.icr.io/cp/ibp-peer:1.4.3-20191108-amd64
docker pull cp.icr.io/cp/ibp-orderer:1.4.3-20191108-amd64
docker pull cp.icr.io/cp/ibp-ca:1.4.3-20191108-amd64
docker pull cp.icr.io/cp/ibp-couchdb:2.3.1-20191108-amd64
docker pull cp.icr.io/cp/ibp-utilities:1.4.3-20191108-amd64
docker pull cp.icr.io/cp/ibp-ccenv:1.4.3-20191108-amd64
docker pull cp.icr.io/cp/ibp-goenv:1.4.3-20191108-amd64
docker pull cp.icr.io/cp/ibp-nodeenv:1.4.3-20191108-amd64
```
{:codeblock}



## Getting started
{: #getting-started}

To deploy and operate the {{site.data.keyword.blockchainfull_notm}} images, you can download the open source tools that are provided by the Hyperledger community. Follow the steps to [Install the prerequisites](https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html) and [Download the Fabric Samples, Binaries, and configuration files](https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html){: external} in the Hyperledger Fabric documentation. These steps might download the open source Fabric images as well.

After you download the Fabric Samples repository, you can find the configuration files and binaries that you need to set up a network in the `fabric-samples\config` and `fabric-samples\bin` folders. You can also find example artifacts and scripts for how to set up a network by using Docker Compose in the `fabric-samples\first-network` directory. You learn more about these artifacts and the steps that are involved by reading the accompanying [Build Your First Network](http://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html){: external} tutorial.

If you are using the open source configuration files, you need to make the following changes to deploy the {{site.data.keyword.blockchainfull_notm}} images:

1. For each component, you need to alter the `image` field to use the {{site.data.keyword.blockchainfull_notm}} image instead of the open source image.

2. You need add the `LICENSE` field to accept the {{site.data.keyword.IBM_notm}} license:
  ```
  LICENSE=accept
  ```

3. If you are deploying a peer node, you need to use the core chaincode variables to instruct the peer to build chaincode using the {{site.data.keyword.blockchainfull_notm}} certified images. For example, if you are using Go chaincode, you need to set the following variables:
  ```
  CORE_CHAINCODE_NODE_RUNTIME=cp.icr.io/cp/ibp-nodeenv:1.4.3-20191108-amd64
  CORE_CHAINCODE_GOLANG_DYNAMICLINK=true
  ```

4. You need to mount the configuration files, `core.yaml` or `orderer.yaml`, inside the node container. You also need to set the `FABRIC_CFG_PATH` environment variable to the path where you mounted the configuration files.

  If you are deploying a peer node, you need to comment out or remove the `PKCS#11` section of the file unless you are using an HSM. The section of `core.yaml` that needs to be removed is below:
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

5. For security reasons, the {{site.data.keyword.blockchainfull_notm}} images require a different level of permission to access data than the open source images. You need to change the access of the folder that you mount to store your organization MSP and TLS certificates and the folder that is mounted to store your ledger data. You can use the following command to change access to the folders before they are mounted inside the container:
  ```
  chmod -R 777 <folder_name>
  ```

In addition to using the Fabric tools, you can also use the tools that are provided in the `ibp-utilities` image to operate your network. The utilities image includes the configtxlator, cryptogen, configtxgen binaries.



## Example

We can provide an example of the changes you need to make by updating the `first-network` sample to run the {{site.data.keyword.blockchainfull_notm}} images instead of the community Docker images.

You need to update the `peer-base.yaml` and `docker-compose-base.yaml` that in the `fabric-samples/first-network/base` folder. You can find the orderer section of the `peer-base.yaml` below:

```yaml
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

To deploy an ordering node by using the {{site.data.keyword.blockchainfull_notm}} image, change the `image` field to the tag of the {{site.data.keyword.blockchainfull_notm}} image, `cp.icr.io/cp/ibp-orderer:1.4.3-20191108-amd64`. Accept the license by adding the field of `LICENSE=accept`. You then need to add the `FABRIC_CFG_PATH` environment variable and set the path to the folder where you mount the configuration files. Set the `working_dir` variable to the same path. After your changes, the orderer section would look like the example below:

```yaml
orderer-base:
  image: cp.icr.io/cp/ibp-orderer:1.4.3-20191108-amd64
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

If you are deploying a peer, you can make the same changes to the peer section of the `peer-base.yaml` file. However, you need add the core chaincode variables and specify images that you downloaded from {{site.data.keyword.IBM_notm}} as the chaincode builder and the runtime environment. You also need to set `DYNAMICLINK=true` After your changes, the peer section would look like the example below:

```yaml
services:
  peer-base:
    image: cp.icr.io/cp/ibp-peer:1.4.3-20191108-amd64
    environment:
      - LICENSE=accept
      - FABRIC_CFG_PATH=/etc/hyperledger/fabric
      - CORE_CHAINCODE_BUILDER=cp.icr.io/cp/ibp-ccenv:1.4.3-20191108-amd64
      - CORE_CHAINCODE_GOLANG_RUNTIME=cp.icr.io/cp/ibp-goenv:1.4.3-20191108-amd64
      - CORE_CHAINCODE_NODE_RUNTIME=cp.icr.io/cp/ibp-nodeenv:1.4.3-20191108-amd64
      - CORE_CHAINCODE_GOLANG_DYNAMICLINK=true
      - CORE_CHAINCODE_NODE_DYNAMICLINK=true
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

The files that are mounted into the peer and orderer containers are specified in the `docker-compose-base.yaml` file. In the `volumes:` section, you can add the following line ``../../config:/etc/hyperledger/fabric`` to mount the peer and orderer configuration files that are located in the `fabric-samples` directory. You need to remove the `PKCS#11` section of the `fabric-samples/config/core.yaml` file. The ledger data folder is mounted in the `/var/hyperledger/production`. We need to change this path to an existing folder to change the permissions of that folder. After your changes, `docker-compose-base.yaml` would look like the example below for an ordering node and one of your peers:

```yaml
orderer.example.com:
  container_name: orderer.example.com
  extends:
    file: peer-base.yaml
    service: orderer-base
  volumes:
      - ../channel-artifacts/genesis.block:/var/hyperledger/orderer/orderer.genesis.block
      - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/msp:/var/hyperledger/orderer/msp
      - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/:/var/hyperledger/orderer/tls
      - ../crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com:/var/hyperledger/production/orderer
      - ../../config:/etc/hyperledger/fabric
  ports:
- 7050:7050

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
      - ../crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com:/var/hyperledger/production
      - ../../config:/etc/hyperledger/fabric
  ports:
    - 7051:7051
```

After you edit the file, you need to change the security permissions of the folders that are mounted to store your certificates and ledger data. Your peer MSP folder and TLS certificates are both stored in the `crypto-config` folder that is created when you run `./byfn.sh generate`. To simplify the deployment, the example also mounts a folder in this directory to store our ledger data. You can run the following commands to generate the crypto material and folders, and then provide those folders the required permissions:
```
./byfn.sh generate
chmod -R 777 crypto-config
```

If you want to deploy a raft ordering service, you need to make the same edits to the `docker-compose-etcdraft2.yaml` file that you made to the order section of `docker-compose-base.yaml`. You need to create ledger folders for each ordering node and change the permissions of each folder.

After you update the relevant files, you can test that you can deploy your images by running the following command:
```
sudo ./byfn.sh up
```

You can use the updates that are provided in this example to deploy the {{site.data.keyword.blockchainfull_notm}} images in the environment of your choice. The Fabric samples that are published by the Hyperledger community are intended to be used only as examples. Do not use the samples as templates for deploying production networks.



## Interoperability

Nodes that are deployed by using the {{site.data.keyword.blockchainfull_notm}} images can join the channels of other {{site.data.keyword.blockchainfull_notm}} offerings, such as {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1 and {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. However, because you cannot use the console to operate the images, you need to use Fabric tools to join existing channels that were created with a console or create new channels. 

## Getting support

If you encounter issues that are related to Hyperledger Fabric or the underlying images, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/support-ibm-blockchain-platform-v21x){: external} page. When you open a case you need to select your product, {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1.

{{site.data.keyword.IBM_notm}} provides support for issues that are related to the Hyperledger Fabric code or the steps to download and deploy the images that you can find in this documentation. {{site.data.keyword.IBM_notm}} does not provide support for issues that relate to the operation of the images or your underlying infrastructure. Deployment issues due to the specific circumstances of the customer environment will be the customer's responsibility to investigate. If you need assistance with the deployment and management of a Fabric network, use the [{{site.data.keyword.blockchainfull_notm}} Platform 2.1.1](/docs/services/blockchain?topic=blockchain-console-ocp-about#console-ocp-about) offering.

You can take advantage of free blockchain developer resources and support forums to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Fabric community. For more information, see [Resources and support forums](/docs/services/blockchain-rhos?topic=blockchain-rhos-blockchain-support#blockchain-support-resources).
