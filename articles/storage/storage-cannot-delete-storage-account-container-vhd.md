<properties
    pageTitle="Tõrkeotsing kustutamine Azure salvestusruumi kontod, ümbriste või VHDs klassikaline juurutuse | Microsoft Azure'i"
    description="Tõrkeotsing: Azure'i salvestusruumi kontod, ümbriste või VHDs kustutamine klassikaline juurutamine"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Tõrkeotsing: Azure'i salvestusruumi kontod, ümbriste või VHDs kustutamine klassikaline juurutamine

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Tõrgete võidakse kuvada, kui proovite kustutada Azure storage konto, container või VHD [Azure portaali](https://portal.azure.com/) või [Azure klassikaline portaalis](https://manage.windowsazure.com/). Probleemid, mis võib olla põhjustatud järgmistest asjaoludest:

-   Kui kustutate VM, ketas ja VHD ei kustutatakse automaatselt. Mis võib olla salvestusruumi konto kustutamise nurjumise põhjust. Me ei kustutaks ketas, et saate ühendada mõne muu VM ketas.

-   On veel rendilepingu plaadil või bloobimälu, mis on seotud ketas.

Kui käesolevas artiklis ei käsitleta Azure probleemi, külastage [MSDN-i ja virnas ületäitumise](https://azure.microsoft.com/support/forums/)Azure foorumeid. Saate postitada oma probleemi foorumites või juurde @AzureSupport Twitter. Samuti saate faili Azure'i tugiteenuse taotluse, valides **tootetugi** [Azure tugiteenuste](https://azure.microsoft.com/support/options/) sait.

## <a name="symptoms"></a>Sümptomid

Järgmises jaotises on loetletud levinumad vead, mis võidakse kuvada, kui proovite kustutada Azure salvestusruumi kontod, ümbriste või VHDs.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Stsenaarium 1: Ei saa kustutada salvestusruumi konto

Kui liigute [Azure portaali](https://portal.azure.com/) või [Azure klassikaline portaal](https://manage.windowsazure.com/) ja valige **Kustuta**salvestusruumi konto, saate võidakse kuvada järgmine tõrketeade:

*Salvestusruumi konto StorageAccountName sisaldab VM pilte. Veenduge, et neid pilte VM eemaldatakse enne selle salvestusruumi konto kustutamine.*

Lisaks võidakse kuvada see tõrge.

**Klõpsake Azure portaali**:

*Salvestusruumi konto < vm-salvestusruumi-kontonimi > kustutamine nurjus. Ei saa kustutada salvestusruumi konto < vm-salvestusruumi-kontonimi >: "salvestusruumi konto < vm-salvestusruumi-kontonimi > on mõned aktiivne pilt või pildid ja/või kontrollimiseks. Veenduge, et need pildid ja/või kontrollimiseks eemaldatakse enne kustutamist selle konto salvestusruumi. ".*

**Klõpsake Azure klassikaline portaali**:

*Salvestusruumi konto < vm-salvestusruumi-kontonimi > on mõned aktiivne pilt või pildid ja/või kontrollimiseks, nt xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Veenduge, et need pildid ja/või kontrollimiseks eemaldatakse enne selle salvestusruumi konto kustutamine.*

Või

**Klõpsake Azure portaali**:

*Salvestusruumi konto < vm-salvestusruumi-kontonimi > on 1 konteineri(te), mis on aktiivne pilt ja/või ketas esemeid. Veenduge, et need esemeid eemaldatakse pilt hoidla enne kustutamist selle salvestusruumi konto*.

**Klõpsake Azure klassikaline portaali**:

*Edastamine nurjus salvestusruumi konto < vm-salvestusruumi-kontonimi > on 1 konteineri(te), mis on aktiivne pilt ja/või ketas esemeid. Veenduge, et need esemeid eemaldatakse pilt hoidla enne selle salvestusruumi konto kustutamine. Kui proovite kustutada salvestusruumi konto ja on endiselt aktiivne ketast sellega seotud, näete teadet, on aktiivne ketast, mida soovite kustutada*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Stsenaarium 2: Ei saa kustutada ümbris

Kui proovite kustutada salvestusruumi, võidakse kuvada järgmine tõrketeade:

Container salvestusruumi kustutamine nurjus * <container name>. Tõrge: "pakendil on praegu rent ja üürilepingu ID on määratud taotluse*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Stsenaarium 3: Ei saa kustutada on VHD

Kui kustutate VM ja proovige uuesti kustutamine plekid seotud VHDs, võib kuvatakse järgmine teade:

*Bloobimälu kustutamine nurjus "tee / [kuupäev]-[kuupäev]-os-1447379084699.vhd'. Tõrge: "on praegu rent, klõpsake soovitud bloobimälu ja üürilepingu ID on määratud taotluse.*

## <a name="solution"></a>Lahendus
Levinumate probleemide lahendamiseks proovida järgmisel viisil:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Samm 1: Kustutada kõik OS ketast, mis ei lase salvestusruumi konto, container või VHD kustutamine

1. Aktiveerige [Azure'i klassikaline portaalis](https://manage.windowsazure.com/).
2. Valige **VIRTUAALSE masina** > **KETAST**.

    ![Klõpsake klassikaline portaalis Azure'i virtuaalmasinates ketast pilt.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Otsige üles ketast, mis on seotud salvestusruumi konto, container või VHD, mille soovite kustutada. Kui märgite ruudu asukohta ketas, leiate seotud salvestusruumi konto, container või VHD.

    ![Pilt, mis näitab asukoha teave ketast Azure klassikaline portaalis](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Veenduge, et pole VM on loetletud **lisatud** soovitud ketast välja ja seejärel kustutage soovitud ketast.

    > [AZURE.NOTE] Kui ketas on ühendatud VM, ei saa kustutada. Kustutatud VM ketast asünkroonselt on lahti. See võib võtta mõne minuti pärast VM kustutatakse selle välja selgitada.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Samm 2: Kustutada mis tahes VM pilte, mis ei lase salvestusruumi konto või container kustutamine

1. Aktiveerige [Azure'i klassikaline portaalis](https://manage.windowsazure.com/).
2. Valige **VIRTUAALSE masina** > **pilte**, ja seejärel kustutage pilte, mis on seotud salvestusruumi konto, container või VHD.

    Pärast seda, proovige uuesti salvestusruumi konto, container või VHD kustutada.

> [AZURE.WARNING] Kindlasti midagi, mida soovite salvestada enne konto kustutamist varundada. Ei saa taastada kustutatud salvestusruumi konto või mis tahes sisu, mis sisaldab see enne kustutamist tuua. See kehtib ressursse konto: Kui kustutate VHD, bloobimälu, tabel, järjekorda või fail, kustutatakse jäädavalt. Veenduge, et ressursi ei kasuta.

## <a name="about-the-stopped-deallocated-status"></a>Peatatud (deallocated) oleku kohta

VMs loodud klassikaline juurutamise mudeli ja mis säilitatakse on **peatatud (deallocated)** oleku [Azure portaali](https://portal.azure.com/) või [Azure klassikaline portaalis](https://manage.windowsazure.com/).

**Azure'i klassikaline portaali**:

![Peatatud (Deallocated) olek vms Azure'i portaalis.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure'i portaalis**:

![Peatatud (deallocated) olek vms Azure'i klassikaline portaalis.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

"Tasuda (deallocated)" olek välja arvuti ressursse, nt CPU, mälu ja võrgu. Ketast, aga endiselt säilitatakse nii, et saate kiiresti uuesti luua VM vajaduse korral. Ketast luuakse VHDs, mis on tagatud Azure storage peal. Salvestusruumi konto on need VHDs ja selle ketast on liisingute nende VHDs.

## <a name="next-steps"></a>Järgmised sammud

- [Salvestusruumi konto kustutamine](storage-create-storage-account.md#delete-a-storage-account)
- [Kuidas murda lukustatud rentida, bloobimälu Microsoft Azure (PowerShelli)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
