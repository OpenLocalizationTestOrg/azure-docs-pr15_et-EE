<properties 
    pageTitle="Kuidas ümber suunata USB-seadmete Azure RemoteApp? | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada ümbersuunamise Azure RemoteApp USB seadmete jaoks." 
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



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Kuidas ümber suunata USB-seadmete Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Seadme ümbersuunamine võimaldab kasutajatel kasutada oma arvuti või tahvelarvuti Azure RemoteApp rakendustega seotud USB-seadmed. Näiteks kui kaudu Azure RemoteApp Skype'i, teie kasutajad peavad saama kasutada oma seadmes foto.

Enne kui minna edasi, veenduge, et loete USB ümbersuunamise teave [Azure RemoteApp ümbersuunamise kaudu](remoteapp-redirection.md). Samas on soovitatav nusbdevicestoredirect:s: * ei toimi USB Vali veebikaamera ja ei pruugi mõned USB-printerite või USB-mitmeotstarbeline seadmed. Kujundus ja turvalisuse põhjustel Azure RemoteApp administraator on lubamiseks ümbersuunamise seadme klassi GUID või seadme eksemplari ID kasutajad saaks kasutada nende seadmed.

Kuigi selles artiklis kirjeldatakse web kaamera ümbersuunamist, saate suunata USB-printerite ja muud USB-mitmeotstarbeline seadmed, mis pole suunatakse sarnast lähenemisviisi on **nusbdevicestoredirect:s:*** käsk.

## <a name="redirection-options-for-usb-devices"></a>Ümbersuunamise suvandid USB-seadmete jaoks
Azure RemoteApp kasutab USB-seadmete ümbersuunamist kui need, mis on saadaval kaugtöölaua teenuste jaoks üsna sarnased menetlustele. Aluseks oleva tehnoloogia võimaldab teil valida õige ümbersuunamise seadmele saada parimat üksikasjalik ja RemoteFX USB-seadme ümbersuunamine abil soovitud **usbdevicestoredirect:s:** käsk. Nelja elementi, et see käsk on:

| Tellimuse töötlemine | Parameetri           | Kirjeldus                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Valib kõigis seadmetes, mis pole kätte üksikasjalik ümbersuunamist. Märkus: teadlikult * USB Vali veebikaamera ei tööta.  |
|                  | {Seadme klassi GUID} | Valib kõigis seadmetes, mis vastavad määratud seadme häälestamise klassi.                                                           |
|                  | USB\InstanceID      | Valib USB-seadme jaoks antud eksemplari ID-ga.                                                                  |
| 2                | -USB\Instance ID    | Eemaldab määratud seade ümbersuunamise sätted.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Ümbersuunamine USB-seadme abil seadme klassi GUID
On kaks võimalust seadme klassi GUID, mida saab kasutada ümbersuunamise leidmiseks. 

Esimene variant on kasutada [System-Defined seadme häälestamise tunnid saadaval tarnijatele](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Valige ainekursuse soovitule kõige täpsemini vastava kohaliku arvutiga ühendatud seadet. Digi see võib olla mõne seadme Imagingi või klassi videoseadme jäädvustada. Peate tegema mõned katsetamiseks seadme klassi leida õige klassi GUID, mis töötab kohalikult manustatud USB-seade (käesoleval juhul veebikaamera).

Paremini või teine võimalus on konkreetse seadme klassi GUID leidmiseks tehke.

1. Seadmehalduri avamiseks, otsige seadme, mille suunatakse ja paremklõpsake seda ja avage soovitud atribuudid.
![Seadmehalduri avamiseks](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Klõpsake vahekaarti **üksikasjad** , valige atribuudi **Klassi Guid**. Väärtus, mis kuvatakse on klassi GUID selle seadme tüübist.
![Kaamera atribuute](./media/remoteapp-usbredir/ra-classguid.png)
3. Kasutage väärtust klassi Guid seadmetes, mis vastavad selle ümbersuunamiseks.

Näiteks:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Saate ühendada mitu seadet ümbersuunamine sama cmdlet. Näide: kohaliku salvestusruum ja USB veebikaamera ümbersuunamiseks näeb välja umbes järgmine cmdlet-käsk:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Kui seadme ümbersuunamine määratud klassi GUID suunatakse kõigis seadmetes, mis vastavad määratud saidikogumi selle klassi GUID. Näiteks kui mitmes arvutis kohalikus võrgus, millel on sama USB Vali veebikaamera, võite käivitada ühe cmdlet ümbersuunamiseks kõik Vali veebikaamera.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Ümbersuunamine USB-seade seadme eksemplari ID abil

Kui soovite veel kohandatud juhtelemendi ja soovite reguleerida ümbersuunamise seadme kohta, saate kasutada parameetrit **USB\InstanceID** ümbersuunamist.

See meetod raskem osa on leida USB-seadme eksemplari ID. Peate arvuti ja teatud USB-seade. Seejärel tehke järgmist.

1. Luba seadme ümbersuunamine Kaugseanss töölaua rakenduses, nagu on kirjeldatud [Kuidas kasutada seadmeid ja ressursse kaugtöölaua seansi?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Avage soovitud kaugtöölaua ühendus ja klõpsake nuppu **Kuva suvandid**.
3. Klõpsake nuppu **Salvesta nimega** salvestamine praegused ühendussätted RDP-faili.  
    ![RDP-faili sätete salvestamine](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Valige faili nimi ja asukoht, näiteks "MyConnection.rdp" ja "See PC\Documents", ja salvestage fail.
5. Avage tekstiredaktoris MyConnection.rdp fail ja otsige seadme, mille soovite ümber suunata eksemplari ID-d.

Nüüd kasutada eksemplari ID järgmine cmdlet-käsk:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Aidake meil aidata teil 
Kas teadsite, et lisaks selles artiklis hindamine ja muutes kommentaaride alla all, saate muuta ise artiklit? Midagi puudu? Midagi on valesti? Kas midagi, mis on lihtsalt selgem kirjutada? Liikuge kerides üles ja klõpsake nuppu **Redigeeri github** muudatuste tegemiseks – need tulevad meile läbivaatamiseks, ja siis, kui me logige välja need, näete oma muudatused ja täiustused siinsamas.