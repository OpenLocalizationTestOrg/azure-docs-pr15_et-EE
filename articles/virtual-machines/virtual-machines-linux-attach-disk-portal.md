<properties
    pageTitle="Andmete kettal manustamiseks Linux VM | Microsoft Azure'i"
    description="Kuidas lisada uusi või olemasolevaid andmeid ketta Linux VM ressursihaldur juurutamise näidise Azure'i portaalis."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Kuidas manustada Linux VM Azure portaalis kettal andmed

Selles artiklis kirjeldatakse, kuidas lisada nii uute ja olemasolevate ketast Linux virtuaalse masina Azure portaali kaudu. Samuti saate [manustada VM Windows Azure'i portaalis kettal andmeid](virtual-machines-windows-attach-disk-portal.md). Enne selle tegemist, vaadake üle järgmisi näpunäiteid.

- Virtuaalse masina suurus juhtelemendid, kui palju andmeid ketast, saate manustada. Lisateavet leiate teemast [virtuaalmasinates suurused](virtual-machines-linux-sizes.md).
- Premium mälu kasutamiseks peate DS-sarja või GS-sarja virtuaalse masina. Saate nii Premium ja Standard salvestusruumi kontodelt ketast nende virtuaalmasinates. Premium mälu on teatud piirkondades saadaval. Lisateavet leiate teemast [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](../storage/storage-premium-storage.md).
- Manustatud virtuaalmasinates ketast on tegelikult .vhd failide Azure storage konto. Lisateavet leiate teemast [ketast ja VHDs jaoks virtuaalmasinates kohta](virtual-machines-linux-about-disks-vhds.md).
- Uue ketta, ei peate esmalt looma, kuna Azure'i loob ühendamisel.
- Olemasolevat ketast, .vhd fail peab olema ka Azure storage konto jaoks saadaval. Saate kasutada .vhd, mis on juba olemas, kui see on seotud teise virtuaalse masina või üles oma .vhd failide salvestusruumi kontole.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Järgmised sammud

Kui ketas on lisatud, peate kasutamiseks ette valmistada. Virtuaalne arvuti opsüsteem näha: "kohta: uute andmete ketta Linux lähtestamine" selles [artiklis](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
