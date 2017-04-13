<properties 
   pageTitle="Desaktiveerimine ja kustutamine StorSimple virtuaalse massiiv | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse esmalt desaktiveerimist ja seejärel kustutades selle teenuse StorSimple seadme eemaldamine."
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
   ms.date="06/20/2016"
   ms.author="alkohli" />

# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Desaktiveerimine ja kustutamine StorSimple virtuaalse massiiv

## <a name="overview"></a>Ülevaade

Väljalülitamiseks StorSimple virtuaalse massiiv, siis katkestab seade ja vastava StorSimple halduri teenuse vahelise ühenduse. Desaktiveerimine on püsivate toiming ja ei saa tagasi võtta. Desaktiveeritud seade ei saa registreeritud StorSimple halduri teenuse uuesti.

Võimalik, et peate desaktiveerimine ja kustutamine StorSimple virtuaalne seadme järgmistel juhtudel:


- Teie seade on võrgus ja kavatsete kasutada kasutatav seade. Kui peate seda teha, kui kavatsete versioonile suuremate seade. Pärast seadet andmete ja selle Tõrkesiirde on lõpule jõudnud, saate kustutada seejärel seade.

- Teie seade on ühenduseta ja kavatsete kasutada kasutatav seade. See võib juhtuda suurõnnetuse, kus on elektrikatkestus andmekeskuses, tõttu oma põhiseade on alla. Plaanite kasutada seadme teisene seade. Pärast seadet andmete ja selle Tõrkesiirde on lõpule jõudnud, saate kustutada seadme.

- Kui soovite eemaldada seade ja kustutage see. 
 

Kui seadme väljalülitamiseks kõik andmed, mis on talletatud kohalik pole enam juurdepääsetav. Saab taastada ainult pilves talletatud andmed. Kui plaanite desaktiveerimine pärast seadet andmeid hoida, siis võtke cloud kõigi andmete hetktõmmist enne on seadmes inaktiveerida. See võimaldab teil kõik andmed hiljem taastada.


Selles õppeteemas selgitatakse, kuidas:

- Seadme desaktiveerimine 
- Desaktiveeritud seadme kustutamine


## <a name="deactivate-a-device"></a>Seadme desaktiveerimine

Teha seadme väljalülitamiseks tehke järgmist.

#### <a name="to-deactivate-the-device"></a>Väljalülitamiseks seadme   

1. Minge lehele **seadmed** . Valige seade, mida soovite.

    ![Valige seade, et desaktiveerimine](./media/storsimple-ova-deactivate-and-delete-device/deactivate1m.png)

3. Klõpsake lehe allosas nuppu **Desaktiveeri**.

    ![Klõpsake nuppu Desaktiveeri.](./media/storsimple-ova-deactivate-and-delete-device/deactivate2m.png)

4. Kuvatakse kinnitusteade. Klõpsake nuppu **Jah** jätkata. 

    ![Kinnitage desaktiveerimine](./media/storsimple-ova-deactivate-and-delete-device/deactivate3m.png)

    Desaktiveeri protsessi käivitub ja kuluda mitu minutit.

    ![Poolelioleva desaktiveerimine](./media/storsimple-ova-deactivate-and-delete-device/deactivate4m.png)

3. Pärast desaktiveerimine, värskendatakse seadmete loend. 

    ![Täielik desaktiveerimine](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

    Nüüd saate kustutada selle seadme. 

## <a name="delete-the-device"></a>Seadme kustutamine

Seade on selleks, et kustutada see esmalt välja lülitada. Seadme kustutamine eemaldab selle teenuse ühendatud seadmete loend. Teenuse seejärel kustutatud seadme enam haldamine. Seadmega seotud andmed jäävad siiski pilveteenuses. Pange tähele, et neid andmeid siis hinnangul kulud. 

Täitke seadme kustutamiseks tehke järgmist.

#### <a name="to-delete-the-device"></a>Seadme kustutamine 

 1. Valige lehel StorSimple halduri teenuse **seadmed** desaktiveeritakse seade, mida soovite kustutada.

    ![Valige seade, kustutamine](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

 2. Klõpsake lehe allosas nuppu **Kustuta**.
 
    ![Klõpsake nuppu Kustuta](./media/storsimple-ova-deactivate-and-delete-device/deactivate6m.png)

 3. Teil palutakse kinnitust. Tippige seade seadme kustutamise kinnitamiseks. Pange tähele, et seade pole kustutamisel kustutatakse seadme seotud andmete pilve. Klõpsake ikooni Otsi jätkata.
 
    ![Kustutamise kinnitamine](./media/storsimple-ova-deactivate-and-delete-device/deactivate7m.png) 

 5. Võib kuluda mõni minut seadme välja jätta. 

    ![Poolelioleva kustutamine](./media/storsimple-ova-deactivate-and-delete-device/deactivate8m.png)

    Pärast seadet on kustutatud, seadmete loendisse värskendatakse.

    ![Täielik kustutamine](./media/storsimple-ova-deactivate-and-delete-device/deactivate9m.png)


## <a name="next-steps"></a>Järgmised sammud

- StorSimple halduri teenuse kasutamise kohta lisateavet, avage [StorSimple halduri teenust hallata oma StorSimple virtuaalse massiivi](storsimple-ova-manager-service-administration.md)kasutada. 