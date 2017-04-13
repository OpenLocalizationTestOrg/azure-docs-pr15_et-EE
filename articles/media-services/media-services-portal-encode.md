<properties
    pageTitle="Kodeerida Media kodeerija Standard funktsiooniga Azure portaali vara | Microsoft Azure'i"
    description="Selles õpetuses juhendab teid kodeerimine Media kodeerija Standard funktsiooniga Azure portaali vara juhiseid."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Kodeerida Media kodeerija Standard funktsiooniga Azure portaali vara

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 

Kui te töötate Azure Media Servicesi üks kõige levinumad stsenaariumid on kohandatava bitrate streaming pakkuda oma klientidele. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks. Videote ettevalmistamine kohandatava bitrate streaming, peate kodeerida allikas video mitme bitikiirusega failid. Kasutage **Media kodeerija Standard** kodeerija kodeerida videoid.  

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma mitme bitikiirusega MP4s streaming failivormingud: MPEG KRIIPSJOONE, HLS, sujuva voogesituse või HDS, pole vaja uuesti sisse neid vorminguid streaming paketti. Dünaamiliste pakendit peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi ehitada ja teeni sobiv vastus taotluste klientrakenduses.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida lähtefaili failikogumi mitme bitikiirusega MP4 (kodeering juhiseid on näidatud allpool selles jaotises).
- Vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [konfigureerimise streaming lõpp-punktid](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

Skaala meediumid töötlemine, leiate [selle](media-services-portal-scale-media-processing.md) teema.

## <a name="encode-with-the-azure-portal"></a>Azure'i portaalis kodeerida

Selles jaotises kirjeldatakse, mida ette võtta teie sisu Media kodeerija Standard kodeerida juhiseid.

1.  Valige [Azure portaali](https://portal.azure.com/)konto Azure Media Servicesi.
2.  Valige aknas **sätted** **varad**.  
2.  Valige aknas **vara** vara, mida soovite kodeerida.
3.  **Encode** nupule.
4.  Valige aknas **vara Encode** "Media kodeerija Standard" protsessor ja valmissäte. Näiteks kui teate, et Sisestuskeel video on resolutsioon 1920 x 1080 pikslit, siis võite kasutada funktsiooni "H264 mitme Bitrate 1080p" eelseadistatud. Lisateabe saamiseks vt [käesoleva](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikli – valmiskombinatsioonid on oluline valige valmissäte, mis on kõige kohasem sisendit video. Kui teil on video madala eraldusvõime (640 x 360), siis te ei tohi kasutada vaikimisi "H264 mitme Bitrate 1080p" eelseadistatud.
    
    Lihtsam juhtimine, on teil võimalus redigeerimine väljundi varade nime ja töö nime.
        
    ![Kodeerida varad](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Vajutage **loomine**.


##<a name="next-step"></a>Järgmise juhise juurde

[Selles](media-services-portal-check-job-progress.md) artiklis kirjeldatud saate jälgida kodeering töö edenemist Azure'i portaalis.  

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


