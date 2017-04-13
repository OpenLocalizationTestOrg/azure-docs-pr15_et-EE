<properties 
    pageTitle="Media kodeerija standardseteks ja kodekite" 
    description="Selles teemas antakse ülevaade Media kodeerija standardseteks ja kodekite." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Meediumivorminguid kodeerija Standard ja kodekite


See dokument sisaldab loendit kõige levinum impordi ja ekspordi failivormingud, mida saate kasutada koos Media kodeerija Standard.


##<a name="input-containerfile-formats"></a>Sisestusmeetodi Container/failivormingud

Failivormingud (faililaiendid)|Toetatud
---|---|---|---
FLV (koos H.264 ja AAC kodekite) (FLV)          |Jah 
MXF (.mxf)                  |Jah 
GXF (.gxf)                  |Jah 
MPEG2-PS, MPEG2-TS, 3GP (TS, tippimisel, 3GP, .3gpp, .mpg)   |Jah 
Windows Media Video (WMV) / ASF (.wmv, .asf) |Jah 
AVI (tihendamata 8/10 bitise) (AVI)|Jah 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|Jah 
[Microsofti Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Jah 
Matroska/WebM (.mkv)        |Jah 
LAINE/WAV (.wav) |Jah 
QuickTime (.mov) |Jah

>[AZURE.NOTE] Ülal on veel peamiste faililaiendid loendit. Media kodeerija Standard ei toeta paljud teised (nt: MTS. M2TS, .mpeg2video, .qt). Kui proovite faili kodeerida ja teile kuvatakse tõrketeade vorming ei toeta, esitage tagasiside andmiseks [siin](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Heli Sisestuskeel ümbriste vormingud 

Media kodeerija Standard toetab, kes Sisestuskeel pakendis heli failivormingud:

- MXF, GXF ja QuickTime faile, mis on üksteise lülitada heli lugusid või 5.1 näidised

või

- Kui heli viiakse eraldi PCM rajad, kuid kanali vastendust (lülitada või 5.1) abil saate tuletada faili metaandmete MXF, GXF ja QuickTime failid

Pange tähele selle tugi jaoks konkreetsete/kasutaja esitatud kanali vastenduse tehakse kättesaadavaks lähitulevikus.


##<a name="input-video-codecs"></a>Sisestuskeel videokodekid

Sisestuskeel videokodekid|Toetatud
---|---|---|---
AVC 8-bitise/10-bitine, kuni 4:2:2, sh AVCIntra   |8 bit 4:2:0 ja 4:2:2 
Innukas DNxHD (jaotises MXF)                                 |Jah 
DVCPro/DVCProHD (jaotises MXF)                            |Jah 
Digitaalse video (DV) (AVI-failid)                   |Jah
JPEG 2000                                           |Jah 
MPEG-2 (kuni 422 profiili ja kõrge; nt XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10 variandid sh)|Kuni 422 profiil 
MPEG-1                                              |Jah 
VC-1/WMV9                                           |Jah 
Canopuse HQ/HQX                                      |Ei 
MPEG-4 osa 2                                       |Jah 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Jah 
YUV420 tihendamata või Standard                   |Jah
Apple ProRes 422                                    |Jah
Apple ProRes 422 LT |Jah
Apple ProRes 422 HQ |Jah
Apple ProRes puhverserveri|Jah
Apple ProRes 4444 |Jah
Apple ProRes 4444 XQ |Jah



##<a name="input-audio-codecs"></a>Sisestuskeel helikodekeid

Sisestuskeel helikodekeid|Toetatud
---|---|---|---
AAC (AAC-LC, AAC-HE ja AAC-HEv2; kuni 5,1)|Jah 
MPEG Layer 2|Jah 
MP3 (MPEG-1 Audio Layer 3)|Jah 
Windows Media Audio|Jah 
WAV-/ PCM|Jah 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Jah 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Jah 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Jah 
AMR (kohandatava mitme määr)|Jah
AES (SMPTE 331 M ja 302 M, AES3 – 2003)        |Ei 
Dolby® E                                    |Ei 
Dolby® digitaalne (AC3)                        |Ei 
Dolby® digitaalne pluss (E-AC3)                 |Ei 


##<a name="output-formats-and-codecs"></a>Väljundvormingusse ja kodekite

Järgmises tabelis on loetletud kodekite ja failivorminguid, mis on toetatud ekspordi.


Failivorming|Video kodek|Heli kodekiga
---|---|---
MP4 <br/><br/>(sh mitme bitikiirusega MP4 ümbriste) |H.264 (kõrge, põhi ja alusjoone profiilid)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2 TS |H.264 (kõrge, põhi ja alusjoone profiilid)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vt ka

[Nõudmisel sisu Azure Media Servicesi kodeerimine](media-services-encode-asset.md)

[Kuidas kodeerida koos Media kodeerija Standard](media-services-dotnet-encode-with-media-encoder-standard.md)
