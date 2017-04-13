<properties
    pageTitle="  Azure'i portaalis sisu avaldada | Microsoft Azure'i"
    description="Selles õpetuses juhendab teid Azure'i portaalis sisu avaldamise juhiseid."
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

# <a name="publish-content-with-the-azure-portal"></a>Azure'i portaalis sisu avaldamine

> [AZURE.SELECTOR]
- [Portaal](media-services-portal-publish.md)
- [.NET-I](media-services-deliver-streaming-content.md)
- [ÜLEJÄÄNUD](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Ülevaade

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 

Pakkuda oma kasutaja URL, mida saab kasutada voogesituseks või teie sisu alla laadida, peate esmalt "avaldada" teie vara on locator loomisega. Lokaatorid pakuvad juurdepääsu vara failid. Media Services toetab kahte tüüpi lokaatorid. 

- Streaming (OnDemandOrigin) lokaatorid, kasutatakse kohandatav voogesitus (näiteks, et voo MPEG MÕTTEKRIIPSU, HLS või sujuva voogesituse). Teie vara streaming locator loomiseks peab sisaldama .ism failina. 
- Järk-järgult (SAS) lokaatorid, kasutatakse kohaletoimetamise video järk-järgult allalaadimise kaudu.


Streaming URL on järgmises vormingus ja abil saate esitada sujuva voogesituse varad.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Koostamiseks on HLS streaming URL-i, lisa (vormingus = m3u8-aapl) URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Koostamiseks on MPEG MÕTTEKRIIPSU streaming URL-i, lisa (vormingus = mpd-kellaaja-csf) URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS URL on järgmises vormingus.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Lisateavet leiate teemast [toimetamine sisu ülevaade](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Kui kasutasite portaali loomine lokaatorid enne märts 2015, loodi lokaatorid koos kahe aasta aegumise kuupäeva.  

Klõpsake soovitud locator aegumiskuupäeva värskendamiseks kasutada [ülejäänud](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) või [.net-i](http://go.microsoft.com/fwlink/?LinkID=533259) API-de. Pange tähele, et SAS locator aegumiskuupäeva värskendamisel URL muutub.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Kasutada portaali avaldada vara

Kasutada portaali vara avaldada, tehke järgmist.

1. Valige [Azure portaali](https://portal.azure.com/)konto Azure Media Servicesi.
1. Valige **sätted** > **varad**.
1. Valige varade, mille soovite avaldada.
1. Klõpsake nuppu **Avalda** .
1. Valige locator tüüp.
2. Vajutage **lisada**.

    ![Avaldamine](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL-i lisatakse **Avaldatud URL-ide**loendit.

## <a name="play-content-from-the-portal"></a>Portaalist sisu esitamine

Azure'i portaalis on sisu mängija, mille abil saate oma video testimiseks.

Klõpsake soovitud video ja seejärel klõpsake nuppu **Esita** .

![Avaldamine](./media/media-services-portal-vod-get-started/media-services-play.png)

Mõni järgmine kehtida.

- Veenduge, et video on avaldatud.
- See **Media Playeri** esitatakse vaikimisi streaming lõpp-punkti. Kui soovite esitada-vaikimisi streaming lõpp, klõpsake URL-i kopeerimine ja kasutamine teise mängija. Näiteks [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Peate kasutama, millest on streaming streaming lõpp-punkti.  
- Voogesituseks streaming lõpp-punkti, tuleks lisada streaming vähemalt ühe üksuse. Lisateavet leiate teemast [selle](media-services-portal-scale-streaming-endpoints.md) teema.   

##<a name="next-steps"></a>Järgmised sammud

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


