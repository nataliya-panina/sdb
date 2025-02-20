```
lsblk | grep sd
sda      8:0    0   100G  0 disk 
├─sda1   8:1    0     1M  0 part 
├─sda2   8:2    0   513M  0 part /boot/efi
└─sda3   8:3    0  99,5G  0 part /
sdb      8:16   0    20G  0 disk 
```
Для разделения возьму диск /dev/sdb, на котором ничего нет
```
sudo parted /dev/sdb
(parted) mklabel gpt
(parted) mkpart primary ext4 1MB 101MB                                    
(parted) print   
```
```
Modèle : VMware, VMware Virtual S (scsi)
Disque /dev/sdb : 21,5GB
Taille des secteurs (logiques/physiques) : 512B/512B
Table de partitions : gpt
Drapeaux de disque : 

Numéro  Début   Fin    Taille  Système de fichiers  Nom      Drapeaux
 1      1049kB  101MB  99,6MB  ext4                 primary
```
