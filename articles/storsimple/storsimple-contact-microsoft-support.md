<properties 
   pageTitle="Pöörduge Microsofti klienditoe poole | Microsoft Azure'i"
   description="Saate teada, kuidas luua esitada tugiteenuse taotluse ja StorSimple seadme tugi seansi käivitamine."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Pöörduge Microsofti klienditoe poole

Kui teil tekib probleeme oma Microsoft Azure'i StorSimple lahendusega, saate luua tehnilise toe saamiseks teenusetaotluse. Online seansi oma tugiteeninduse, teil võib-olla ka vaja StorSimple seadme tugi seansi käivitamine. Selles artiklis tutvustatakse:

- Kuidas luua esitada tugiteenuse taotluse.
- Teavet Windows PowerShelli liideses StorSimple seadme tugi seansi käivitamine.

Vaadake üle [StorSimple 8000 sarja tugi SLAs ja teavet](https://msdn.microsoft.com/library/mt433077.aspx) enne loomist esitada tugiteenuse taotluse.

## <a name="create-a-support-request"></a>Esitada tugiteenuse taotluse loomine

Järgmiste toimingute esitada tugiteenuse taotluse loomiseks:

#### <a name="to-create-a-support-request"></a>Kui soovite esitada tugiteenuse taotluse loomine

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com/)parempoolses ülanurgas, klõpsake oma konto nime ja klõpsake **Pöörduge Microsofti tootetoe poole**.

    ![Kontakti MS tugi ManagementPortal kaudu](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Teid suunatakse ümber uue Azure portaali (portal.azure.com). Klõpsake paani **Uus tugiteenuste taotlus** .

    ![Kontakti MS tugi uue portaali kaudu](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Kuva paremas servas kuvatakse paan **Uus tugiteenuste taotlus** . 

    ![Koosolekukutse paan tugi](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. Dialoogiboksis **põhitõed** tehke järgmist:                                
    1. Valige ripploendi **probleemi tüüp** : **tehniline**.
    2. Valige ripploendist soovitud **tellimuse** .
    3. Valige ripploendist **Service** **StorSimple**. 
    4. Valige loendist drop-down **toe kavandamine** . Teil on vaja makstud toe kavandamine lubamiseks tehnilise toe poole.

4. Klõpsake nuppu **edasi**. Kuvatakse dialoogiboks **probleemi** .

    ![Koosolekukutse paan tugi](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. Dialoogiboksis **probleemi** tehke järgmist:

    1.  Valige loendist drop-down **raskusaste** tase.
    2.  Valige loendist drop-down **probleemi tüüp** .
    3.  Valige rippmenüüst **kategooria** . 
    4.  Klõpsake väljal **üksikasjad** lühidalt teie probleemi.
    5.  **Aja jooksul** väljale märkida kuupäev, kellaaeg ja ajavöönd, mis vastab teie probleemi viimase esinemiskord.
    6.  Klõpsake **faili üles laadida**, liikuge sirvides oma tugiteenuste pakett kaustaikooni.
    7.  Märkige ruut **ühiskasutus diagnostikateave** .

6. Klõpsake nuppu **edasi**. Kuvatakse dialoogiboks **Kontaktteave** .

    ![Koosolekukutse paan tugi](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Sisestage oma kontaktteave ja valige kontakt meetodi (telefoni või meili). 

8. Märkige ruut **Salvesta kontakt muudatused tulevaste tugi taotluste** .

9. Klõpsake nuppu **Loo**.

Kui olete saatnud teie taotlus, toe teiega ühendust nii kiiresti kui võimalik, et teie taotlus jätkata.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Tugiteenuste seansi käivitamine Windows PowerShelli StorSimple

Tõrkeotsingu probleeme, mis võivad ilmneda StorSimple seadmega, peate Microsoft Support team suhelda. Microsoft Support võib tekkida vajadus tugi seansi abil saate oma seadmesse sisse logida. 

Tehke tugi seansi käivitamine toimige järgmiselt.

#### <a name="to-start-a-support-session"></a>Tugiteenuste seansi käivitamine

1. Juurdepääs seade otse järjestikune konsooli abil või Telneti seansi kaugarvutist kaudu. Selle tegemiseks järgige [kasutamine](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)Putty ühenduse seadme järjestikune konsooli.

2. Seansi jooksul, mis avab, vajutage klahvi **Enter** saada Käsuviip.

3. Järjestikune konsooli menüüs suvand 1, **logige sisse täielik juurdepääs**.

4. Vastava viiba kuvamisel tippige järgmine parool: 

    `Password1`

5. Vastava viiba kuvamisel tippige järgmine käsk:

    `Enable-HcsSupportAccess`

6. Teile esitatakse on krüptitud string. Kopeerige see string tekstiredaktoris, nt Notepadis.

7. Salvestage see string ja saata meilisõnumi Microsoft Support. 

> [AZURE.IMPORTANT] Saate keelata tugi Accessi käivitades `Disable-HcsSupportAccess`. StorSimple seadme proovib 8 tundi pärast seansi käivitati tugi juurdepääsu keelamiseks. See on parim vahetama mandaat StorSimple seadme tugi seansi algatamine.
