<properties 
    pageTitle="Kuidas luua meediumi protsessor | Microsoft Azure'i" 
    description="Saate teada, kuidas luua meediumi protsessor komponent kodeerida, teisendamine, krüptida või dekrüptida meediumisisu Azure Media Servicesi jaoks. Koodinäiteid C# kirjutada ja kasutada Media Services SDK .net-i jaoks." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

##<a name="get-media-processor"></a>Saada meediumi protsessor

Järgmise meetodi näitab, kuidas hankida meediumi protsessor eksemplari. Koodi näide eeldab mooduli taseme muutuja nimega **_context** viitamiseks Serveri konteksti, nagu on kirjeldatud jaotises Kasuta [kohta: ühenduse Media Services programmiliselt](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui teate, kuidas saada meediumi protsessor eksemplari, minge [kuidas kodeerida vara](media-services-dotnet-encode-with-media-encoder-standard.md) teema, mis näitab teile, kuidas kasutada Media kodeerija Standard kodeerida vara.


