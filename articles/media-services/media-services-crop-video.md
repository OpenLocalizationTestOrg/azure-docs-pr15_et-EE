<properties
    pageTitle="Video kärpimine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse kärpimine Media kodeerija Standard videod."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Media kodeerija Standard Kärbi videod

Saate kärpida sisendit video Media kodeerija Standardne (MES). Kärpimise on protsessi videot ristküliku aken, valides ja kodeering lihtsalt pikslite aknas sees. Järgmine diagramm aitab illustreerida protsess.

![Video kärpimine](./media/media-services-crop-video/media-services-crop-video01.png)

Oletame, et teil sisendina video, mis sisaldab resolutsioon 1920 x 1080 pikslit (16:9 proportsioonid), kuid on mustad ribad (samba loendiboksid) vasakule ja paremale, nii, et ainult 4:3 akna või 1440 x 1080 pikslit sisaldab aktiivse videot. MES abil kärpida või redigeerida välja mustad ribad ja kodeerida 1440 x 1080 piirkond.

MES kärpimine on eelnevalt töötlemiseks etapi, kärpimise parameetrid kodeering valmissäte rakendamine algse Sisestuskeel video. Kodeerimine on järgmise etapi ja laius ja kõrgus sätete rakendada soovitud *eelmääratletud töödeldav* video, mitte algse video. Teie valmissäte kujundamisel peate tegema järgmist: a põhjal algse Sisestuskeel video kärpimine parameetrid (b) valige ja oma kodeerida sätted kärbitud video põhjal. Kui te ei vasta teie kodeerida sätted kärbitud video, väljund ei ole ootuspäraselt.

[Teema](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) näitab, kuidas luua mõne kodeering töö MES ja luua kohandatud eelmääratud kodeering tööülesande määramine. 

## <a name="creating-a-custom-preset"></a>Luua kohandatud valmissäte

Näites joonisel:

1. Algne on 1920 x 1080
1. See peab olla kärbitud kuni 1440 x 1080, mis on keskele Sisestuskeel raami väljund
1. See tähendab, et mõne X offset (1920 – 1440) / 2 = 240 ja Y on null offset
1. Laius ja kõrgus Kärbi ristküliku on 1440 ja 1080, vastavalt
1. Kodeeri etapis esitage on kolm kihti esitada, on lahendused 1440 x 1080 960 x 720 ja 480 x 360 vastavalt

###<a name="json-preset"></a>JSON valmissäte


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Kärpimise piirangud

Kärpimise funktsiooni on mõeldud käsitsi. Peate Sisestuskeel video laadimiseks sobivad redigeerimise tööriista, mis võimaldab teil valige raame intressi, asetage kursor määratlemiseks korvamismeetmeid jaoks kärpimise ristkülik, määratlemiseks kodeering valmissäte, mis on häälestatud nii, et kindla video jne. See funktsioon on pole mõeldud, näiteks: automaattuvastus ja video sisendit musta postkasti/pillarbox ääriste eemaldamine.

Järgmised piiranguid rakendada kärpimise funktsiooni. Kui need on täidetud, saate encode tööülesande nurjuda, või põhjustada ootamatuid väljund.

1. Koordinaadid ja Kärbi ristküliku suuruse tuleb sobitu Sisestuskeel video
1. Nagu eespool, laius ja kõrgus kodeerida sätted on kärbitud video vastavad
1. Kärpimise kehtib horisontaalpaigutus videod (st nutitelefoni salvestatud videoid ei kehti hoitakse vertikaalselt või vertikaalpaigutus režiim)
1. Toimib kõige paremini järk-järgult jäädvustatud ruudukujuliste pikslit video

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Järgmise juhise juurde
 
Teemast Azure Media Servicesi õppeteemad aitavad pakutud AMS hea funktsioone tutvustava Õppevideo.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
