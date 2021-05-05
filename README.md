# Kubernetes for Windows Testing

Test some of the Windows featurs of Azure Kubernetes Service (AKS).

## Prerequisites

### Azure CLI Prerequisites

As part of this demo, make sure all of the preview extensions are up to date.

```bash
az extension add --name connectedk8s
az extension add --name k8sconfiguration

az extension update --name aks-preview
az extension update --name connectedk8s
az extension update --name k8sconfiguration
```

### Setup Shell Variables

These variables will be used throughout the demo.

```bash
NAME=cdw-kubernetes-20210504
LOCATION=westus2
WIN_USERNAME=chriswiederspan
WIN_PASSWORD=YOUR_PASSWORD_GOES_HERE    (min 14 chars)
```

## Azure Resource Setup

### Create an Azure Resource Group

```bash
az group create -n $NAME -l $LOCATION
```

### Create an Azure Kubernetes Cluster

```bash
# Create cluster
az aks create \
--resource-group $NAME \
--name $NAME \
--location $LOCATION \
--kubernetes-version 1.20.5 \
--node-count 2 \
--network-plugin azure \
--generate-ssh-keys \
--enable-managed-identity \
--windows-admin-username $WIN_USERNAME \
--windows-admin-password $WIN_PASSWORD \
--enable-ahub
# --enable-addons monitoring

# Add a Windows node pool
az aks nodepool add \
--resource-group $NAME \
--cluster-name $NAME \
--name win001 \
--os-type Windows \
--node-count 1

# Connect kubectl
az aks get-credentials -n $NAME -g $NAME --overwrite

# Delete cluster
az aks delete \
--resource-group $NAME \
--name $NAME
```

### Check and Upgrade Node OS Version

```bash
az aks nodepool get-upgrades \
--nodepool-name win001 \
--cluster-name $NAME \
--resource-group $NAME

az aks nodepool show \
--resource-group $NAME \
--cluster-name $NAME \
--name win001 \
--query nodeImageVersion

# Get images sizes on Kubernetes
kubectl get nodes -o json | jq '.items[].status.images[] | .names[1], .sizeBytes'

```

### Other Utilities from Within Container

```cmd
systeminfo

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts" /s

```
