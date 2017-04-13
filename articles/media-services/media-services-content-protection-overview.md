<properties 
    pageTitle="Kaitsmise ülevaade | Microsoft Azure'i" 
    description="See artikkel annab ülevaate sisu kaitse Media teenustega." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Sisu kaitsmise ülevaade


Microsoft Azure Media Servicesi võimaldab teil kaitsta oma meediumi jätab arvuti kaudu säilitamiseks, töötlemiseks ja kohaletoimetamise aeg. Media Services võimaldab teil esitada oma reaalajas ja tellitav sisu krüptitud dünaamiliselt täiustatud Standard (AES) (kasutades 128-bitine) või mis tahes põhi digitaalõiguste: Microsoft PlayReady, Google Widevine ja Apple'i FairPlay. Media Servicesi pakub ka teenust pakkuda AES võtmed ja DRM (PlayReady Widevine ja FairPlay) litsentside volitatud klientidele. 

Järgmisel pildil näitab AMS toetab sisu kaitse töövood. 

![Koos PlayReady kaitsmine](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Dünaamiline krüptimise saama, peate esmalt saada vähemalt üks streaming reserveeritud ühiku streaming lõpp-punkti, millest soovite voona krüptitud sisu.

Selles teemas selgitatakse [põhimõtet ja terminoloogia](media-services-content-protection-overview.md) oluline sisu kaitse AMS mõistmine. Teema sisaldab ka teemad, mis näitab, kuidas sisu kaitse ülesannete [lingid](media-services-content-protection-overview.md#common-scenarios) . 

##<a name="dynamic-encryption"></a>Dünaamiliste krüptimine

Microsoft Azure Media Servicesi võimaldab teil esitada oma sisu krüptitud dünaamiliselt AES tühjendage klahv või DRM krüptimise: Microsoft PlayReady, Google Widevine ja Apple'i FairPlay.

Praegu saab krüptida streaming failivormingud: HLS, MPEG MÕTTEKRIIPSU ja sujuva voogesituse. Te ei saa krüptida HDS streaming vorming või järkjärguline allalaadimine.

Kui soovite Media Servicesi vara krüptimiseks, peate seostada teie vara krüptimisvõtme (CommonEncryption või EnvelopeEncryption) ja ka konfigureerimiseks autoriseerimine poliitikate võti.

Samuti peate vara kohaletoimetamise poliitika konfigureerimine. Kui soovite voona salvestusruumi krüptitud varade, veenduge, et määrata, kuidas soovite pakkuda varade kohaletoimetamise poliitika konfigureerimisega.

Kui voo nõuab mängija, kasutab Media Servicesi määratud klahvi krüptimiseks dünaamiliselt oma sisu, kasutades AES tühjendage klahv või DRM krüptimine. Dekrüptida voo, mängija taotleb võti teenusest võtmed. Otsustada, kas kasutajal on õigus saada võti, hindab teenuse autoriseerimine poliitikate võti määratud.

>[AZURE.NOTE]Dünaamiline krüptimise ära, peate esmalt saada vähemalt üks nõudmisel streaming ühiku streaming lõpp-punkti, millest plaanite kohaletoimetamise krüptitud sisu. Lisateavet leiate teemast [skaala Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="storage-encryption"></a>Salvestusruumi krüptimine

Eemalda sisu kohalikult abil AES 256 bitises krüptimist krüptimiseks ja laadige Azure Storage salvestuskoha krüptitakse ülejäänud salvestusruumi krüptimise. Varade salvestusruumi krüptimise abil kaitstud automaatselt krüptimata ja enne kodeerimine süsteemi krüptitud faili asukoht ja soovi uuesti krüptitud enne üleslaadimist tagasi nimega uue väljundi varade. Salvestusruumi krüptimise esmane kasutamine puhul on, kui soovite oma kõrge kvaliteediga Sisestuskeel meediumifaile tugev krüptimist ja ülejäänud vaba.

Salvestusruumi krüptitud varade osutamiseks tuleb konfigureerida vara kohaletoimetamise poliitika, et Media Servicesi teaksid, kuidas soovite esitada oma sisu. Enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu sisu abil määratud kohaletoimetamise poliitika (nt AES, levinud krüptimise või krüptimine pole).

## <a name="common-encryption-cenc"></a>Levinud krüptimise (CENC)

Levinud krüptimise kasutatakse teie sisu PlayReady krüptimisel või / ja Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs-aapl krüptimine

Cbcs-aapl kasutatakse teie sisu FairPlay krüptimisel.

## <a name="envelope-encryption"></a>Ümbriku krüptimine 

Kasutage seda suvandit, kui soovite oma sisu kaitsta AES 128 kustutusklahvi. Kui soovite turvalisemad suvand, valige soovitud digitaalõiguste loetletud selles teemas. 

##<a name="licenses-and-keys-delivery-service"></a>Litsentside ja võtmed teenus

Media Servicesi pakub teenuseid DRM (PlayReady Widevine, FairPlay) litsentside kohaletoimetamiseks ja AES tühjendage volitatud klientidele. Saate konfigureerida autoriseerimine ja autentimise poliitika võtmed ja litsentside [Azure portaali](media-services-portal-protect-content.md), REST API-ga või Media Services SDK .net-i jaoks.

##<a name="token-restriction"></a>Turbeloa piirang

Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: avamine või sümboolne piirang. Turbeloa piiratud poliitika peab olema lisatud märgiks väljaandja järgi on turvaline Turbeloa teenus (STS). Media Services toetab sõned lihtne Web sõned (SWT) vorming ja JSON Web Turbeloa (JWT) vormingus. Media Servicesi ei paku Secure Turbeloa teenused. Saate luua kohandatud STS või kasutada Microsoft Azure ACS, et probleem sõned. Määratud võti ja probleemi nõuete Turbeloa piirang konfiguratsioonis määratud allkirjastatud märgiks loomiseks tuleb konfigureerida STS. Media Servicesi võtme teenus tagasi nõutud klahvi (või litsentside) kui luba on kehtiv ja taotluste Turbeloa kattuvad need konfigureeritud võti (või litsentside).

Kui poliitika konfigureerimise luba piiratud, peate määrama esmane kinnitamine klahvi, väljaandja ja sihtrühma parameetrid. Esmane kinnitamine võti sisaldab klahvi, mis on allkirjastatud luba, väljaandja on turvaline Turbeloa teenus, mis annab luba. Sihtrühma (mõnikord nimetatakse ulatus) kirjeldatakse soovidele vastavaks luba või ressursi luba lubab juurdepääsu. Media Servicesi võtme teenus kinnitab, et need väärtused luba vastavad väärtused mall.

##<a name="streaming-urls"></a>Streaming URL-id

Kui teie vara on rohkem kui üks DRM krüptitud, peaksite kasutama mõne krüptimise sildi streaming URL: (vormingus = 'm3u8 aapl' krüptimise = 'xxx').

Kehtivad järgmisega.

- Saate määrata ainult null või üks krüptimise tüüp.
- Krüptimise tüüp pole määratud URL-i, kui ainult ühe krüptimise rakendatud vara.
- Krüptimise tüüp on tõstutundlikud.
- Saate määrata krüptimise järgmist tüüpi:  
    - **Cenc**: levinud krüptimise (Playready või Widevine)
    - **cbcs-aapl**: Fairplay
    - **CBC**: AES ümbriku krüptimine.

##<a name="common-scenarios"></a>Levinumad stsenaariumid

Järgmistes teemades näitavad, kuidas salvestusruumi sisu kaitsta, dünaamiliselt krüptitud meediumi esitamisel, kasutage AMS klahv/litsentsi teenus

- [Mis AES kaitsmine](media-services-protect-with-aes128.md) 
- [PlayReady ja/või Widevine kaitsmine](media-services-protect-with-drm.md)
- [Voona HLS sisu kaitstud Apple'i FairPlay ja/või PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Täiendavad stsenaariumid

- [Kuidas oma krüptija/streaming server Azure'i PlayReady litsentsi teenuse integreerida](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [CastLabs esitamisel DRM litsentsid Azure Media Servicesi kaudu](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Seotud lingid

[Teenuse ja AES dünaamiline krüptimise abil Azure Media Servicesi PlayReady väljakuulutamist](http://mingfeiy.com/playready)

[Azure Media Services PlayReady litsentsi kohaletoimetamise hinnad ülevaade](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Kuidas silumine AES krüptitud voo Azure Media teenuste jaoks](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT Turbeloa authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Sujuv Azure Media Services OWIN MVC vastavalt rakenduse Azure Active Directory ja sisu võtme kohaletoimetamise JWT taotluste alusel piirata](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Kasuta Azure ACS probleemi sõned abil](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
