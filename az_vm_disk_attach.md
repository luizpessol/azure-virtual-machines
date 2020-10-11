## Criar um novo disco e anexar o mesmo em uma VM:

az vm disk attach \
   --resource-group rg-VM \
   --vm-name VM02 \
   --name DataDisk \
   --new \
   --size-gb 50 \
   --sku StandardSSD_LRS \
   

## Acesse sua VM Linux - centOS

ssh usuario@100.200.300.20

ou

ssh -i key.pem usuario@100.200.300.20

## Localizar o novo disco:

lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"

## Criar uma partição:

sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%

## Formatar a partição com sistema de arquivos XFS:

sudo mkfs.xfs /dev/sdc1

## Vamos rodar o partprobe para informar o sistema das mudanças na tabela de partições:

sudo partprobe /dev/sdc1

## Criar um diretório para montar o disco:

sudo mkdir /DataDisk

## Exibir o UUID:

sudo blkid

## Copiar o seu UUID e colar aqui:

UUID=20f65bb9-9c69-4630-a04b-0c2045cf8307   /DataDisk   xfs   defaults,nofail   1   2

## Montar/persistir o disco adicionando seu UUID no fstab:

sudo vim /etc/fstab

## Montor ou desmontar o novo disco:

sudo mount /dev/sdc1 /DataDisk

sudo umount /DataDisk
