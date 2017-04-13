<properties 
    pageTitle="Taasesitus sisu | Microsoft Azure'i" 
    description="Selles teemas on loetletud olemasoleva mängijad, et saate taasesituse sisu." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Olemasoleva osalejatega sisu esitamine

Azure Media Servicesi toetab paljude populaarsete streaming vorminguid, nagu sujuva voogesituse, HTTP Live Streaming ja MPEG-mõttekriipsu. See teema osutab olemasoleva mängijad, mille abil saate oma voogu testida.

>[AZURE.NOTE]Esita dünaamiliselt pakitud või dünaamiliselt krüptitud sisu, veenduge, et saada vähemalt üks streaming ühiku streaming lõpp-punkti, millest soovite esitada oma sisu. Skaleerimist streaming üksuste kohta leiate artiklist: [Kuidas mastaapimiseks streaming üksused](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure'i portaali Media Servicesi sisu Playeri

**Azure'i** portaalis on sisu mängija, mille abil saate oma video testimiseks.

Klõpsake soovitud video (Veenduge, et see on [avaldatud](media-services-portal-publish.md)) ja klõpsake portaali allservas nuppu **Esita** .

Mõni järgmine kehtida.

- **MEEDIUMIPLEIERIT teenuste sisu** esitatakse vaikimisi streaming lõpp-punkti. Kui soovite esitada-vaikimisi streaming lõpp, kasutage teise mängija. Näiteks [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure Media Playeri

[Azure Media Playeri](http://amsplayer.azurewebsites.net/azuremediaplayer.html) abil taasesitus (Eemalda või kaitstud) sisu mis tahes järgmistes vormingutes:

- Sujuva voogesituse
- MPEG MÕTTEKRIIPSU
- HLS
- Järk-järgult MP4


###<a name="flash-player"></a>Flash Playeri

####<a name="aes-encrypted-with-token"></a>AES krüptitud luba abil

[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlighti pleierid

####<a name="monitoring"></a>Jälgimine

[http://SMF.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>Luba koos PlayReady

[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>MÕTTEKRIIPSU mängijad

[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Muud

Saate kasutada ka HLS URL-ide testimiseks tehke järgmist.

- **Safari** iOS-seadmes või
- Windowsi **3ivx HLS mängija** .

##<a name="developing-video-players"></a>Video mängijad arendamise

Arendada oma mängijad kohta leiate teemast [video mängijad arendamine](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
