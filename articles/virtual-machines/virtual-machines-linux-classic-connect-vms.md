<properties
    pageTitle="Luua ühenduse Linux VMs pilveteenus | Microsoft Azure'i"
    description="Ühendage Linux virtuaalmasinates loodud klassikaline juurutamise mudeli Azure pilveteenuses või virtuaalse võrgu."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Linux virtuaalmasinates loodud klassikaline juurutamise mudeli virtuaalse võrgu või pilvepõhises teenusega ühenduse loomine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Klassikaline juurutamise mudeliga loodud Linuxi virtuaalmasinates paigutatakse alati mõnda pilveteenusesse. Pilveteenusesse ümbris toimib ja pakub kordumatu avaliku DNS-i nimi, avaliku IP-aadress ja kogum, virtuaalse masina Interneti kaudu juurdepääsu lõpp-punktid. Pilveteenusesse saab virtuaalse võrgus, kuid mis ei ole nõue. Samuti saate [Windowsi virtuaalmasinates virtuaalse võrgu või pilvepõhises teenusega ühenduse](virtual-machines-windows-classic-connect-vms.md).

Kui mõnda pilveteenusesse pole virtuaalse võrk, seda nimetatakse *autonoomse* pilveteenus. Virtuaalmasinates autonoomse pilveteenuses saate muude virtuaalmasinates suhelda ainult selle muude virtuaalmasinates avaliku DNS-i nimede abil ja liikluse läbib Interneti kaudu. Kui mõnda pilveteenusesse on virtuaalse võrgus, saate virtuaalmasinates, et pilveteenuses kõik muud virtuaalmasinates virtuaalse võrgu suhelda saatmisele mis tahes liikluse Interneti kaudu.

Kui te oma virtuaalmasinates sama autonoomse pilveteenuses, saate siiski kasutada tasakaalustamiseks laadimine ja kättesaadavus komplektid. Lisateavet leiate teemast [koormusetasakaalustuseks virtuaalmasinates](virtual-machines-linux-load-balance.md) ja [haldamine virtuaalmasinates olemasolu](virtual-machines-linux-manage-availability.md). Siiski ei saa korraldada virtuaalmasinates, klõpsake alamvõrku või kohapealse võrgu autonoomse pilveteenus ühenduse. Siin on näide:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Järgmised sammud

Kui olete loonud virtuaalse masina, on mõistlik [lisada andmed kettale](virtual-machines-linux-classic-attach-disk.md) , nii et oma teenustele ja töökoormus on koht, kuhu soovite salvestada andmed. 



