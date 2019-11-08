---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-07"

keywords: IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# Removing your deployment
{: #Removing-k8}

The {{site.data.keyword.blockchainfull}} Platform operator automatically restarts your blockchain nodes or your console if they stop or crash. As a result, you cannot manually remove your blockchain components by manually deleting their pods. Use the following steps to remove the {{site.data.keyword.blockchainfull_notm}} Platform from your cluster. You must follow these steps for Kubernetes namespace that you create.

## Step one: Use the console to delete your blockchain nodes

You can use your console to remove your blockchain node and any accompanying artifacts from your cluster. These artifacts include your ledger data in persistent storage and the keys that are stored in Kubernetes secrets. Log in to your console and go to the node overview page. Click the **Delete** under the node name and enter the node name on the slider to permanently remove the node from the cluster. Note that you can delete a node only by using the console that created the node. You cannot use your console to delete nodes that are deployed on other clusters. You can also delete nodes by using the {{site.data.keyword.blockchainfull_notm}} Platform APIs.

If you cannot use your console or the APIs to remove your nodes, you can manually remove all of the nodes from your cluster by using the kubeclt CLI. Navigate to your namespace:
```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{:codeblock}

Then run the following commands to delete all of your blockchain nodes:
```
kubectl delete ibpca --all
kubectl delete ibppeer --all
kubectl delete ibporderer --all
```
{:codeblock}

## Step two: Delete the {{site.data.keyword.blockchainfull_notm}} Platform operator

You can use the Kubectl CLI to remove the {{site.data.keyword.blockchainfull_notm}} Platform operator. When the operator is no longer running on your cluster, you can delete the console and any remaining nodes without them being restarted.

1. Log in to your cluster by using the Kubectl CLI. 

2. Use the CLI to switch to the namespace that you created for your blockchain network:

  ```
  kubectl project <NAMESPACE>
  ```
  {:codeblock}

3. You can remove the operator deployment using the Kubectl CLI:

  ```
  kubectl delete deployment ibp-operator
  ```
  {:codeblock}

## Step Three: Delete the {{site.data.keyword.blockchainfull_notm}} Platform console

After you remove the operator, you can then delete the console without it being restarted. When you are logged in to your cluster and are operating from your project, you can delete the console by issuing the following command:

```
kubectl delete ibpconsole --all
```
{:codeblock}

## Step Four: Remove policies and secrets

If you are done working with the {{site.data.keyword.blockchainfull_notm}} Platform on your Kubernetes cluster, you can delete your namespace.

```
kubectl delete namespaces  <NAMESPACE>
```
{:codeblock}

This command deletes any remaining blockchain nodes that are running on the project, in addition to your console and the {{site.data.keyword.blockchainfull_notm}} Platform operator.
{:important}

Deleting the namespace removes the {{site.data.keyword.blockchainfull_notm}} Platform Security Context Constraint, clusterRole, and ClusterRoleBinding that you applied. It deletes the secrets that you created. You cannot undo this action. As a result, deleting your project is not recommended unless you are not going to deploy another instance of the platform.
