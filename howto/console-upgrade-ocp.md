---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-24"

keywords: OpenShift, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# Upgrading your console and components
{: #upgrade-ocp}

You can upgrade the {{site.data.keyword.blockchainfull}} Platform 2.1.0 without disrupting a running network. Because the platform is deployed using a Kubernetes operator, you can pull the upgraded {{site.data.keyword.blockchainfull_notm}} Platform images from the IBM Entitlement registry without having to reinstall the platform. After you upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator and console, you can use your console to upgrade the docker images of your blockchain nodes.

You can follow the instructions in this guide to upgrade to the latest version of the platform. This page is updated with the most recent {site.data.keyword.blockchainfull_notm}} platform images and configuration updates. Because the {{site.data.keyword.blockchainfull_notm}} Platform is compatible with later versions, you can upgrade any deployed network by using the following instructions.
{:shortdesc}

You can upgrade your network by using the following steps:

1. [Upgrade the {site.data.keyword.blockchainfull}} Platform operator](#upgrade-ocp-operator)
2. [Upgrade your console](#upgrade-ocp-console)
3. [Use your console to upgrade your running blockchain nodes](#upgrade-ocp-nodes)

You can continue to submit transactions to your network while you are upgrading the operator and the console. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the upgrade process.

## Before you begin

To upgrade your network, you need to [retrieve your entitlement key](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-entitlement-key) from the My IBM Dashboard, and [create a Kubernetes secret](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp##deploy-ocp-docker-registry-secret) to store the key on your OpenShift project. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download a key and create a new secret.

## Step one: Upgrade the {{site.data.keyword.blockchainfull_notm}} operator
{: #upgrade-ocp-operator}

You can upgrade the {{site.data.keyword.blockchainfull_notm}} operator by fetching the operator deployment spec from your OpenShift project. You can then update the spec to pull the latest operator image from the IBM Entitlement registry.

Log in to your cluster by using the OpenShift CLI. Because each {{site.data.keyword.blockchainfull_notm}} network runs in a different project, you must switch to each OpenShift project and upgrade each network separately. Go to the OpenShift project of the network that you want to upgrade. Replace `<PROJECT_NAME>` with the name of your project.
```
oc project <PROJECT_NAME>
```
{:codeblock}

When you are operating from your project, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibp-operator -o yaml > operator.yaml
```
{:codeblock}

Open the `operator.yaml` file in a text editor and save a new copy of the file as `operator-upgrade.yaml`. It is important to preserve the original spec in case you need to revert to your existing deployment in case a problem occurs during the upgrade process.

In the `operator-upgrade.yaml` file, you need to update the `image:` field with the updated version of the operator image. You can find the name and tag of the latest operator image below:
```
cp.icr.io/cp/ibp-operator:2.1.0-20190924-amd64
```
{:codeblock}

When you are done editing the file, the relevant section looks like the following:
```
cp.icr.io/cp/ibp-operator:2.1.0-20190924-amd64
imagePullPolicy: Always
```
{:codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml
```
{:codeblock}

You can use the `kubectl get deployment` command to confirm that the command upgraded the operator spec.

After you apply the `operator-upgrade.yaml` custom resource definition to your OpenShift project, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to install and instantiate chaincode, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment ibp-operator`. If the upgrade is successful, then you can see the following tables with four ones displayed.
```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1         1         1            1           1m
```

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/services/blockchain-rhos/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f operator.yaml` to restore your original operator deployment.

## Step two: Upgrade the {{site.data.keyword.blockchainfull_notm}} Platform console
{: #upgrade-ocp-console}

After you upgrade the operator, you can upgrade your console. Updating the console spec downloads the latest images of the blockchain components.

Log in to your cluster by using the OpenShift CLI and switch to your OpenShift project. Run the following command to download the custom resource definition of the console:
```
kubectl get deployment ibpconsole -o yaml > console.yaml
```
{:codeblock}

Open `console.yaml` in a text editor and save a new copy of the file as `console-upgrade.yaml`. You need to update `console-upgrade.yaml` with the latest images of {{site.data.keyword.blockchainfull_notm}} Platform console and the blockchain nodes. You can find the latest list of images below:

```
image:
    imagePullSecret: docker-key-secret
    consoleInitImage: cp.icr.io/cp/ibp-init
    consoleInitTag: 2.1.0-20190924-amd64
    consoleImage: cp.icr.io/cp/ibp-console
    consoleTag: 2.1.0-20190924-amd64
    configtxlatorImage: cp.icr.io/cp/ibp-utilities
    configtxlatorTag: 1.4.3-20190924-amd64
    couchdbImage: cp.icr.io/cp/ibp-couchdb
    couchdbTag: 2.3.1-20190924-amd64
    deployerImage: cp.icr.io/cp/ibp-deployer
    deployerTag: 2.1.0-20190924-amd64
versions:
    ca:
      1.4.3-0:
        default: true
        version: 1.4.3-0
        image:
          caInitImage: cp.icr.io/cp/ibp-ca-init
          caInitTag: 2.1.0-20190924-amd64
          caImage: cp.icr.io/cp/ibp-ca
          caTag: 1.4.3-20190924-amd64
    peer:
      1.4.3-0:
        default: true
        version: 1.4.3-0
        image:
          peerInitImage: cp.icr.io/cp/ibp-init
          peerInitTag: 2.1.0-20190924-amd64
          peerImage: cp.icr.io/cp/ibp-peer
          peerTag: 1.4.3-20190924-amd64
          dindImage: cp.icr.io/cp/ibp-dind
          dindTag: 1.4.3-20190924-amd64
          fluentdImage: cp.icr.io/cp/ibp-fluentd
          fluentdTag: 2.1.0-20190924-amd64
          grpcwebImage: cp.icr.io/cp/ibp-grpcweb
          grpcwebTag: 2.1.0-20190924-amd64
          couchdbImage: cp.icr.io/cp/ibp-couchdb
          couchdbTag: 2.3.1-20190924-amd64
    orderer:
      1.4.3-0:
        default: true
        version: 1.4.3-0
        image:
          ordererInitImage: cp.icr.io/cp/ibp-init
          ordererInitTag: 2.1.0-20190924-amd64
          ordererImage: cp.icr.io/cp/ibp-orderer
          ordererTag: 1.4.3-20190924-amd64
          grpcwebImage: cp.icr.io/cp/ibp-grpcweb
          grpcwebTag: 2.1.0-20190924-amd64
```
{:codeblock}

The updated images must be added as a complete block and replace the `image:` section of `console-upgrade.yaml` file. If you used a different name when you created your [docker token secret](/docs/services/blockchain-rhos/howto?topic=blockchain-rhos-deploy-ocp#deploy-ocp-docker-registry-secret), you need to edit the field of `imagePullSecret: docker-key-secret`. When you are done editing the file, `console-upgrade.yaml` would resemble the following example:

```
apiVersion: ibp.com/v1alpha1
kind: IBPConsole
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: '{"license":"accept","serviceAccountName":"default","email":"gvt@ibm.com","passwordSecretName":"ibpconsole-console-pw","image":{"imagePullSecret":"regcred"},"storage":{"console":{"size":"1Gi"}},"networkinfo":{"domain":"ibp20openshifttestcluster-0defdaa0c51bd4a2dcb024eab4bf04a1-0001.us-south.containers.appdomain.cloud","peerPort":0}}'
    last-applied-spec: '{"license":"accept","serviceAccountName":"default","email":"gvt@ibm.com","passwordSecretName":"ibpconsole-console-pw","image":{"imagePullSecret":"regcred"},"storage":{"console":{"size":"1Gi"}},"networkinfo":{"domain":"ibp20openshifttestcluster-0defdaa0c51bd4a2dcb024eab4bf04a1-0001.us-south.containers.appdomain.cloud","peerPort":0}}'
  creationTimestamp: 2019-09-17T01:08:17Z
  generation: 3
  name: ibpconsole
  namespace: gvttest
  resourceVersion: "10015040"
  selfLink: /apis/ibp.com/v1alpha1/namespaces/gvttest/ibpconsoles/ibpconsole
  uid: a1ec51af-d8e7-11e9-b410-f29368fc99e3
spec:
  email: user@website.com
  image:
      imagePullSecret: docker-key-secret
      consoleInitImage: cp.icr.io/cp/ibp-init
      consoleInitTag: 2.1.0-20190924-amd64
      consoleImage: cp.icr.io/cp/ibp-console
      consoleTag: 2.1.0-20190924-amd64
      configtxlatorImage: cp.icr.io/cp/ibp-utilities
      configtxlatorTag: 1.4.3-20190924-amd64
      couchdbImage: cp.icr.io/cp/ibp-couchdb
      couchdbTag: 2.3.1-20190924-amd64
      deployerImage: cp.icr.io/cp/ibp-deployer
      deployerTag: 2.1.0-20190924-amd64
  versions:
      ca:
        1.4.3-0:
          default: true
          version: 1.4.3-0
          image:
            caInitImage: cp.icr.io/cp/ibp-ca-init
            caInitTag: 2.1.0-20190924-amd64
            caImage: cp.icr.io/cp/ibp-ca
            caTag: 1.4.3-20190924-amd64
      peer:
        1.4.3-0:
          default: true
          version: 1.4.3-0
          image:
            peerInitImage: cp.icr.io/cp/ibp-init
            peerInitTag: 2.1.0-20190924-amd64
            peerImage: cp.icr.io/cp/ibp-peer
            peerTag: 1.4.3-20190924-amd64
            dindImage: cp.icr.io/cp/ibp-dind
            dindTag: 1.4.3-20190924-amd64
            fluentdImage: cp.icr.io/cp/ibp-fluentd
            fluentdTag: 2.1.0-20190924-amd64
            grpcwebImage: cp.icr.io/cp/ibp-grpcweb
            grpcwebTag: 2.1.0-20190924-amd64
            couchdbImage: cp.icr.io/cp/ibp-couchdb
            couchdbTag: 2.3.1-20190924-amd64
      orderer:
        1.4.3-0:
          default: true
          version: 1.4.3-0
          image:
            ordererInitImage: cp.icr.io/cp/ibp-init
            ordererInitTag: 2.1.0-20190924-amd64
            ordererImage: cp.icr.io/cp/ibp-orderer
            ordererTag: 1.4.3-20190924-amd64
            grpcwebImage: cp.icr.io/cp/ibp-grpcweb
            grpcwebTag: 2.1.0-20190924-amd64
  license: accept
  networkinfo:
    domain: openshiftcluster.appdomain.cloud
    peerPort: 0
  passwordSecretName: ibpconsole-console-pw
  serviceAccountName: default
  storage:
    console:
      size: 10Gi
status:
  lastHeartbeatTime: 2019-09-17 01:11:39.713842542 +0000 UTC m=+218.363012778
  reason: allPodsRunning
  status: "True"
  type: Deployed
  ```

Save the file on your local system. You can then issue the following command upgrade your console:
```
kubectl apply -f console-upgrade.yaml
```
{:codeblock}

After you apply the file, the console deployment restarts and downloads the updated images for the console and the components. The console upgrade takes a few minutes. You cannot access the console UI from your browser or use the {{site.data.keyword.blockchainfull_notm}} APIs during the upgrade.

You can check that the upgrade is finished by running `kubectl get deployment ibpconsole`. If the upgrade is successful, then you can see the following tables with four ones displayed.
```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ibpconsole     1         1         1            1           1m
```

If you experience a problem while you are upgrading the console, go to this [troubleshooting topic](/docs/services/blockchain-rhos/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f console.yaml` to restore your original operator deployment.

## Step three: Upgrade your blockchain nodes
{: #upgrade-ocp-nodes}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. Browse to the console UI open the nodes overview tab. You can find the **Patch available** text on a node tile if there is an update available for the component. You can install this patch whenever you are ready. These patches are optional, but they are recommended. You cannot patch nodes that were imported into the console.

Apply patches to nodes one at a time. Your nodes are unavailable to process requests or transactions while the patch is being applied. Therefore, to avoid any disruption of service, whenever possible you should ensure that another node of the same type is available to process requests. Installing patches on a node takes about a minute to complete and when the update is complete, the node is ready to process requests.
{:important}

To apply a patch to a node, open the node tile and click the **Install patch** button. You cannot patch nodes that you imported to the console.
