<properties 
    pageTitle="Kuidas luua meediumi protsessor | Microsoft Azure'i" 
    description="Saate teada, kuidas luua meediumi protsessor komponent kodeerida, teisendamine, krüptida või dekrüptida meediumisisu Azure Media Servicesi jaoks." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Kuidas: Media protsessor eksemplari hankimine


> [AZURE.SELECTOR]
- [.NET-I](media-services-get-media-processor.md)
- [ÜLEJÄÄNUD](media-services-rest-get-media-processor.md)

##<a name="overview"></a>Ülevaade

Vormindage Media Servicesi meediumi protsessor on osa, mis tegeleb konkreetse töötlemise ülesannet, näiteks kodeeringus, teisendamine, krüptimise või dekrüptimine meediumisisu. Tavaliselt loote meediumi protsessor loomisel tööülesande kodeerida, krüptimiseks või teisendamine meediumi sisu.

Järgmises tabelis on ära toodud nime ja kirjeldust iga saadaval meediumi protsessor.

Meediumi protsessor nimi|Kirjeldus|Lisateave
---|---|---
Media kodeerija Standard|Standardse võimalusi pakub nõudmisel kodeerimine. |[Ülevaade ja Azure klõpsake vajadusel Media kodeerimisseadmetest võrdlus](media-services-encode-asset.md)
Media kodeerija Premium töövoo|Saate kodeering tööülesannete abil Media kodeerija Premium töövoo käivitada.|[Ülevaade ja Azure klõpsake vajadusel Media kodeerimisseadmetest võrdlus](media-services-encode-asset.md)
Azure Media indekseerija| Võimaldab teil teha meediumifailide ja sisu otsida, samuti luua suletud pealdise jälitab ja märksõnad.|[Azure Media indekseerija](media-services-index-content.md)
Azure Media Liikuv stoppkaader (eelvaade)|Võimaldab tõrgeteta välja "muhke" Videostabiliseerimise video. Võimaldab teil kiirendamiseks sisu tarbitavad klipi sisse.|[Azure Media Liikuv stoppkaader](media-services-hyperlapse-content.md)
Azure Media Encoderi|Väärtus on kahanenud
Salvestusruumi dekrüptimine| Väärtus on kahanenud|
Azure Media pakendaja|Väärtus on kahanenud|
Azure Media krüptija|Väärtus on kahanenud|

##<a name="get-mediaprocessor"></a>MediaProcessor hankimine

>[AZURE.NOTE] Töötades Media Services REST API-ga, rakendatakse järgmisega.
>
>Üksuste Media teenuste kasutamisel peate seadma kindlate päise välju ja väärtuste HTTP-päringud. Kohta leiate teemast [Media Services REST API arengu häälestus](media-services-rest-how-to-use.md).

>Pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI, nagu on kirjeldatud [Media Servicesi kaudu REST API -ga ühenduse loomine](media-services-rest-connect-programmatically.md). 


Järgmise ülejäänud kõne näitab, kuidas saada meediumi protsessor eksemplari nimi (sel juhul, **Media kodeerija Standard**). 



    
Taotlemiseks tehke järgmist.

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Vastus:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui teate, kuidas saada meediumi protsessor eksemplari, minge [kuidas kodeerida vara](media-services-rest-get-started.md) teema, mis näitab teile, kuidas kasutada Media kodeerija Standard kodeerida vara.
