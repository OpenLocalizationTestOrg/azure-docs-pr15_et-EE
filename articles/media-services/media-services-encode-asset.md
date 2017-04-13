<properties 
    pageTitle="Ülevaade ja vajadusel meediumi kodeerimisseadmetest Azure võrdlus | Microsoft Azure'i" 
    description="See teema annab ülevaade ja vajadusel meediumi kodeerimisseadmetest Azure võrdlus." 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Ülevaade ja vajadusel meediumi kodeerimisseadmetest Azure võrdlus

##<a name="encoding-overview"></a>Kodeering ülevaade

Azure Media Servicesi pakub meediumi pilveteenuses kodeerimine mitu võimalust.

Kui alustamist Media Servicesi, on oluline kodekite ja failivorminguid erinevus.
Kodekite on tarkvara, mis rakendab tihendab mahukaid algoritmide failivormingud on tihendatud video reguleerivad ümbriste.

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 või sujuva voogesituse kodeeritud sisu streaming Media Services (MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse, HDS) poolt toetatavad pole vaja uuesti sisse neid vorminguid streaming paketti.

[Dünaamiliste pakendit](media-services-dynamic-packaging-overview.md)ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili kogumi kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide (kodeering juhiseid on näidatud allpool selle õpetuse).
- Vähemalt üks nõudmisel streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [skaala nõudmisel Streaming reserveeritud üksused](media-services-portal-manage-streaming-endpoints.md).

Media Services toetab nõudmisel kodeerimisseadmetest, mida selles artiklis kirjeldatakse järgmist:

- [Media kodeerija Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media kodeerija Premium töövoo](media-services-encode-asset.md#media-encoder-premium-workflow)

Selles artiklis antakse lühiülevaade nõudmisel meediumi kodeerimisseadmetest ja sisaldab linke artiklitele, mis annab üksikasjalikumat teavet. Teema sisaldab ka koodritega võrdlus.

Pange tähele, et vaikimisi iga Media Servicesi konto saate ühe aktiivse kodeering tööülesande korraga. Saate reserveerida kodeering üksused, mis võimaldavad teil on mitu kodeering tööülesannete töötab üheaegselt, üks iga kodeering reserveeritud üksuse ostate. Lisateavet leiate teemast [mastaapimine kodeerimine üksused](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media kodeerija Standard

###<a name="how-to-use"></a>Kuidas kasutada

[Kuidas kodeerida koos Media kodeerija Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Vormingud

[Vormingute ja kodekite](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Valmiskombinatsioonid

Media kodeerija Standard on konfigureeritud, kasutades ühte kodeerija valmiskombinatsioonid kirjeldatud [allpool](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Sisend- ja metaandmed

Kodeerimisseadmetest Sisestuskeel metaandmete on kirjeldatud [allpool](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Metaandmete kodeerimisseadmetest väljund on kirjeldatud [allpool](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Luuakse pisipildid

Lisateavet leiate teemast [Kuidas luua pisipildid Media kodeerija Standard abil](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Trimmimine videod (lõikamine)

Lisateavet leiate teemast [trimmimise videote abil Media kodeerija standardne](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Ülekatete loomine

Lisateavet leiate teemast [kihtide kasutamise Media kodeerija Standard loomise kohta](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Vt ka

[Media Servicesi ajaveeb](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media kodeerija Premium töövoo

###<a name="overview"></a>Ülevaade

[Premium kodeerimine Azure Media Services tutvustus](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Kuidas kasutada

Media kodeerija Premium töövoog on konfigureeritud keerukate töövoogude kasutamise. Töövoo failide saanud luua ja värskendatud [Töövoodisaineris](media-services-workflow-designer.md) tööriista abil.

[Kuidas kasutada Premium kodeerimine Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Teadaolevad probleemid

Kui Sisestuskeel video ei sisalda tiitrid, väljund varade sisaldavad endiselt tühja TTML faili. 


##<a id="compare_encoders"></a>Kodeerimisseadmetest võrdlus

###<a id="billing"></a>Iga kodeerija poolt kasutatud arveldamine arvesti

Meediumi protsessor nimi|Kohaldatavad hinnad|Märkmete
---|---|---
**Media kodeerija Standard** |KODEERIJA|Kodeerimine tööülesanded tuleb tasuda suurusest väljund vara GBytes nimetatud määra [siin][1], KODEERIJA veerus.
**Media kodeerija Premium töövoo** |PREMIUM KODEERIJA|Kodeerimine tööülesanded tuleb tasuda suurusest väljund vara GBytes nimetatud määra [siin][1], PREMIUM KODEERIJA veerus.


Selles jaotises võrdleb kodeering võimaluste **Media kodeerija Standard** - ja **Media kodeerija Premium töövoog**.


###<a name="input-containerfile-formats"></a>Sisestusmeetodi Container/failivormingud

Sisestusmeetodi Container/failivormingud|Media kodeerija Standard|Media kodeerija Premium töövoo
---|---|---
Adobe® Flash® F4V           |Jah|Jah
MXF/SMPTE 377M              |Jah|Jah
GXF                         |Jah|Jah
MPEG-2 Transport voogu    |Jah|Jah
MPEG-2 programmi voogu      |Jah|Jah
MPEG-4/MP4                  |Jah|Jah
Windows Media/ASF           |Jah|Jah
AVI (tihendamata 8/10 bitise)|Jah|Jah
3GPP/3GPP2                  |Jah|Ei
Sujuva voogesituse failivorming (PIFF 1.3)|Jah|Ei
[Microsofti Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Jah|Ei
Matroska/WebM               |Jah|Ei
QuickTime (.mov) |Jah|Ei

###<a name="input-video-codecs"></a>Sisestuskeel videokodekid

Sisestuskeel videokodekid|Media kodeerija Standard|Media kodeerija Premium töövoo
---|---|---
AVC 8-bitise/10-bitine, kuni 4:2:2, sh AVCIntra   |8 bit 4:2:0 ja 4:2:2|Jah
Innukas DNxHD (jaotises MXF)                                 |Jah|Jah
DVCPro/DVCProHD (jaotises MXF)                            |Jah|Jah
JPEG2000                                            |Jah|Jah
MPEG-2 (kuni 422 profiili ja kõrge; nt XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10 variandid sh)|Kuni 422 profiil|Jah
MPEG-1                                              |Jah|Jah
Windows Media Video/VC-1                            |Jah|Jah
Canopuse HQ/HQX                                      |Ei|Ei
MPEG-4 osa 2                                       |Jah|Ei
[Theora](https://en.wikipedia.org/wiki/Theora)      |Jah|Ei
Apple ProRes 422    |Jah|Ei
Apple ProRes 422 LT |Jah|Ei
Apple ProRes 422 HQ |Jah|Ei
Apple ProRes puhverserveri|Jah|Ei
Apple ProRes 4444 |Jah|Ei
Apple ProRes 4444 XQ |Jah|Ei

###<a name="input-audio-codecs"></a>Sisestuskeel helikodekeid

Sisestuskeel helikodekeid|Media kodeerija Standard|Media kodeerija Premium töövoo
---|---|---
AES (SMPTE 331 M ja 302 M, AES3 – 2003)        |Ei|Jah
Dolby® E                                    |Ei|Jah
Dolby® digitaalne (AC3)                        |Ei|Jah
Dolby® digitaalne pluss (E-AC3)                 |Ei|Jah
AAC (AAC-LC, AAC-HE ja AAC-HEv2; kuni 5,1)|Jah|Jah
MPEG Layer 2|Jah|Jah
MP3 (MPEG-1 Audio Layer 3)|Jah|Jah
Windows Media Audio|Jah|Jah
WAV-/ PCM|Jah|Jah
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Jah|Ei
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Jah|Ei
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Jah|Ei


###<a name="output-containerfile-formats"></a>Container väljundi failivormingud

Container väljundi failivormingud|Media kodeerija Standard|Media kodeerija Premium töövoo
---|---|---
Adobe® Flash® F4V|Ei|Jah
MXF (OP1a, XDCAM ja AS02)|Ei|Jah
DPP (sh AS11)|Ei|Jah
GXF|Ei|Jah
MPEG-4/MP4|Jah|Jah
MPEG-TS|Jah|Jah
Windows Media/ASF|Ei|Jah
AVI (tihendamata 8/10 bitise)|Ei|Jah
Sujuva voogesituse failivorming (PIFF 1.3)|Ei|Jah

###<a name="output-video-codecs"></a>Väljundi videokodekid

Väljundi videokodekid|Media kodeerija Standard|Media kodeerija Premium töövoo
---|---|---
AVC (H.264; 8-bitise; kõrge profiil, kuni kõik 5.2; 4 K Ultra HD; AVC Sisene)|Ainult 8 bit 4:2:0|Jah
Innukas DNxHD (jaotises MXF)|Ei|Jah
MPEG-2 (kuni 422 profiili ja kõrge; nt XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10 variandid sh)|Ei|Jah
MPEG-1|Ei|Jah
Windows Media Video/VC-1|Ei|Jah
JPEG pisipiltide loomine|Ei|Jah

###<a name="output-audio-codecs"></a>Väljundi Helikodekid

Väljundi Helikodekid|Media kodeerija Standard|Media kodeerija Premium töövoo
---|---|---
AES (SMPTE 331 M ja 302 M, AES3 – 2003)|Ei|Jah
Dolby® digitaalne (AC3)|Ei|Jah
Dolby® Digital Plus (E-AC3) kuni 7.1|Ei|Jah
AAC (AAC-LC, AAC-HE ja AAC-HEv2; kuni 5,1)|Jah|Jah
MPEG Layer 2|Ei|Jah
MP3 (MPEG-1 Audio Layer 3)|Ei|Jah
Windows Media Audio|Ei|Jah


##<a name="error-codes"></a>Tõrkekoodid  

Järgmises tabelis on loetletud tõrkekoodid, mis võib tagastada juhul kodeering tööülesande täitmise ajal ilmnes tõrge.  Tõrke üksikasjade .NET-koodi saamiseks kasutage [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) klassi. Tõrke üksikasjade ülejäänud koodi saamiseks kasutage [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API-ga.

ErrorDetail.Code|Tõrke võimalikud põhjused
-----|-----------------------
Pole teada| Tööülesande täitmisel tundmatu tõrge
ErrorDownloadingInputAssetMalformedContent|Tõrgete kategooria, mis hõlmab tõrgete allalaadimine Sisestuskeel varade, nt vigased failinimedes null pikkus faile, mis on vale vormingud jne.
ErrorDownloadingInputAssetServiceFailure|Tõrgete kategooria, mis hõlmab teenuse side - näide allalaadimise ajal võrgus või salvestusruumi tõrgete lahendamine.
ErrorParsingConfiguration|Kategooria tõrkeid, kus tööülesande <see cref="MediaTask.PrivateData"/> (konfiguratsiooni) ei sobi, näiteks konfiguratsioon ei sobi süsteemi eelseadistatud või see sisaldab sobimatut XML-i.
ErrorExecutingTaskMalformedContent|Kategooria tõrkeid, kus sees Sisestuskeel meediumifailid probleeme põhjustada tööülesande täitmise ajal.
ErrorExecutingTaskUnsupportedFormat|Kategooria vigu, kus meediumi protsessor ei töödelda faile, mis on esitatud - meediumi vormindamine pole toetatud või ei vasta konfiguratsiooni. Näiteks, andes ainult heli väljund vara, mis sisaldab ainult video proovimist
ErrorProcessingTask|Muud tõrked meediumi protsessor tekib tööülesande töötlemise ajal, mis ei nõua sisu kategooria.
ErrorUploadingOutputAsset|Kategooria vigade üleslaadimisel väljundi vara
ErrorCancelingTask|Kategooria vigade katta tõrkeid, kui proovite tööülesande tühistamiseks
TransientError|Kategooria vigade kataks siirdamiseks probleeme (nt. ajutiste võrgu probleemid Azure Storage)


Avage **Media Services** meeskond abi saamiseks [tugiteenuste Piletite](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Seotud artiklid

- [Kohandada Media kodeerija Standard valmiskombinatsioonid Täpsemad kodeerimise ülesannete](media-services-custom-mes-presets-with-dotnet.md)
- [Kvootide ja -piirangud](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
