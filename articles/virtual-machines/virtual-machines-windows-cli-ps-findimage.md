<properties
   pageTitle="Valige Windows VM pildid | Microsoft Azure'i"
   description="Siit saate teada, kuidas kindlaks teha, publisher, pakkumine ja SKU piltide loomisel Windows virtuaalse masina ressursihaldur juurutamise mudeli."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Valige Windows virtuaalse masina pilte Azure PowerShelli või CLI

Selles teemas kirjeldatakse, kuidas leida VM pilt tootjad, pakkumised, SKU-de jaoks ja versioonide iga koht, kuhu võib juurutamist. Näiteks on mõned sagedamini kasutatavad Windows VM pildid.

## <a name="table-of-commonly-used-windows-images"></a>Tabeli levinumaid Windowsi pilte


| PublisherName                        | Pakkumine                                 | SKU-ga                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Ettevõtte-optimeeritud-jaoks-DW      |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Ettevõtte-optimeeritud-jaoks-OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 – R2-andmekeskusega                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 – andmekeskusega               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2-SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows Serveri-tehniline eelvaade |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
