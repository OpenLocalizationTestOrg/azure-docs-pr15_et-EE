<properties
    pageTitle=" Alustamine videosisu Azure'i portaalis nõudmisel | Microsoft Azure'i"
    description="Selles õpetuses juhendab teid rakendamiseks Video-on-Demand (VoD) sisu kohaletoimetamise põhiteenused Azure'i portaalis Azure Media Services (AMS) rakendusega juhiseid."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Alustamine videosisu Azure'i portaalis nõudmisel

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Selles õpetuses juhendab teid rakendamiseks Video-on-Demand (VoD) sisu kohaletoimetamise põhiteenused Azure'i portaalis Azure Media Services (AMS) rakendusega juhiseid.

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). 

Õppeteema sisaldab järgmisi toiminguid:

1.  Looge konto Azure Media Servicesi.
2.  Konfigureerige streaming lõpp-punkti.
1.  Video-või helifaili üleslaadimine
1.  Kodeerida lähtefail failikogumi kohandatava bitrate MP4.
1.  Varade ja streaming hankimine ja järk-järgult allalaadimine URL-ide avaldada.  
1.  Sisu esitamine


## <a name="create-an-azure-media-services-account"></a>Looge konto Azure Media Servicesi

Selle jaotise juhised näitab, kuidas luua konto AMS.

1. Logige sisse veebisaidil [Azure portaali](https://portal.azure.com/).
2. Valige **+ Uus** > **Web + Mobile** > **Media Services**.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Sisestage **MEDIA SERVICES konto loomine** nõutav väärtused.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Sisestage väljale **Konto nimi**uue AMS konto nimi. Media Servicesi konto nimi on kõik väiketähtedeks või -tähed ilma tühikuteta ja on 3 kuni 24 märki.
    2. Valige tellimus, vahel eri Azure'i tellimused, mida teil on juurdepääs.
    
    2. **Ressursirühm**valige uue või olemasoleva ressurss.  Ressursirühma on ressursid, millest jagada elutsükli, õiguste ja poliitikate kogum. Lisateavet [siin](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Asukoht**, valige geograafiliselt saab talletada meediumid ja metaandmete kirjeid Media Servicesi konto jaoks. See piirkond saab töödelda ja voogesitus. Ainult saadaval Media Servicesi piirkondade kuvatakse väljal ripploendist. 
    
    3. **Salvestusruumi konto**, valige salvestusruumi konto bloobimälu Media Servicesi kontolt meediumi sisu esitada. Saate valida olemasoleva salvestusruumi konto sama geograafilise piirkonna Media Servicesi konto või luua salvestusruumi konto. Piirkonna on loodud uue konto salvestusruumi. Salvestusruumi kontonimed reeglid on samad Media Servicesi kontod.

        Lisateavet mäluruumi [siin](storage-introduction.md).

    4. Valige **Kinnita armatuurlaua** konto juurutamise edenemise kuvamiseks.
    
7. Klõpsake nuppu **Loo** vormi allservas.

    Kui konto on loodud, olek muutub **töötab**. 

    ![Media Servicesi sätted](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS konto haldamiseks (nt videote üleslaadimine, kodeerida varad, töö edenemist jälgida) aknas **sätted** .

## <a name="manage-keys"></a>Klahvid haldamine

Peate konto nimi ja selle primaarvõtme teabe programmiliselt kontole juurde pääseda Media Services.

1. Azure'i portaalis, valige oma konto. 

    **Sätete** aken paremal. 

2. Valige aknas **sätted** **võtmed**. 

    Windowsi **haldamine** kuvatakse konto nimi ja esmaseid ja teiseseid klahvid ei kuvata. 
3. Väärtuste kopeerimiseks nuppu Kopeeri.
    
    ![Media Services võtmed](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Streaming lõpp-punktid konfigureerimine

Kui te töötate Azure Media Servicesi üks kõige levinumad stsenaariumid on pakkuda oma klientidele video voogesitus kohandatava bitrate kaudu. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Media Services pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 kodeeritud sisu voogesitamine pole vaja talletada eelnevalt pakitud versioonid iga järgmistes vormingutes streaming Media Services (MPEG SIDEKRIIPSU, HLS, sujuva voogesituse, HDS) lihtsalt õigel ajal, poolt toetatavad.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili failikogumi kohandatava bitrate MP4 (kodeering juhiseid on näidatud allpool selle õpetuse).  
- Looge vähemalt üks streaming ühiku *streaming lõpp-punkti* , millest plaanite kohaletoimetamise sisu. Kuidas muuta streaming ühikute alltoodud juhiseid kuvamine

Dünaamiliste pakendit, peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostab ja pakutakse sobiv vastus taotluste klientrakenduses.

Luua ja muuta streaming reserveeritud üksuste arvu, tehke järgmist.


1. Klõpsake aknas **sätted** **Streaming lõpp-punktid**. 

2. Klõpsake nuppu Vaikesäte streaming lõpp-punkti. 

    Kuvatakse aken **Vaikimisi STREAMING lõpp-punkti üksikasjad** .

3. Streaming ühikute määramiseks libistage liugurit **Streaming üksused** .

    ![Streaming ühikud](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klõpsake muudatuste salvestamiseks nuppu **Salvesta** .

    >[AZURE.NOTE]Mis tahes uute üksuste eraldatud kuluda kuni 20 minutit.

## <a name="upload-files"></a>Failide üleslaadimine

Voog Azure Media Servicesi kaudu videote, peate andmeallika videote üleslaadimine, kodeerida neid mitme bitikiirus ja avaldada. Selles jaotises käsitletakse kõigepealt. 

1. Klõpsake aknas **säte** **varad**.

    ![Failide üleslaadimine](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Klõpsake nuppu **üles laadida** .

    Kuvatakse aken **video vara üles laadida** .

    >[AZURE.NOTE] Ei ole faili mahupiirang.
    
4. Otsige sirvides üles soovitud video oma arvutist, valige see ja vajutage OK.  

    Käivitab üles ja näete edenemist faili nime all.  

Kui üleslaadimine on lõpule jõudnud, kuvatakse aknas **vara** loetletud uue vara. 

## <a name="encode-assets"></a>Kodeerida varad

Kui te töötate Azure Media Servicesi üks kõige levinumad stsenaariumid on kohandatava bitrate streaming pakkuda oma klientidele. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks. Videote ettevalmistamine kohandatava bitrate streaming, peate kodeerida allikas video mitme bitikiirusega failid. Kasutage **Media kodeerija Standard** kodeerija kodeerida videoid.  

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma mitme bitikiirusega MP4s streaming failivormingud: MPEG KRIIPSJOONE, HLS, sujuva voogesituse või HDS, ilma neid vorminguid streaming sisse pakkige. Dünaamiliste pakendit, peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostab ja pakutakse sobiv vastus taotluste klientrakenduses.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida lähtefaili failikogumi mitme bitikiirusega MP4 (kodeering juhiseid on näidatud allpool selles jaotises).
- Vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [konfigureerimise streaming lõpp-punktid](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### <a name="to-use-the-portal-to-encode"></a>Kasutada portaali kodeerida

Selles jaotises kirjeldatakse, mida ette võtta teie sisu Media kodeerija Standard kodeerida juhiseid.

1.  Valige aknas **sätted** **varad**.  
2.  Valige aknas **vara** vara, mida soovite kodeerida.
3.  **Encode** nupule.
4.  Valige aknas **vara Encode** "Media kodeerija Standard" protsessor ja valmissäte. Näiteks kui teate, et Sisestuskeel video on resolutsioon 1920 x 1080 pikslit, siis võite kasutada funktsiooni "H264 mitme Bitrate 1080p" eelseadistatud. Lisateabe saamiseks vt [käesoleva](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikli – valmiskombinatsioonid on oluline valige valmissäte, mis on kõige kohasem sisendit video. Kui teil on video madala eraldusvõime (640 x 360), siis te ei tohi kasutada vaikimisi "H264 mitme Bitrate 1080p" eelseadistatud.
    
    Lihtsam juhtimine, on teil võimalus redigeerimine väljundi varade nime ja töö nime.
        
    ![Kodeerida varad](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Vajutage **loomine**.

### <a name="monitor-encoding-job-progress"></a>Kodeerimine töö edenemise jälgimine

Kodeering töö jälgimiseks klõpsake nuppu **sätted** (lehe ülaservas) ja seejärel valige **tööde haldamine**.

![Tööde haldamine](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Sisu avaldamine

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

>[AZURE.NOTE] Kui kasutasite portaali loomine lokaatorid enne märts 2015, lokaatorid kahe aasta aegumiskuupäev on loodud.  

Klõpsake soovitud locator aegumiskuupäeva värskendamiseks kasutada [ülejäänud](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) või [.net-i](http://go.microsoft.com/fwlink/?LinkID=533259) API-de. Kui värskendate SAS locator aegumiskuupäeva, muutub URL-i.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Kasutada portaali avaldada vara

Kasutada portaali vara avaldada, tehke järgmist.

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

##<a name="next-steps"></a>Järgmised sammud

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


