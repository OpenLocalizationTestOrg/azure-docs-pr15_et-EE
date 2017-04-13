<properties
    pageTitle=" Lõpp-punktid Azure'i portaalis streaming skaala | Microsoft Azure'i"
    description="Selles õpetuses juhendab teid skaleerimist streaming lõpp-punktid Azure'i portaalis juhiseid."
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


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Lõpp-punktid Azure'i portaalis streaming skaala

##<a name="overview"></a>Ülevaade

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 

Kui te töötate Azure Media Servicesi üks kõige levinumad stsenaariumid on pakkuda oma klientidele video voogesitus kohandatava bitrate kaudu. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 kodeeritud sisu voogesitamine vormingud Media Services (MPEG KRIIPSJOONE, HLS, sujuva voogesituse, HDS) lihtsalt õigel ajal, pole vaja salvestada iga streaming vormingud eelnevalt pakitud versioonid ei toeta.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili failikogumi kohandatava bitrate MP4 (kodeering juhiseid on näidatud allpool selle õpetuse).  
- Looge vähemalt üks streaming ühiku *streaming lõpp-punkti* , millest plaanite kohaletoimetamise sisu. Kuidas muuta streaming ühikute alltoodud juhiseid kuvamine

Dünaamiliste pakendit peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi ehitada ja teeni sobiv vastus taotluste klientrakenduses.

Lisaks saate määrata teenuse lõpp-punkti Streaming võimsus kasvab läbilaskevõime vajadustele reageerimine, reguleerides streaming üksused. Soovitatav on eraldada rakenduste tootmiskeskkonnas skaala üks või mitu üksust. Streaming üksuste teile nii sihtotstarbeline sealt võimsus, mida saab osta kerimine 200 Mbps ja lisafunktsioonidega millised funktsioonid, mis sisaldab: [dünaamiline pakendit](media-services-dynamic-packaging-overview.md), CDN integreerimine ja täpsem konfiguratsioon. Lisateavet leiate teemast [haldamine streaming lõpp-punktid Azure'i portaalis](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Lõpp-punktid streaming skaala

Luua ja muuta streaming reserveeritud üksuste arvu, tehke järgmist.

1. Valige [Azure portaali](https://portal.azure.com/)konto Azure Media Servicesi.
2. Valige aknas **sätted** **Streaming lõpp-punktid**.
3. Klõpsake streaming lõpp-punkti, mida soovite skaala. 
4. Liigutage liugurit, et määrata streaming ühikute
 
![Lõpp-punkti streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Kehtivad järgmisega.

- Mis tahes streaming uute üksuste eraldatud ette võtta umbes 20 minutit lõpuleviimine. 
- Praegu läheb mis tahes positiivne väärtus streaming üksuste tagasi pole, saate keelata nõudmisel streaming kuni tund.
- Suurim arv 24 tunni jooksul jaoks määratud mõõtühikutes kasutatakse kulude arvutamisel. Lisateavet üksikasjad hindade kohta leiate teemast [Media Services hinnad üksikasjad](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Järgmised sammud

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


