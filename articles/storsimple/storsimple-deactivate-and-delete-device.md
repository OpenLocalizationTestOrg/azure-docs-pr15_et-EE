<properties 
   pageTitle="Desaktiveerimine ja kustutamine StorSimple seade | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse esmalt desaktiveerimist ja seejärel kustutades selle teenuse StorSimple seadme eemaldamine."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Desaktiveerimine ja kustutamine StorSimple seadme

## <a name="overview"></a>Ülevaade

Soovi korral võite võtta StorSimple seadme välja (näiteks siis, kui olete asendamine või seadme üleminekut või kui kasutate enam StorSimple). Kui see on nii, peate väljalülitamiseks seadme, enne kui saate selle kustutada. Desaktiveerides severs seade ja vastava StorSimple halduri teenuse vahelise ühenduse. Selle õpetuse selgitab, kuidas eemaldada StorSimple seadme esimese desaktiveerimist ja seejärel kustutades selle teenuse. 

Kui seadme väljalülitamiseks kõik andmed, mis on talletatud kohalik seadmes pole enam juurdepääsetav. Saab taastada ainult seostatud seadme pilves talletatud andmed.  

>[AZURE.WARNING] Desaktiveerimine on püsivate toiming ja ei saa tagasi võtta. Juhul, kui see on esimene lähtestada selle factory ei saa desaktiveeritakse seadme StorSimple halduri teenuse registreerida. 
>
>Funktsiooni factory lähtestada protsess kustutab kõik andmed, mis on talletatud kohalik seadmes. Seetõttu on oluline kõigi andmete pilve hetktõmmis teha enne on seadmes inaktiveerida. See võimaldab teil kõik andmed hiljem taastada.

Selles õppeteemas selgitatakse, kuidas:

- Desaktiveerige seadme ja selle andmete kustutamine
- Desaktiveerige seadme ja andmete säilitamise

See samuti selgitatakse, kuidas desaktiveerimine ja kustutamine StorSimple virtuaalse seade töötab.

>[AZURE.NOTE] Enne StorSimple füüsilise või virtuaalse seadme desaktiveerimist veenduge, et peatada või kustutada kliendid ja hakkab majutama, mis sõltuvad sellest, et seade.

## <a name="deactivate-and-delete-data"></a>Desaktiveerige ja kustutavad andmeid

Kui huvi seadme täielikult kustutada ja ei soovi säilitada seadme andmeid, siis tehke järgmist.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Väljalülitamiseks seadme ja selle andmete kustutamine  

1. Enne desaktiveerides seadme, siis kustutage kõik helitugevuse seadmega seotud ümbriste (ja mahud). Helitugevuse ümbriste saate kustutada ainult siis, kui olete kustutanud seotud varukoopiad.

2. Väljalülitamiseks seadme järgmiselt:

    1. Klõpsake lehel StorSimple halduri teenuse **seadmed** valige seade, mida soovite desaktiveerida ja klõpsake lehe allosas nuppu **Desaktiveeri**.

    2. Kuvatakse kinnitusteade. Klõpsake nuppu **Jah** jätkata. Desaktiveeri protsessi käivitub ja kuluda mitu minutit.

3. Pärast desaktiveerimine, saate seadme täielikult kustutada. Seadme kustutamine eemaldab selle teenuse ühendatud seadmete loend. Teenuse seejärel kustutatud seadme enam haldamine. Järgmiste juhiste abil saate kustutada seadme:

    1. Valige lehel StorSimple halduri teenuse **seadmed** desaktiveeritakse seade, mida soovite kustutada.

    2. Klõpsake lehe allosas nuppu **Kustuta**.

    3. Teil palutakse kinnitust. Klõpsake nuppu **Jah** jätkata.

    Võib kuluda mõni minut seadme välja jätta.

## <a name="deactivate-and-retain-data"></a>Desaktiveerige ja andmete säilitamise

Kui olete huvitatud seade, kuid soovite säilitada andmed kustutada, siis tehke järgmist.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Seadme inaktiveerida ja andmete säilitamise 

1. Seadme inaktiveerida. Säilitatakse kõik helitugevuse ümbriste ja seadme pilte.

    1. Klõpsake lehel StorSimple halduri teenuse **seadmed** valige seade, mida soovite desaktiveerida ja klõpsake lehe allosas nuppu **Desaktiveeri**.

    2. Kuvatakse kinnitusteade. Klõpsake nuppu **Jah** jätkata. Desaktiveeri protsessi käivitub ja kuluda mitu minutit.

2. Te saate nüüd ei õnnestu üle helitugevuse ümbriste ja seotud pilte. Toimingute, avage [seadme StorSimple Tõrkesiirde ja Avariijärgne taaste](storsimple-device-failover-disaster-recovery.md).

3. Pärast desaktiveerimine ja Tõrkesiirde, saate seadme täielikult kustutada. Seadme kustutamine eemaldab selle teenuse ühendatud seadmete loend. Teenuse seejärel kustutatud seadme enam haldamine. Täitke seadme kustutamiseks tehke järgmist.
 
    1. Valige lehel StorSimple halduri teenuse **seadmed** desaktiveeritakse seade, mida soovite kustutada.

    2. Klõpsake lehe allosas nuppu **Kustuta**.

    3. Teil palutakse kinnitust. Klõpsake nuppu **Jah** jätkata.

    Võib kuluda mõni minut seadme välja jätta.

## <a name="deactivate-and-delete-a-virtual-device"></a>Desaktiveerimine ja kustutamine virtuaalse seade

StorSimple virtuaalse seadme desaktiveerimine deallocates virtuaalse masina. Seejärel saate kustutada virtuaalse masina ja ressursse, mis on loodud, kui see on ette valmistatud. Pärast virtuaalse seade on välja lülitatud, ei saa seda taastada eelmine olek. 

Desaktiveerimine tulemuste järgmisi toiminguid:

- StorSimple virtuaalse seade on eemaldatud.

- Tavalise nimetusega OSDisk ja andmete ketast StorSimple virtuaalse seadme loodud eemaldatakse.

- Majutatud teenuse ja virtuaalse võrgu, mis on loodud ajal ettevalmistamise säilivad. Kui te ei kasuta need üksused, peaks need käsitsi kustutada.

- Pilveteenuse hetktõmmiseid loodud StorSimple virtuaalse seadme säilitatakse.

## <a name="next-steps"></a>Järgmised sammud
- Taastada desaktiveeritakse seadme algsätted, minge [algsätted seadme lähtestada](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Tehnilise abi saamiseks [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).

- StorSimple halduri teenuse kasutamise kohta lisateabe saamiseks minge [StorSimple halduri teenuse StorSimple seadme haldamiseks kasutada](storsimple-manager-service-administration.md). 
