<properties 
    pageTitle="Media kodeerija Premium töövoo vormingute ja kodekite | Microsoft Azure'i" 
    description="Selles teemas antakse ülevaade kodeerija Premium töövoo meediumivorminguid vormingud ja kodekite" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media kodeerija Premium töövoo vormingud ja kodekite


>[AZURE.NOTE]Premium kodeerija küsimusi, e-posti mepd veebisaidil Microsoft.com.
>
>Selles teemas kirjeldatud Media kodeerija Premium töövoo meediumi protsessor pole saadaval Hiinas. 

See dokument sisaldab sisendi ja väljundi failivorminguid ja avaliku eelvaateversiooni **Media kodeerija Premium töövoo** kodeerija poolt toetatavad kodekite loendi.

[Kodeerija Premium Worflow sisendit meediumivorminguid ja kodekite](#input_formats)

[Kodeerija Premium Worflow väljundi meediumivorminguid ja kodekite](#output_formats)

**Media kodeerija Premium töövoo** toetab tiitreid [selles](#closed_captioning) jaotises kirjeldatud. 


##<a id="input_formats"></a>Media kodeerija Premium töövoo sisestada vormingud ja kodekite

Järgmises jaotises on loetletud kodekite ja failivorminguid, mis selle meediumi protsessor toetab sisendina.

###<a name="input-containerfile-formats"></a>Sisestusmeetodi Container/failivormingud

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2 Transport voogu
- MPEG-2 programmi voogu
- MPEG-4/MP4
- Windows Media/ASF
- AVI (tihendamata 8/10 bitise)

###<a name="input-video-codecs"></a>Sisestuskeel videokodekid

- AVC 8-bitise/10-bitine, kuni 4:2:2, sh AVCIntra
- Innukas DNxHD (jaotises MXF)
- DVCPro/DVCProHD (jaotises MXF)
- JPEG2000
- MPEG-2 (kuni 422 profiili ja kõrge; nt XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10 variandid sh)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Sisestuskeel helikodekeid

- AES (SMPTE 331 M ja 302 M, AES3 – 2003)
- Dolby® E
- Dolby® digitaalne (AC3)
- AAC (AAC-LC, AAC-HE ja AAC-HEv2; kuni 5,1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV-/ PCM
 
##<a id="output_format"></a>Media kodeerija Premium töövoo Väljundvormingusse ja kodekite

Järgmises jaotises on loetletud selle meediumi protsessori väljundina toetatud kodekite ja faili vormingute.

###<a name="output-containerfile-formats"></a>Container väljundi failivormingud

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM ja AS02)
- DPP (sh AS11)
- GXF
- MPEG-4/MP4
- Windows Media/ASF
- AVI (tihendamata 8/10 bitise)
- Sujuva voogesituse failivorming (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Väljundi videokodekid

- AVC (H.264; 8-bitise; kõrge profiil, kuni kõik 5.2; 4 K Ultra HD; AVC Sisene)
- Innukas DNxHD (jaotises MXF)
- DVCPro/DVCProHD (jaotises MXF)
- MPEG-2 (kuni 422 profiili ja kõrge; nt XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10 variandid sh)
- MPEG-1
- Windows Media Video/VC-1
- JPEG pisipiltide loomine

###<a name="output-audio-codecs"></a>Väljundi Helikodekid

- AES (SMPTE 331 M ja 302 M, AES3 – 2003)
- Dolby® digitaalne (AC3)
- Dolby® Digital Plus (E-AC3) kuni 7.1
- AAC (AAC-LC, AAC-HE ja AAC-HEv2; kuni 5,1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Tiitreid tugi

Sisse neelata, **Media kodeerija Premium töövoo** toetab:

1. SCC failid
1. SMPTE-TT failid
1. CEA-608/CEA-708 – kasutaja andmetena (SEI sõnumid on H.264 põhikooli voogu ATSC/53, SCTE20) või nimega täiendavate andmete MXF/GXF failid
1. STL alapealkiri failid

Väljundi, on saadaval järgmised suvandid:

1. CEA-608 CEA-708 tõlkimine
1. CEA 608/CEA 708 läbida (SEI sõnumid on H.264 põhikooli voogu manustatud või kantakse nii täiendavate andmete MXF failid)
1. SCC
1. SMPTE ajastatud tekst (allikatest CEA-608 SMPTE RP2052 kohta, sh DFXP faili loomine)
1. SRT alapealkiri faili
1. DVB alapealkiri voogu

Märkus: Kõik ülaltoodud väljundvormingusse on toetatud streaming Azure Media Servicesi kaudu.

##<a name="known-issues"></a>Teadaolevad probleemid

Kui Sisestuskeel video ei sisalda tiitrid, väljund varade sisaldavad endiselt tühja TTML faili. 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
