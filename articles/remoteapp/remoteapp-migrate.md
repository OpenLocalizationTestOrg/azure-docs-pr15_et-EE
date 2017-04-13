
<properties
    pageTitle="Kasutaja andmeid migreerida Azure RemoteApp | Microsoft Azure'i"
    description="Saate teada, kuidas oma kasutaja andmeid ja sealt Azure RemoteApp migreerida."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Kuidas migreerida andmed ja sealt Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Saate [kasutaja andmete](remoteapp-upd.md) edastamiseks ja sealt Azure RemoteApp mitme erineva tööriista ja võimalused. Siin on paar meetodit.

- Kopeerige ja kleepige lõikelauale ühiskasutus
- Failide ja andmete kopeerimiseks failiserverisse
- OneDrive for Business failide kopeerimine brauseri kaudu
- Failide kopeerimine ümbersuunamine abil

>[AZURE.NOTE] 
> Ei saa lubada OneDrive for Business või tarbija sünkroonimine agentide - need Azure RemoteApp [ei toetata](remoteapp-onedrive.md) .

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopeerimise ja kleepimise File Exploreris

Kopeeri ja Kleebi abil lõikelaua on lubatud RemoteApp juurutuste [vaikimisi](remoteapp-redirection.md). See võimaldab kasutajatel, kohaliku arvuti ja RemoteApp rakenduste vahel faile kopeerida. Sageli, kuni tavapärase RemoteApp rakenduste kasutamine, kasutajate salvestatud failid oma UPDs - teisaldada, et andmed välja RemoteApp on lihtne:

1. [Avaldamine File Exploreri rakenduse](remoteapp-publish.md) RemoteApp kogum. (Pange tähele, et see on haldustoimingut.)
2. Otse kasutajate avaldatud File Exploreri rakenduse käivitamiseks ja mis kopeerida ja kleepida faile nii oma UPD ja sealt seda kasutama.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Failide ja andmete üleslaadimine failiserverisse standard võrgu faili koopia abil

Sageli ettevõtted kasutada serverites üldine andmete talletamiseks. Kui teate, et serveri nime või asukohta, saate kasutajate kohalikus võrgus otsige server ja kopeerige oma faile, sarnaselt nad ei kohal. Uuesti peaksite avaldamine RemoteApp File Explorer ja kasutajatega ühiskasutatava.

>[AZURE.NOTE] 
> Faili server tuleb marsruuditavaid võrgus, mis RemoteApp võeti kasutusele võtta.

## <a name="copy-files-to-onedrive-for-business"></a>OneDrive for Business failide kopeerimine
Kuigi te ei saa luba OneDrive for Businessi sünkroonimise agent RemoteApp, saate siiski kopeerida faile oma UPD teenusesse OneDrive for Business brauseri kaudu. 

1. Avaldamine RemoteApp File Explorer ja seejärel Paluge kasutajatel failidele juurde pääseda rakenduse kaudu. 
2. See on lihtsam faile juhul, kui need on tihendatud, et kasutajad peaksid luua ZIP-faili, mis sisaldab kõiki faile, mis on OneDrive for Businessi liikumiseks.
3. Paluge kasutajatel Office 365 portaalis, ja seejärel avage OneDrive ja ZIP-faili üleslaadimine.

## <a name="copy-files-by-using-drive-redirection"></a>Failide kopeerimine ketas ümbersuunamine abil

Kui olete lubanud [draivi ümbersuunamine](remoteapp-redirection.md), olete juba loonud Vastendatud draivi kasutajate jaoks. Sel juhul nad oma faile ümber kettadraivi zip ja seejärel salvestage need oma kohaliku arvuti.