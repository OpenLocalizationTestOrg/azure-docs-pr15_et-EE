<properties
    pageTitle="Azure Media Services Analytics ülevaade | Microsoft Azure'i"
    description="Azure Media Servicesi pakub Azure Media Analytics, kõne- ja arvuti nägemine teenusekomplekt enterprise skaala, nõuetele vastavus, Turve ja globaalse REACHi avaliku eelvaade. Azure Media veebianalüütika teenused on Azure Media Servicesi platvormi põhikomponentide abil loodud ja seega on valmis hakkama meediumi töötlemise skaala esimesel päeval. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Azure Media Services Analytics ülevaade

##<a name="overview"></a>Ülevaade

Lisateavet ettevõtted ja suurettevõtete on ülevaade video eelistatud keskmise ja koolitada oma töötajatele, nende klientide ja dokumendi ärifunktsioonide. Pilveteenuse arvutuste muudab need tõhusaks talletamiseks, taustal alla laadida ja neid suurte meediumifaile juurde, kuid ettevõtted kasvavad sisu Videoteek, ta peab olema sama tõhusaid ekstraktimine uusi teadmisi video loomiseks tähendusrikkama, isikupärastatud kasutusviisid oma publikule ja oma ettevõtte viimine uuele tasemele.

Selle funktsiooni turuplatsil suureneb käsitlema Azure Media Servicesi pakub Media Analytics, kõne- ja nägemispuudega komponendid (enterprise skaala, nõuetele vastavus, turvalisus ja globaalse REACHi), mis hõlbustavad ettevõtted ja ettevõtetele tuletada vaidlustatavad ülevaateid oma videofaile kogum. Azure Media veebianalüütika teenused on Azure Media Servicesi platvormi põhikomponentide abil loodud ja seega on valmis hakkama meediumi töötlemise skaala esimesel päeval.

Azure Media Analytics arendajatel kiiresti alustada nägemispuudega inimestele video piiratud alal ja selle täpsemate funktsioonide kasutamiseks rakendustesse. Azure Media Analytics on ehitatud enterprise keskkonnad skaala, nõuetele vastavus, turvalisus ja globaalse REACHi nõutud suurettevõtete kasutada.

Järgmisel joonisel on esitatud **Meediumi Analytics** ja Media Servicesi platvormi põhi mujal. 

![VoD töövoo](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Meediumi Analytics meediumi protsessorite aedvili MP4 või JSON failide. Kui meediumi protsessor toodeti MP4 faili, saate faili järk-järgult alla. Kui meediumi protsessor toodeti JSON faili, saate laadida alla faili Azure'i bloobimälu. 

## <a name="azure-media-analytics-services"></a>Azure Media veebianalüütika teenused

- **Indekseerija** – Azure Media indekseerija võimaldab teil teha sisu otsida, samuti luua suletud pealdise jälitab. Azure Media Servicesi välja **Azure Media indekseerija 2 eelvaade** koos kiiremini indekseerimise ja laiem keele tuge. Toetatud keeltes kaasata inglise, Hispaania, prantsuse, saksa, Itaalia, Hiina, Portugali ja Araabia. Üksikasjalikku teavet ja näited leiate teemast [Azure Media indekseerija 2 protsessi videod](media-services-process-content-with-indexer2.md)
 
- **Liikuv stoppkaader** – Microsoft Hyperlapse on tulemus on üle 20 aasta arvuti nägemine uurimistöö veebisaidil Microsoft Researchi (MSR), kombineerides Videostabiliseerimise ja kellaaja aegumine luua kiirülevaate, tarbitavad, ilus videod pikk vormile sisu. Lisaks saate luua ajavahemikud, saate kasutada Liikuv stoppkaader loomiseks ühed videod nõrk videod mobiiltelefone ja videokaamerad kaudu. Üksikasjalikku teavet ja näited leiate teemast [Azure Media Liikuv stoppkaader Liikuv stoppkaader meediumifaile](media-services-hyperlapse-content.md)
 
- **Liikumise tuvastamine** – selle teenuse abil saate tuvastada liikumistee kirjaplangi taustaga video. See on mõeldud klientidele, kes soovite kontrollida vale-positiivsed liikumistee sündmused avastada järelevalve foto järelevalve video kanalite kohta. Üksikasjalikku teavet ja näited leiate teemast [Azure Media Analyticsi liikumise tuvastamine](media-services-motion-detection.md).
 
- **Näo tuvastamise ja näo emotsioone** – selle teenuse abil saate tuvastada, inimeste näod ja oma emotsioone, sh õnne, kurbust, ebameeldivusi, viha, korralduse, hirmu, jälestust ja ükskõiksus/neutraalne. See on mitu kasulikku valdkonna rakendust, allpool kirjeldatud, sh liitmise ja analüüsimine esinenud osalemise sündmuse inimesed. Üksikasjalikku teavet ja näited leiate teemast [näo ja Azure Media Analyticsi emotsioon tuvastamise](media-services-face-and-emotion-detection.md).
 
- **Video kokkuvõtet** – Video kokkuvõtet aitavad teil luua kokkuvõtted pikk videod, valides automaatselt huvitav Koodilõigud allikas video. See on kasulik, kui soovite anda kiire ülevaade sellest, mida oodata pikk video. Üksikasjalikku teavet ja näited leiate teemast [Kasutamine Azure Media Video pisipildid Video kokkuvõtet loomiseks](media-services-video-summarization.md)

- **Optilise märgituvastuse** - Azure Media Analytics OCR (OCR) võimaldab teil teksti sisu video faile redigeerida, otsida digitaalse tekstiks teisendada. See võimaldab teil mõtestatud metaandmete ekstraktimine video signaal meediumite automatiseerimiseks.
 
- **Skaleeritav näo muutmiseks** - **Azure Media redaktor** on Azure Media Analytics MP, mis pakub scalable näo muutmiseks pilveteenuses. Näo muutmiseks võimaldab teil muuta video selleks, et blur on valitud inimesed. Kui soovite avaliku ohutus- ja meedia stsenaariumide näo muutmiseks teenust kasutada. Videomaterjali, mis sisaldab mitut nägu paar minutit võib kuluda tundi redigeerimine käsitsi, kuid see teenus nõuab näo muutmiseks protsessi vaid mõned lihtsad juhised. Lisateabe saamiseks lugege [artiklit](media-services-face-redaction.md) .

 
## <a name="common-scenarios"></a>Levinumad stsenaariumid

Allpool on mõned stsenaariumid, kus Azure Media Analytics aitavad ja suurettevõtete tööstusharudes noppima videot, et luua veel isikupärastatud sihtrühma ja töötaja kokkulepete uusi teadmisi, samuti tõhusamalt hallata suurel hulgal video sisu:

- **Kõne andmekeskuste** – paaris, sotsiaalmeedia, kliendi kõne andmekeskuste hõlbustada endiselt suur osa teenuse klienditoimingud on muutnud. Kodeeritud heli andmed on hulgaliselt teavet klientidele, mis võib parandada toote tegevuskava ja koolitada ka kõne center töötajad klientide rahulolu saavutamiseks analüüsida. Azure Media indekseerija abil kliendid on ekstraktida teksti ja otsing indeks ja armatuurlaudade eraldamiseks ärianalüüsi ümber levinumate kurdab, allika kurdab koostamine ja muud asjakohased andmed.

- **Kasutaja loodud sisu modereerimine** – kaudu meedia väljundi politseiosakonnad, paljude organisatsioonidel on avaliku suunatud portaalide, kus aktsepteerimist UGC meediumi, nagu videod ja pildid. Sisu maht saate kühvel tõttu ootamatud sündmused. Need stsenaariumid on lähedal efektiivse käsitsi läbivaatamiseks asjakohasuse sisu ei ole võimalik. Kliendid saavad toetuvad sisu modereerimine teenuse sisu, mis on asjakohane keskenduda.

- **Järelevalve** - kasvu IP kaamera, on plahvatus järelevalve videod. Käsitsi läbivaatamise järelevalve video on aeg intensiivse ja palju inimeste vigu. Azure Media Analytics sisaldab mitut komponentide nagu liikumise tuvastamine, näo tuvastamise ja Liikuv stoppkaader läbivaatamise, haldamine ja derivaadid loomise lihtsamaks teha.

## <a name="media-services-analytics-media-processors"></a>Media Services Analytics meediumi protsessorid 

Selles jaotises on loetletud kõik Media Services Analytics meediumi protsessorite (MP) ja kuvab kasutamise .net-i või REST saada MP objekti.

### <a name="mp-names"></a>MP nimed


- Azure Media indekseerija 2 eelvaade
- Azure Media indekseerija
- Azure Media Liikuv stoppkaader
- Azure Media esinema detektor
- Azure Media liikumisandur
- Azure Media Video pisipildid
- Azure Media OCR-i

### <a name="net"></a>.NET-I

Järgmise funktsiooni võtab üks määratud MP nimed ja tagastada MP objekti.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>ÜLEJÄÄNUD

Taotlemiseks tehke järgmist.

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Vastus:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Demo

[Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Järgmised sammud

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Seotud artiklid

[Media Services Analytics teadaanne](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
