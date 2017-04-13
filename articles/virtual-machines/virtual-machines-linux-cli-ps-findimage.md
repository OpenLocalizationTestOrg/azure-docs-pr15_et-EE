<properties
   pageTitle="Valige Linux VM pildid koos Azure CLI | Microsoft Azure'i"
   description="Siit saate teada, kuidas kindlaks teha, publisher, pakkumine ja SKU piltide loomisel Linux virtuaalse masina ressursihaldur juurutamise mudeli."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="select-linux-vm-images-with-the-azure-cli"></a>Valige Linux VM Azure CLI-pildid

Selles teemas kirjeldatakse, kuidas leida tootjad, pakkumised, SKU-de jaoks ja versioonide iga koht, kuhu võib juurutamist. Näiteks on mõned sagedamini kasutatavad Linux VM pildid.

**Tabeli levinumaid Linux pildid**


| PublisherName                        | Pakkumine                                 | SKU-ga                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| RedHat                           | RHEL                                       | 7.2                              |
| credativ                         | Debian                                     | 8                                | 
| SUSE                             | openSUSE                                   | 13.2                             |
| SUSE                             | SLES                                       | 12-SP1                           |
| OpenLogic                        | CentOS                                     | 7.1                              |
| Kanoonilise                        | UbuntuServer                               | 14.04.4-LTS                      |
| CoreOS                           | CoreOS                                     | Stabiilne                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
