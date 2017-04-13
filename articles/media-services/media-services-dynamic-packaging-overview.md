<properties
    pageTitle="Dünaamiliste pakendit ülevaade | Microsoft Azure'i"
    description="Teema annab ja dünaamiline pakendit ülevaade."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dünaamiliste pakendit

##<a name="overview"></a>Ülevaade

Microsoft Azure Media Servicesi saab kasutada esitamisel palju meediumi allikas failivormingute, vorminguid, voogesituse ja sisu kaitse kliendi tehnoloogiad mitmesuguseid vorminguid (nt iOS-i, XBOX, Silverlighti, Windows 8). Need kliendid mõista erinevaid protokolle, näiteks iOS-i jaoks on vaja mõnda HTTP Live Streaming (HLS) V4 vorming ja Silverlighti ja Xbox jaoks on vaja sujuva voogesituse. Kui teil on kogumi kohandatava bitrate (mitme bitrate) MP4 (ISO Base meediumifailide 14496 – 12) või kohandatava bitrate sujuva voogesituse faile, mis aitavad mõista MPEG MÕTTEKRIIPSU, HLS või sujuva voogesituse klientides kogum, tuleks ära Media Servicesi dünaamiline pakendamise.

Dünaamiliste pakendit peate on varade, mis sisaldab kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide loomiseks. Seejärel alusel määratud vormingu loetelu või fragment taotluse, tellitavate Streaming server tagab kuvatakse voo valitud protokolli. Seetõttu peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi teenuse koostamine ja teeni sobiv vastus taotluste klientrakenduses.

Järgmisel joonisel on esitatud traditsiooniline kodeering ja staatilise pakendit töövoog.

![Staatilise kodeerimine](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Järgmisel joonisel on esitatud dünaamilise pakendit töövoog.

![Dünaamiliste kodeerimine](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Dünaamiliste pakendit ära, peate esmalt saada vähemalt üks tellitav streaming ühiku streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [skaala Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Levinum stsenaarium

1. Laadida Sisestuskeel faili (Standard faili nimi). Näiteks H.264, MP4 või WMV (jaoks toetatud vormingus vt [Vormingus toetatud Media kodeerija Standard](media-services-media-encoder-standard-formats.md)loendit.

1. Kodeerida Standard faili H.264 MP4 kohandatava bitrate komplektid.

1. MP4 määramine, luues tellitavate Locator kohandatava bitrate sisaldava varade avaldada.

1. Koostada streaming URL-ide juurdepääsu ja nende sisu voona.


##<a name="preparing-assets-for-dynamic-streaming"></a>Dünaamiliste streaming varad ettevalmistamine

Dünaamiliste streaming teie vara ettevalmistamiseks on teil kaks võimalust:

1. [Juhtslaidi faili üles laadida](media-services-dotnet-upload-files.md).
2. [Kasutage Media kodeerija Standard kodeerija andes H.264 MP4 kohandatava bitrate komplektid](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Voo sisu](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Vormingud, mis ei toeta dünaamiline pakendit

Andmeallika järgmistes failivormingutes ei toeta dünaamiline pakendit.

- Dolby digital mp4 faile.
- Dolby digital sujuv failid.

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
