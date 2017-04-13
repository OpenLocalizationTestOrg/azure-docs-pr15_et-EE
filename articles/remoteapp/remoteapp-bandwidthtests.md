<properties 
    pageTitle="Azure RemoteApp - testimine teie võrgu läbilaskevõime kasutuse mõne levinud stsenaariumi | Microsoft Azure'i"
    description="Siit saate teada, kuidas oma võrgu läbilaskevõime vajab Azure RemoteApp välja selgitada ühise kasutamise stsenaariumid, mis aitavad teil."
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
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - teie võrgu läbilaskevõime kasutuse koos mõne levinud stsenaariumi testimine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Nagu me [hinnangu Azure RemoteApp võrgu läbilaskevõime kasutuse](remoteapp-bandwidth.md), on parim viis aru saada, mis Azure RemoteApp mõju teie võrku on mõned kasutus käivitamiseks testitakse. Käivita katsed aja jooksul ja iga stsenaariumi puhul vajalikku läbilaskevõimet mõõta. Kui teil on võimalus, saate selle paketi kaotsimineku ja võrgu võrguvärin mõista võrgu mustreid, mis on loodud teatud keskkonna ka mõõta.

    
Läbilaskevõime kasutuse võrdlemisel pidage meeles, et kasutamine erineb erinevatele kasutajatele oma ettevõttes. Näiteks teksti lugejad ja poolt tavaliselt peab olema väiksem kui kasutajatele, mis töötavad video ribalaius. Parimate tulemuste saamiseks uurimiseks kasutaja oma vajadustele ja mix järgmistest tingimustest, mis tähistab kõige paremini teie ettevõtte kasutajate loomine. Ärge unustage [Läbivaatus tegureid, mis mõju läbilaskevõime kasutuse ja kasutajale kogemus](remoteapp-bandwidthexperience.md) – mis aitavad teil tuvastada optimaalne testide.

Esmalt lugege testide, valige oma Mixi ja seejärel käivitage need. Järgmises tabelis abil saate jälgida jõudlust.

>[AZURE.NOTE] Kui te ei saa oma võrgu testimine või teil on selleks aega, vaadake meie [peamiste võrgu läbilaskevõime hinnangulise/soovitused](remoteapp-bandwidthguidelines.md). Teie läbitud vahemaa võib erineda, aga nii, et kui te *saate* käivitada oma testide, peaksite.


## <a name="the-usage-tests"></a>Testide kasutamine
Kõigi nende testi Käivita eri palju aega ja testida erinevaid funktsioone/funktsioonid, mis võrgu läbilaskevõime. Ärge unustage valida testi kombinatsiooni, et kõige paremini vastab individuaalsed kasutajate.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Kommenteeritud/kompleksarvu PowerPoint - 900-1000 sekundit käivitamine

Kasutaja esitab 45 – 50 kvaliteetse slaidide, kasutades Microsoft Office PowerPointi täisekraanirežiimist. Slaidide peaks sisaldama pilte, üleminekud (koos animatsioonid) ja taustaga värv, astmik, mis on tüüpilised oma ettevõtte jaoks. Kasutaja peaksite kulutama vähemalt 20 sekundit iga slaidi.
    
Selle stsenaariumi loob bursty liikluse, kui slaidi üleminekud järgmisele slaidile esitluse.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Lihtsa PowerPoint - ~ 620 sekundit käivitamine

Kasutaja annab lihtsa PowerPointi faili ligikaudu 30 slaide, kasutades Microsoft Office PowerPointi täisekraanirežiimist. Slaidide on veel teksti mahukat kui kommenteeritud/kompleksarvu PowerPointi stsenaariumi ja lihtsam taustad ja pildid (must diagrammid). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Exploreris – käitada ~ 250 sekundit all

Kasutaja sirvib veebi Internet Exploreri abil. Kasutaja sirvib ja kerides teksti, loomulikus pilte ja mõned skemaatilised diagrammid kaudu. Kaugtöölaua seansi hosti (RD seansi hosti) server nimega kohalikule kettale salvestatud veebilehtede on. MHT-faili. Kasutaja kerides erineval tagant kerimisriba Type iga klahv Page Up, Page Down, üles ja alla klahvid, abil:
    
    - Alla - 250 klahvivajutusega keelevälja 500 ms.
    - Page Up - 36 klahvivajutusega iga 1000 ms
    - Alla - 75 klahvivajutusega iga 100 ms
    - Page Down – 20 klahvivajutusega iga 500 ms
    - Üles - 120 klahvivajutused iga 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-dokumendi - lihtne - käivitada ~ 610 sekundit all
Kasutaja loeb ja otsib PDF-dokumendi mitmel viisil Adobe Acrobat Reader abil. Dokumendi peaks olema tabelid, graafikud lihtsate ja mitu teksti fonti. Dokument on 35-40 lehekülge. Kasutaja kerides kuni kahe erinevate määrade tahapoole ja edastab nelja erinevate suumi suuruses (lehel, laius Mahuta mahu 100%, ja teine enda valitud Meilikausta). Suum tagab, et tekst (font) muudab erinevas suuruses. Kerimine on alla Page Up, Page Down, üles ja alla klahvid, kasutades erineva intervallide iga kerimisriba.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>Dokumendi PDF - kombineeritud - Käivita ~ 320 sekundit all
Kasutaja loeb ja otsib PDF-dokumendi mitmel viisil Adobe Acrobat Reader abil. Dokumendi koosneb kõrge kvaliteediga kujutised (sh fotod), tabeleid, lihtsa graafikuid ja mitu teksti fonti. Kasutaja kerides kuni kahe erinevate määrade tahapoole ja edastab nelja erinevate suumi suuruses (lehel, laius Mahuta mahu 100%, ja teine enda valitud Meilikausta). Suum tagab, et tekst (font) muudab erinevas suuruses. Kerimine on alla Page Up, Page Down, üles ja alla klahvid, kasutades erineva intervallide iga kerimisriba.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash video taasesitus - ~ 180 sekundit käivitamine
Kasutaja kuvab manustatud veebilehele Adobe Flash-kodeeringuga video. Veebilehe on talletatud arvuti kõvakettale RD seansi hosti server. Videot esitatakse Internet Exploreri manustatud pleieris, kes on lisandmooduli.

Selle stsenaariumi emuleerib vaatamine rikkaliku sisu veebilehti, mis sisaldab kasutajate. Enamik andmeid peaks bo VOBR kaudu.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Wordi tippimist Kaug - Run ~ 1800 sekundit all
Kasutaja tipib dokumendi üle RDP-seanss. Klahvivajutusega saadetakse kliendi poolel RDP-seansi kaudu Microsoft Wordi dokumendi Kaugseanss töötab. Tippimisega on ühe märgi iga 250 ms (7050 märke kokku). 

See on üks kõige levinumad stsenaariumid teadmisi töötaja. See stsenaarium kontrollib reageerimine tippides ajakohase töö protsessor kasutaja. See stsenaarium on tundlik isegi väikesed muutused läbilaskevõime kasutuse.

## <a name="tracking-the-test-results"></a>Kontrolltöö tulemuste jälgimine

Järgmise tabeli abil saate stsenaariumid hindab teie keskkonnas. Allpool esitatud andmed on just joonisel – see võib olla väga erinevad, mida saate jälgida. 

Lihtsa, me oletame, et kõik stsenaariumid testitakse, kasutades sama 1920 x 1080 pikslit resolutsioon ja TCP transport võrgus, kus latentsus (viivitus) all 200 ms ja võrgu värin 120 ms + märgi 1%.

Tabel: kohta
- **Keskmise versioon** sisaldab võrgu läbilaskevõime, kui kasutaja kontoritöö on märkimisväärselt mõjutada, kuid ei välista aeg-ajalt video või heli tõrgete. Süsteemi on võimalik taastada kiiresti dünaamiline loogika ära. Võrgu läbilaskevõime hinnangulise proovi kasutusvõimalused kvaliteedi.
 - Võrgu läbilaskevõime, kus kasutajad märgata märkimisväärset probleemid oma kogemusi ja nende tööviljakust mõjutab mõõta ajaperioodide **noticeable probleemid (leheküljepiiri punkt)** sisaldab. Selles etapis RDP algoritmide on raskusi ja ei saa tagada kasutaja kvaliteedi kogemus pole piisavalt võrgu läbilaskevõime tõttu.
 - **Soovitatav** sisaldab võrgu läbilaskevõime, mis on hea või suurepärane ette nähtud. See on tavaliselt ühe toiming väärtust, **Keskmine kogemus** veerus.
 - **Märkmete** sisaldavad vaatluste ja kommentaarid.
 
| Test                  | Keskmine kogemus | Märgatav probleemid (leheküljepiiri punkt) | Soovitatav võrgu läbilaskevõime | Märkmete                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Kommenteeritud/kompleksarvu PPT-faili | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s eelistatud    | 1 MB/s palju animatsioone lähevad kaotsi                                   |
| Lihtne PPT-faili            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | 256 KB/s slaidide märgatav viivitusega laadimine                   |
| Internet Exploreris     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s eelistatud    | 1 MB/s web videod on udune ja hakkimine, Kiire kerimine on probleeme |
| Lihtne PDF-faili            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | 256 KB/s kulub samas lehe laadimiseks                       |
| Kombineeritud PDF-faili             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | 256 KB/s lehe kulub palju aega laadida    |
| Flash video taasesitus  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s eelistatud    | 1 MB/s video on teraline ja mõned raamid kõrvaldatakse           |
| Wordi remote tippimist    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | 256 KB/s kasutaja võib-olla märkate klahvivajutusega vahele jääv aeg             |

Hinnata võrgu läbilaskevõime kasutaja kohta, luua kombinatsiooni ülaltoodud stsenaariumid ja nõutava võrgu läbilaskevõime vastav osa. Valige oma stsenaariumi järgi vaja suurim lubatud arv. Kuna kasutajad kasutavad peaaegu kunagi eraldi süsteemi, kaaluge mõne reserv kasutajatelt samasse võrku samaaegselt töötada.
     
## <a name="learn-more"></a>Lisateave
- [Azure RemoteApp võrgu läbilaskevõime kasutuse prognoosimiseks](remoteapp-bandwidth.md)

- [Azure RemoteApp – kuidas teha võrgu läbilaskevõime ja -kvaliteeti ning kogemusi töö koos?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp võrgu läbilaskevõime - üldised juhised (kui te ei saa Testige oma)](remoteapp-bandwidthguidelines.md)