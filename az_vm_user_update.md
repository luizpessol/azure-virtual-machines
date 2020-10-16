## Listar suas VMs, Name, S.O, Versão e Usuário Admin:

az vm list \
    --resource-group Rg-VM \
    --query '[].{Name:name, OS:storageProfile.osDisk.osType, VERSION:storageProfile.imageReference.sku, Admin:osProfile.adminUsername}' \
    --output table

## Update da senha em sua VM:

az vm user update \
  -u lpessol \
  -p 'p3$$0L83br@$il' \
  -n VM01 \
  -g Rg-VM

## Acesso ssh com usuário e senha:

ssh lpessol@13.68.152.53

## Criar novo par de chaves:
-t = tipo da sua chave
-b = número de bits da sua chave
-f = nome do arquivo que vamos armazenar a chave

ssh-keygen -t rsa -b 2048 -f vm02

## Update de sua chave pubica na VM Linux:

az vm user update \
  --username ze \
  --ssh-key-value "$(<vm02.pub)" \
  --name VM02 \
  --resource-group Rg-VM

## Acesso ssh com chave:

ssh -i vm02 ze@40.121.142.145
