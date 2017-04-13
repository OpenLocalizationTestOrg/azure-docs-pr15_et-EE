<properties
    pageTitle="Meediumi töötlemine ülevaade skaleerimist | Microsoft Azure'i"
    description="See teema on skaleerimise meediumi töötlemine Azure Media Servicesi ülevaade."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Skaleerimise meediumi töötlemine ülevaade

Selle lehe kaudu saate ülevaate sellest, kuidas ja miks skaala meediumid töötlemine. 

## <a name="overview"></a>Ülevaade

Media Servicesi kontoga on seotud reserveeritud üksuse tüüp, mis määratleb, millega töötlemise tööülesannete meediumifailide töödeldakse kiirust. Saate valida vahel reserveeritud ühiku järgmist tüüpi: **S1**, **S2**või **S3**. Näiteks sama kodeering tööd on kiirem kui kasutate **S2** reserveeritud üksuse tüüp Võrdle **S1** tüüp. Lisateabe saamiseks vt [Reserveeritud üksuse tüüpi](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Lisaks määramisele reserveeritud üksuse tüüp, saate määrata, kas teie konto ettevalmistamise reserveeritud üksustega. Ettevalmistatud reserveeritud ühikute määratleb media tööülesandeid, mida saab üheaegselt töödeldav antud kontol arvu. Näiteks kui teie konto on viis reserveeritud üksused, siis viis meediumi tööülesannete töötab samaaegselt nii kaua, nagu on ülesandeid töötlemiseks. Ülejäänud ülesanded järjekorras oodata ja saada piirkonnadiagrammi töötlemiseks järjest kui töötava tööülesande lõpeb. Kui konto ei saa mis tahes reserveeritud üksused, mis on ette valmistatud, siis tööülesannete võetakse peale järjest. Sel juhul ootama aega ühe tööülesande viimistlemine ja edasi ühe alates vahel sõltub ressursside kättesaadavus süsteemi.

## <a name="choosing-between-different-reserved-unit-types"></a>Erinevate reserveeritud ühiku vahel valimine

Järgmine tabel aitab teil kujundada otsust valides mõõtmeid kodeering vahel. See pakub võrdlusalus mõnel juhul ja pakub SAS URL-id, mille abil saate alla laadida videoid, mida saate teha oma kontrollib:

Stsenaariumid|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Mõeldud juhul| Ühe bitrate kodeeringus. <br/>Failide SD või lahendused, ei ole aega tundliku loomuga, madal.|Ühe bitrate ja mitme bitrate kodeeringus.<br/>Tavaline kasutus nii SD ja HD kodeeringus. |Ühe bitrate ja mitme bitrate kodeerimine.<br/>Täielik HD ja 4K eraldusvõimega videod. Aeg tundlik, kiirem pööre kodeerimine. 
Kriteerium|[Sisestatud fail: 5 minutit pikk 640x360p veebisaidil 29.97 raamid/teise](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Ühe bitrate MP4 faili sama eraldusvõimega kodeering võtab aega umbes 11 minutit.|[Sisestuskeel fail: 5 minutit pikk 1280x720p veebisaidil 29.97 raamid sekundis](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>"H264 ühe bitrate 720p" kodeering algne võtab ligikaudu 5 minutit.<br/><br/>Koos kodeerimine "H264 mitme Bitrate 720p" valmissäte võtab aega umbes 11,5 minutit.|[Sisestatud fail: 5 minutit pikk 1920 x 1080 p veebisaidil 29.97 raamid/teise](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>"H264 ühe bitrate 1080p" kodeeringus algne võtab ligikaudu 2,7 minutit.<br/><br/>Koos kodeerimine "H264 mitme Bitrate 1080p" valmissäte võtab aega umbes 5,7 minutit.

##<a name="considerations"></a>Kaalutlused

>[AZURE.IMPORTANT] Selles jaotises kirjeldatud kaalutlused läbivaatus.  

- Reserveeritud üksuste tööd parallelizing kõik meediumi töötlemine, sh indekseerimise abil Azure Media indekseerija.  Erinevalt kodeerimine, indekseerimise tööde haldamine ei saada töödelda, kiiremini kiiremini reserveeritud üksustega.

- Kui kaudu ühiskasutusse antud kausta, mis on, ilma mis tahes reserveeritud üksused, siis Kodeeri tööülesanded on sama jõudlus S1 RUs sarnaselt. Samas on tööülesannete kulutada ootel olekus ja mis tahes ajal, kõige üks tööülesanne on aeg ülempiiri töötama.

- Järgmised andmekeskuste ei paku **S2** reserveeritud üksuse tüüp: Brasiilia Lõuna, India Lääne, India Kesk- ja Lõuna India.

- Järgmised andmekeskuste ei paku **S3** reserveeritud üksuse tüüp: Brasiilia Lõuna India Lääne, India Kesk.

- Suurim arv 24 tunni jooksul jaoks määratud mõõtühikutes kasutatakse kulude arvutamisel.


##<a name="quotas-and-limitations"></a>Kvootide ja -piirangud

Kvootide ja piirangud ja kuidas avada tugi piletite kohta leiate teavet teemast [kvootide ja -piirangud](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Järgmise juhise juurde

Saavutada skaleerimise meediumi töötlemine ülesanne ühe nende tehnoloogiate: 

> [AZURE.SELECTOR]
- [.NET-I](media-services-dotnet-encoding-units.md)
- [Portaal](media-services-portal-scale-media-processing.md)
- [ÜLEJÄÄNUD](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
