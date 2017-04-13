<properties 
    pageTitle="Azure RemoteApp – kuidas teha võrgu läbilaskevõime ja -kvaliteeti ning kogemusi töö koos? | Microsoft Azure'i"
    description="Siit saate teada, kuidas võrgu läbilaskevõime Azure RemoteApp võib mõjutada teie kasutaja kvaliteedi kogemus."
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

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – kuidas teha võrgu läbilaskevõime ja -kvaliteeti ning kogemusi töö koos?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kui vaatate [üldised võrgu läbilaskevõime](remoteapp-bandwidth.md) Azure RemoteApp jaoks nõutavad, võtke arvesse järgmisi tegureid – need on kõik osa dünaamiline süsteem, mis mõjutab üldine kasutusvõimalused. 

- **Võrgu läbilaskevõime ja praegust võrku** - parameetrite (kaotsimineku latentsuse värin) kindlal ajahetkel samas võrgus võib mõjutada rakenduse streaming kogemusi, mis tähendab alandas üldine kasutusvõimalused. Saadaval võrgu läbilaskevõime on selle funktsiooni ülekoormuse, juhusliku kaotsimineku, latentsus, kuna kõik need parameetrid mõjutavad ülekoormuse juhtelemendi süsteem, mis omakorda juhtelementide edastamise kiirust kokkupõrgete vältimiseks.  Näiteks kadudega võrgu või pika latentsusajaga võrgu teeb kasutaja kogemus halb isegi võrgu läbilaskevõime 1000 MB. Kaotsimineku ja latentsusaeg sõltuvad samasse võrku ja mida nende kasutajate (nt videotest alla- või üleslaadimise suuri faile, printimine) teed asuvate kasutajate arv.
- **Kasutus stsenaarium** - kogemusi sõltub sellest, mida kasutajad teevad üksikisikud ja rühmana samas võrgus. Näiteks lugemine ühe slaidi nõuab ainult ühe paneeli värskendatud; Kui kasutaja skims ja kerides üle teksti dokumendi sisu, tuleb raamid värskendata sekundis suurema arvu. Lõpuks tarbib edasi ja tagasi teatis selle stsenaariumi server mitme võrgu läbilaskevõime. Kaaluge ka äärmiselt näide: mitme kasutaja on kõrglahutusega videotest (nt 4K eraldusvõime), hoides HD Konverentskõnede, 3D video mängimine või töötamise CAD operatsioonisüsteemides. Kõik need saab teha ka väga kõrge läbilaskevõime võrgu praktiliselt kasutuskõlbmatuks.
- **Eraldusvõimega ekraan ja arvu** - mitme võrgu läbilaskevõime on vaja täieliku värskenduse suurem ekraanid kui väiksematel ekraanidel. Aluseks oleva tehnoloogia ei päris hea töö kodeering ja edastavate ainult need regioonid värskendatud ekraanid, kuid kord, kui, kogu ekraani vaja värskendada. Kui kasutaja on suurema eraldusvõimega ekraan (nt 4K resolutsioon), et värskendus nõuab mitme võrgu läbilaskevõime kui ekraani väiksema eraldusvõimega (nt 1024x768px). Sama loogika kehtib juhul, kui kasutate rohkem kui ühe ekraanikuva ümbersuunamine. Läbilaskevõime peab koos arvu suurendamine.
- **Lõikelaua ja seadme ümbersuunamine** – see on probleem, mitte väga selge, kuid paljudel juhtudel kui kasutaja talletab suur hulk andmed lõikelauale, kulub veidi aega selle teabe edastamiseks kaugtöölaua klient server. Lõikelaua sisu enne saatmist kogemus võivad mõjutada järgmise etapi kogemus. Sama kehtib seadme ümbersuunamine – kui skannerist või veebikaamera toodab palju andmeid, mida on vaja saata varustava server või printer vajab vastuvõtmiseks mahukate dokumendi, või kohalikku peab olema saadaval rakendus töötab, et suured faili kopeerimine, kasutajate märgata kaotatud raamid ajutiselt "külmutatud" video kuna seadme ümbersuunamise vajalikud andmed kasvab võrgu läbilaskevõime vajab. 

Kui teie võrgu läbilaskevõime vajadustele hinnata, veenduge, et kõik töötab süsteemi teguritest silmas pidada.

Nüüd, minge tagasi [peamised võrgu läbilaskevõime artiklis](remoteapp-bandwidth.md)või viia oma [võrgu läbilaskevõime](remoteapp-bandwidthtests.md)testimine.

## <a name="learn-more"></a>Lisateave
- [Azure RemoteApp võrgu läbilaskevõime kasutuse prognoosimiseks](remoteapp-bandwidth.md)

- [Azure RemoteApp - teie võrgu läbilaskevõime kasutuse mõne levinud stsenaariumi testimine](remoteapp-bandwidthtests.md)

- [Azure RemoteApp võrgu läbilaskevõime - üldised juhised (kui te ei saa Testige oma)](remoteapp-bandwidthguidelines.md)