<properties 
    pageTitle="Videopleieri rakenduste arendajatele." 
    description="Teema sisaldab linke mängija raamistik ja lisandmoodulid, mille abil saate arendada oma kliendi rakendused, mida saab kasutada Media Servicesi kaudu videote voogesitamise meediumi." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Videopleieri rakenduste arendajatele.

##<a name="overview"></a>Ülevaade

Azure Media Servicesi tööriistade, peate looma rikkalike, dünaamiliste Playeri klientrakendustes enamik platvormid, sh: iOS-seadmetes, Android-seadmetes, Windowsi, Windows Phone, Xbox ja Set-top ruudud. See teema sisaldab ka linke SDK-d ja Playeri raamistiku, mille abil saate arendada oma kliendi rakendused, mida saab kasutada Azure Media Servicesi kaudu videote voogesitamise meediumi.

##<a name="azure-media-player"></a>Azure Media Playeri

[Azure Media Player](http://aka.ms/ampinfo) on web videopleieri meediumisisu tagasi Microsoft Azure Media Servicesi kaudu erinevaid seadmed ja brauserid sisse ehitatud. Azure Media Player kasutab tööstusstandarditele, nt HTML5, allika laiendid (MSE) ja Media laiendid (EME) kohandatava täiustatud streaming rakenduseks. Kui need standardid ei ole saadaval seadmes või veebibrauseris, kasutab Azure Media Playeri taandepäringud tehnoloogia Flash ja Silverlight. Sõltumata kasutatav taasesitus tehnoloogia arendajad on ühendatud JavaScripti kasutajaliidese API-d juurdepääsu. See võimaldab kätte Azure Media Servicesi sisu üle mitmesuguste seadmed ja brauserid ilma mis tahes vaeva mängida.

Microsoft Azure Media Servicesi võimaldab sisu serveeritud MÕTTEKRIIPSU, sujuva voogesituse ja HLS streaming vormingud sisu. Azure Media Playeri arvestab neid erinevaid vorminguid ja automaatselt esitatakse parim link platvormi/brauseri võimaluste põhjal. Microsoft Azure Media Servicesi võimaldab dünaamiline krüptimise varade PlayReady krüptimise abil või AES 128-bitise ümbriku krüptimise. Azure Media Player võimaldab PlayReady dekrüptimine ja AES 128 bit krüptitud sisu, kui see on õigesti konfigureeritud. 

Lisateave:

- [Azure Media Playeri](http://aka.ms/ampinfo)
- [Azure Media Playeri dokumentatsioon](http://aka.ms/ampdocs) 
- [Azure Media Playeri kasutamise alustamine ajaveeb](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Logi sisse, et püsida kursis: Azure Media Playeri uusim](http://aka.ms/ampsignup)
- [Lisage uus soovitada funktsioone, ideede, tagasiside](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Muid tööriistu Playeri rakenduste loomine

Saate kasutada ka ühte järgmistest SDK-d

- [Sujuva voogesituse kliendi SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Sujuva voogesituse Windowsi poe rakendus](media-services-build-smooth-streaming-apps.md)
- [Microsoft Media platvorm: Mängija raamistik](http://playerframework.codeplex.com/) 
- [HTML5 Playeri raamistik dokumentatsioon](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft sujuva voogesituse lisandmooduli jaoks OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Hulgilitsentsimise Microsoft® sujuva voogesituse kliendi teisaldamise komplekt](http://aka.ms/sspk) 
- [XBOX Video, rakenduste arendamise](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Reklaami

Azure Media Servicesi toetab ad järjepunkti Windows Media platvormi kaudu: mängija raamistik. Windows 8, Silverlighti, Windows Phone 8 ja iOS-seadmete jaoks saadaolevaid Playeri raamistiku ad toega. Iga mängija framework sisaldab proovi kood, mis näitab teile, kuidas rakendada pleieri rakendus. On kolm erinevat tüüpi reklaamid, saate lisada oma meediumi.

Lineaarne – täielik paneeli reklaamid, mis peamine video peatamiseks

Mittelineaarne – ülekatet reklaamid, mis kuvatakse peamised video esitamise, tavaliselt logo või muude paigutatud mängija staatiline pilt

Companion – reklaamid, mis on kuvatud väljaspool mängija

Reklaamid paigutada põhilise video ajaskaala igal ajal. Rääkige mängija reklaami esitamise ja millised reklaamid esitamiseks. Seda saab teha mitu standardne XML-põhine faili abil: Video Ad teenuse mall (VAST), digitaalse Video mitme Ad loend (VMAP), meediumi abstraktsed järjestuse mall (MAST) ja digitaalse Video mängija Ad kasutajaliidese määratlus (VPAID). SUUR failide määrata, millised reklaamid kuvamiseks. VMAP failide määrata, millal esitada erinevad reklaamid ja sisaldavad suur XML-i. MAST failid on teine võimalus jada reklaamid, mis võib sisaldada suur XML-i ka. VPAID failide määratleda liidest videopleieri ja ad või ad serveri vahel. Lisateabe saamiseks vt [Reklaamid lisamine](https://msdn.microsoft.com/library/dn387398.aspx).

Suletud pealdis ja reklaamid tugi Live voogesituse kohta leiate teemast [toetatud suletud pealdis ja Ad järjepunkti standardid](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vt ka

[In HTML5 taotluse DASH.js MPEG-kriips kohandatava Streaming Video manustamine](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js hoidla](https://github.com/Dash-Industry-Forum/dash.js)
 
