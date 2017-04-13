<properties
    pageTitle="Manustamine kettal VM | Microsoft Azure'i"
    description="Lisada andmed kettale Windows virtuaalse masina loodud mudeliga klassikaline juurutamise ja lähtestage see."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Windowsi virtuaalse masina loodud klassikaline juurutamise mudeli andmed kettale manustamiseks

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Kui soovite kasutada uut portaali, vaadake, [Kuidas lisada VM Windows Azure'i portaalis kettal andmeid](virtual-machines-windows-attach-disk-portal.md).

Kui teil on vaja mõnda täiendavad andmed kettale, saate manustada mõne tühjale kettapuhastusriista või olemasolevat ketast andmetega VM. Mõlemal juhul on soovitud ketast .vhd faile, mis asu Azure storage konto. Puhul uue ketta, kui manustate ketas, samuti peate lähtestage see nii, et see on Windows VM kasutamiseks valmis.

Ketast kohta leiate lisateavet teemast [kohta ketast ja VHDs Virtuaalmasinates jaoks](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Selle ketta lähtestamine

1. Ühenduse, virtuaalse masina. Juhised leiate teemast [Windows Server töötab sisselogimiseks][logon].

2. Pärast sisselogimist virtuaalse masina, avage **Serveri haldur**. Valige vasakul paanil **faili- ja salvestusruumi teenused**.

    ![Avatud Server Manager](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Laiendage menüü ja valige **ketast**.

4. **Ketast** jaotises loendid soovitud ketast. Enamikul juhtudel on ketas 0, kettale 1 ja 2 ketas. Ketas 0 on operatsioonisüsteem ketas, ketas 1 on ajutine kettale ja ketas 2 on andmed kettale äsja lisatud VM. Andmete uue ketta loetletakse sektsiooni nimega **teadmata**. Paremklõpsake ketta ja valige **lähtestada**.

5.  Olete teavitada, et kustutatakse kõik andmed, kui ketas on lähtestatud. Klõpsake nuppu **Jah** Pange tähele kuvatavat hoiatust ja ketas lähtestada. Kui olete lõpetanud, kuvatakse Partion loetletud **GPT**. Paremklõpsake ketast uuesti ja valige **Uus draiv**.

6.  Täitke viisardi abil sätte vaikeväärtust. Kui viisard on lõpule jõudnud, on loetletud **mahud** jaotis uus draiv. Ketas on nüüd võrgus ja kasutusvalmis andmete talletamiseks.

    ![Helitugevuse lähtestatud](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Suuruse VM määratleb mitu ketast, saate selle manustada. Lisateavet leiate teemast [virtuaalmasinates suurused](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Lisaressursid

[Kuidas eemaldada arvutist Windows virtual kettal](virtual-machines-windows-classic-detach-disk.md)

[Ketast ja VHDs jaoks virtuaalmasinates kohta](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
