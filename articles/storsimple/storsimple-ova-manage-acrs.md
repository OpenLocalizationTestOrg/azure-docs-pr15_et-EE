<properties 
   pageTitle="Juurdepääsu juhtimine kirjeid hallata StorSimple virtuaalse massiivi | Microsoft Azure'i"
   description="Kirjeldab, kuidas hallata juurdepääsu juhtimine kirjete (ACRs) määramiseks, mis hosts saate luua ühenduse StorSimple virtuaalse massiivi maht."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Juurdepääsu juhtimine kirjeid hallata StorSimple virtuaalse massiivi StorSimple halduri teenuse abil 

## <a name="overview"></a>Ülevaade

Juurdepääsu juhtimine kirjete (ACRs) abil saate määrata, mis hosts saate luua ühenduse draivi StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seade). ACRs on määratud teatud helitugevust ja sisaldavad iSCSI kvalifitseeritud veerunimede (IQNs) hosts. Kui host proovib maht, seadme kontrollib iga-aastase seostatud IQN nime, et maht, ja kui vaste puudub, siis ühendus on loodud. **Juurdepääsu juhtimine kirjete** jaotis lehe **konfigureerimine** kuvatakse kõik Accessi juhtelemendi kirjed, koos vastavate IQNs hosts.

Selle õpetuse selgitab levinud ACR seotud järgmist:

- Funktsiooni IQN hankimine
- Accessi juhtelement kirje lisamine 
- Accessi juhtelement kirje redigeerimiseks 
- Accessi juhtelement kirje kustutamine 

> [AZURE.IMPORTANT] 
> 
> - Kui ka ACR määramisel maht, olge ettevaatlik, et maht ei ole samaaegselt kättesaadav rohkem kui üks-rühmitatud host kuna see võib rikutud helitugevuse. 
> - Kui kustutate mõne ACR maht, veenduge, et vastava host on juurdepääs maht, kuna kustutamise võib kaasa tuua lugemis-ja kirjutamisõigusega häirete.

## <a name="get-the-iqn"></a>Funktsiooni IQN hankimine

Järgmiste toimingute saada IQN Windows hosti, kus töötab Windows Server 2012.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Lisage soovitud ACR

StorSimple halduri teenuse **konfiguratsiooni** lehele saate lisada ACRs. Tavaliselt on üks ACR ühele seostada.

Lisateabe saamiseks seostamise mis ACR mahuga, minge [lisatakse](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Kui ka ACR määramisel maht, olge ettevaatlik, et maht ei ole samaaegselt kättesaadav rohkem kui üks-rühmitatud host kuna see võib rikutud helitugevuse.
 
Järgmiste toimingute lisamiseks mõne ACR.

#### <a name="to-add-an-acr"></a>Lisada mõne ACR

1. Klõpsake teenuse sihtleht, valige teenust, topeltklõpsake soovitud teenuse nime ja seejärel vahekaarti **konfigureerimine** .

    ![vahekaarti konfigureerimine](./media/storsimple-ova-manage-acrs/acr1.png)

2. Klõpsake tabeli kirjet jaotises **juurdepääsu juhtimine kirjed**, esitama oma ACR **nimi** .

3. Sisestage jaotises **iSCSI algataja nimi**, IQN oma Windows hosti nimi. 

4. Klõpsake nuppu **Salvesta** lehe allosas vastloodud iga-aastase salvestamiseks. Kuvatakse järgmine kinnitusteade.

    ![kinnitusteate](./media/storsimple-ova-manage-acrs/acr2.png)

5. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-manage-acrs/check-icon.png). Tabeli kirjet värskendatakse vastavalt selle peale.

## <a name="edit-an-acr"></a>Mõne ACR redigeerimine

Saate lehel **konfigureerimine** Azure klassikaline portaalis ACRs redigeerimine. 

> [AZURE.NOTE] Muutke ainult ACRs, mis pole praegu kasutusel. On seostatud maht, mis on praegu kasutusel ACR redigeerimiseks esmalt võtke helitugevuse ühenduseta.

Järgmiste toimingute redigeerimiseks on ACR.

#### <a name="to-edit-an-acr"></a>Mõne ACR redigeerimiseks

1. Klõpsake teenuse sihtleht, valige teenust, topeltklõpsake soovitud teenuse nime ja seejärel vahekaarti **konfigureerimine** .

2. Tabeli kirjet juurdepääsu juhtimine kirjeid, viige kursor iga-aastase, mida soovite muuta.

3. Uus nimi ja/või IQN esitama iga-aastase.

4. Klõpsake nuppu **Salvesta** lehe allosas muudetud iga-aastase salvestamiseks. Kuvatakse kinnitusteade. 

5. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-manage-acrs/check-icon.png). Tabeli kirjet, värskendatakse selle muudatuse kajastamiseks.

## <a name="delete-an-access-control-record"></a>Accessi juhtelement kirje kustutamine

Saate lehel **konfigureerimine** Azure klassikaline portaalis ACRs kustutada. 

> [AZURE.NOTE] 
> 
> - Tuleks kustutada ainult ACRs, mis pole praegu kasutusel. On seostatud maht, mis on praegu kasutusel ACR kustutamiseks esmalt võtke helitugevuse ühenduseta.
> - Kui kustutate mõne ACR maht, veenduge, et vastava host on juurdepääs helitugevuse, kuna kustutamise võib kaasa tuua lugemis-ja kirjutamisõigusega häirete.

Järgmiste toimingute juurdepääsu juhtelement kirje.

#### <a name="to-delete-an-access-control-record"></a>Kirje juurdepääsu juhtimine

1. Klõpsake teenuse sihtleht, valige teenust, topeltklõpsake soovitud teenuse nime ja seejärel vahekaarti **konfigureerimine** .

2. Selle tabeli kirjet juurdepääsu juhtimine kirjete (ACRs), viige kursor iga-aastase, mida soovite kustutada.

3. Äärmiselt paremas veerus kuvatakse iga-aastase diagrammitüüpide jaoks ikooni Kustuta (**x**). Iga-aastase kustutamiseks ikooni **x** . Kuvatakse järgmine kinnitusteade.

    ![kinnitusteate](./media/storsimple-ova-manage-acrs/acr3.png)

5. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-manage-acrs/check-icon.png). Tabeli kirjet värskendatakse kustutamise kajastamiseks.

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [lisamise mahud ja ACRs konfigureerimise](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume)kohta.
