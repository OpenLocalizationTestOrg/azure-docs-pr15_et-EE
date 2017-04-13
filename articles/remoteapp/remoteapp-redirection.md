<properties
    pageTitle="Ümbersuunamise kasutamine Azure RemoteApp | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerimine ja kasutamine ümbersuunamise RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Ümbersuunamise kasutamine Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Seadme ümbersuunamine võimaldab kasutajatel suhelda remote rakenduste abil oma kohalikus arvutis, telefonis või tahvelarvutis manustatud seadmete. Kui teil on Skype'i kaudu Azure RemoteApp, peab teie kasutaja kaamera Skype'i töötamiseks oma arvutisse installitud. See kehtib ka printerid, kõlareid, kuvari ja vahemik, USB-porti ühendatud välisseadmed.

RemoteApp mõjutab Kaugtöölaua protokolli (RDP) ja RemoteFX esitada ümbersuunamist.

## <a name="what-redirection-is-enabled-by-default"></a>Millist ümbersuunamine on vaikimisi?
Kui kasutate RemoteApp, on vaikimisi lubatud järgmised ümbersuunamine. Sulgude teave kuvatakse RDP säte.

- Helisid kohalikus arvutis (**selles arvutis**). (audiomode:i:0)
- Jäädvustada kohaliku arvuti heli ja saata kaugarvuti (**ainult sellest arvutist kirje**). (audiocapturemode:i:1)
- Prindi kohalikud printerid (redirectprinters:i:1)
- COM-pordid (redirectcomports:i:1)
- Kiipkaardi seadme (redirectsmartcards:i:1)
- Lõikelaua (võimalus kopeerimine ja kleepimine) (redirectclipboard:i:1)
- Tühjendage tüüp fondi Eksponentsilumise (luba fondi Eksponentsilumise: i:1)
- Kõigi toetatud seadmete suunata. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Millist ümber suunamine on saadaval?
Vaikimisi on keelatud ümbersuunamise kaks võimalust:

- Draiv ümbersuunamine (draivi vastendamine): kohaliku arvuti draivid muutuvad Kaugseanss vastendatud draivid. See võimaldab salvestatud või teie kohaliku draivid failide avamine Kaugseanss töötamise ajal.
- USB-ümbersuunamine: saate kasutada jooksul Kaugseanss kohaliku arvutiga ühendatud USB-seadmed.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Ümbersuunamise RemoteApp sätete muutmine
Saate muuta seadme ümbersuunamise sätete kogumi, SDK Microsoft Azure'i PowerShelli abil. Pärast uue PowerShelli ja SDK installimist esmalt konfigureerida oma tellimuse haldamiseks, nagu on kirjeldatud [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

Seejärel kohandatud RDP atribuutide seadmine sarnaneb järgmise käsu abil:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Pange tähele, kasutatakse selle *n* eraldaja omaduste vahel).

Milliseid RDP kohandatud atribuudid on konfigureeritud loendi saamiseks käivitage järgmine cmdlet-käsk. Arvestage, et ainult kohandatud atribuudid on kujutatud väljund tulemused ja pole vaikeatribuudid.  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Kui määrate kohandatud atribuudid määrake kõigi kohandatud atribuutide iga kord, kui; muul juhul säte taastatakse keelatud.   

### <a name="common-examples"></a>Levinud näited
Ketas lubamiseks kasutada järgmine cmdlet-käsk:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Selle cmdlet-käsu abil saate lubada USB ja ketas ümbersuunamine:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Selle cmdlet-käsu abil saate lõikelaua ühiskasutuse keelamiseks.  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Veenduge, et täielikult väljalogimiseks kõigi kasutajate kogumine (ja mitte ainult need katkestada) enne katsetasite muutmine. Täielikult logib tagamiseks avage saidikogumi Azure'i portaalis **seansid** vahekaardi ja kõik kasutajad, kellel on katkestatud või sisse logitud välja logima. Mõnikord võib kuluda mitu sekundit kohaliku draivid kuvamiseks Exploreris seansi jooksul.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Teie Windowsi klient USB ümbersuunamise sätete muutmine

Kui soovite USB-ümbersuunamine kasutamine arvutis, mis ühendab RemoteApp, on 2 toiminguid, mis juhtub vaja. 1 - Teie administraator peab olema USB-ümbersuunamine saidikogumi tasemel lubada Azure PowerShelli abil. 2 - on iga seade, kuhu soovite kasutada USB-ümbersuunamine, peate lubama rühmapoliitika, mis lubab seda. Selles etapis tuleb teha iga kasutaja, kes soovib kasutada USB-ümbersuunamine.

> [AZURE.NOTE] USB-ümbersuunamine Azure'i RemoteAppi toetatakse ainult Windows arvutis.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>USB-ümbersuunamine RemoteApp kogumi lubamine
Kasutage lubamiseks USB-ümbersuunamine saidikogumi tasemel järgmine cmdlet-käsk:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Klientarvuti USB lubamiseks

USB ümbersuunamise sätete konfigureerimiseks arvutis järgmist.

1. Avage kohaliku rühmapoliitika redaktori (GPEDIT. MSC). (Käivita gpedit.msc käsureale.)
2. Avage **arvutis Configuration\Policies\Administrative konfiguratsioon\haldusmallid\windowsi Components\Remote töölaua Services\Remote ühendus Client\RemoteFX USB-seadmete ümbersuunamine**.
3. Topeltklõpsake **Luba RDP ümbersuunamine muud toetatud RemoteFX USB-seadmed ainult sellest arvutist**.
4. Valige **lubatud**ja valige **Administraatorid ja kasutajate juurdepääsuõigusi RemoteFX USB ümbersuunamine**.
5. Avage käsuviip administraatori õigustega ja käivitage järgmine käsk:

        gpupdate /force
6. Taaskäivitage arvuti.

Rühmapoliitika haldus tööriista abil saate ka luua ja rakendada USB ümbersuunamine poliitika kõik arvutid domeeni:

1. Selle domeenikontrolleri domeeni administraatorina sisse logida.
2. Avage rühmapoliitika halduskonsool. (Valige **Alustamine > Haldustööriistad > rühma rühmapoliitika haldus**.)
3. Liikuge domeeni või Organisatsiooniüksus, mille jaoks soovite luua poliitika.
4. Paremklõpsake **Domeeni vaikepoliitika**ja klõpsake siis nuppu **Redigeeri**.
5. Avage **arvutis Configuration\Policies\Administrative konfiguratsioon\haldusmallid\windowsi Components\Remote töölaua Services\Remote ühendus Client\RemoteFX USB-seadmete ümbersuunamine**.
6. Topeltklõpsake **Luba RDP ümbersuunamine muud toetatud RemoteFX USB-seadmed ainult sellest arvutist**.
7. Valige **lubatud**ja valige **Administraatorid ja kasutajate juurdepääsuõigusi RemoteFX USB ümbersuunamine**.
8. Klõpsake nuppu **OK**.  
