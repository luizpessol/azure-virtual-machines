## Variável de Ambiente, Região, Resource Group VM, subnet, vnet e Resource Group da Vnet:

```
REGION_NAME=eastus
RESOURCE_GROUP=rg-VM
VNET_NAME=vnet-hub
SUBNET_NAME=infra
RG_VNET=Rg-vnet-hub
```

## Armazenar em uma variável ID da Subnet:

```
SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RG_VNET \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
```

## Criar availability-set

```
az vm availability-set create \
    --resource-group $RESOURCE_GROUP \
    --name av-set-vm02 \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

## Criar a VM Linux com centOS

```
az vm create \
    --name VM02 \
    --resource-group $RESOURCE_GROUP \
    --subnet $SUBNET_ID \
    --image OpenLogic:CentOS:7-CI:latest \
    --os-disk-size-gb 50 \
    --size Standard_DS2_v2 \
    --location eastus \
    --nsg-rule SSH \
    --availability-set av-set-vm02 \
    --authentication-type ssh \
    --generate-ssh-keys \
    --public-ip-address-allocation static \
    --custom-data cloud-init.yaml
```

## Liberar acesso porta HTTP 80 para o mundo:

```
az vm open-port \
    --port 80 \
    --resource-group $RESOURCE_GROUP \
    --name VM02
```
