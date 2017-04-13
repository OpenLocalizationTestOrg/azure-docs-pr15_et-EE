<properties
    pageTitle="Mis on VM skaala määrab? | Microsoft Azure'i"
    description="Lisateavet VM skaala komplektid."
    keywords="Linux virtuaalse masina, virtuaalse masina skaala määrab" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Mis on virtuaalse masina skaala määrab?

Virtuaalse masina skaala komplektid võimaldavad teil hallata mitut VMs kogumina. Kõrge, on skaala komplektid järgmises spetsialistidele ja miinuste.

Spetsialistidele:

1. Kõrge-saadavus. Igas kogumis skaala asetab selle VMs kättesaadavus seadmine 5 viga Domains (FDs) ja 5 värskenduse Domains (UDs), et tagada kättesaadavus (FDs ja UDs, kohta vt [VM-saadavus](./virtual-machines-linux-manage-availability.md)lisateabe saamiseks). 
2. Lihtne integreerimine Azure laadi koormusetasakaalustusteenuse ja rakenduse lüüsi.
3. Lihtne integreerimine Azure Autoscale.
4. Lihtsustatud juurutamise, haldus ja vms puhastamiseks.
5. Toetab levinud Windows ja Linux maitseid kui ka kohandatud pilte.

Miinuste:

1. VM juhtudel skaala kogumis olevate andmete ketast ei saa manustada. Selle asemel kasutada bloobimälu, Azure'i faile, Azure'i tabelite või muude salvestamise lahendus.

## <a name="quick-create-using-azure-cli"></a>Kiire loomine Azure CLI abil

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Järgmised sammud

Üldist teavet, lugege teemat [skaala komplektide peamised sihtleht](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Rohkem dokumente, tutvuge [peamised dokumentatsiooni leht skaala määrab](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Näiteks ressursihaldur mallide abil skaala komplekti, otsige "vmss" [Azure Kiirjuhend Mallid github repo](https://github.com/Azure/azure-quickstart-templates).

