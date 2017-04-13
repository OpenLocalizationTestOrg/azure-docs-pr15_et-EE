<properties 
    pageTitle="CastLabs esitamisel Widevine litsentside Azure Media Servicesi kaudu | Microsoft Azure'i" 
    description="Selles artiklis kirjeldatakse, kuidas saate kasutada Azure Media Services (AMS) voo, mis on dünaamiliselt krüptitud AMS nii PlayReady ja Widevine digitaalõiguste esitamisel. PlayReady litsentsi pärineb Media Services PlayReady litsentsi serveri ja Widevine litsentsi on esitatud castLabs litsentsi serveri." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>CastLabs esitamisel Widevine litsentside Azure Media Servicesi kaudu

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas saate kasutada Azure Media Services (AMS) voo, mis on dünaamiliselt krüptitud AMS nii PlayReady ja Widevine digitaalõiguste esitamisel. PlayReady litsentsi pärineb Media Services PlayReady litsentsi serveri ja Widevine litsentsi on esitatud **castLabs** litsentsi serveri.

Taasesitus on kaitstud, CENC (PlayReady ja/või Widevine), saate kasutada [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Üksikasjad leiate [AMP dokumendi](http://amp.azure.net/libs/amp/latest/docs/) .

Järgmine diagramm näitab üksikasjalik Azure Media Servicesi ja castLabs integreerimine arhitektuur.

![integreerimine](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Tüüpilised süsteemi häälestamine

- Meediumisisu talletatakse AMS.
- CastLabs nii AMS on talletatud sisu klahvid võti ID-d.
- castLabs ja AMS on ehitatud Turbeloa autentimist. Järgmistes lõikudes käsitletakse autentimine märkide. 
- Kui kliendi taotlustele voo video, sisu dünaamiliselt krüptitud **Levinud krüptimise** (CENC) ja dünaamiliselt pakitud AMS sujuva voogesituse ja kriips järgi. Pakume ka PlayReady M2TS põhikooli voo krüptimise HLS streaming protokolli.
- PlayReady litsentsi on AMS litsentsi serverist alla laadida ja Widevine litsents on castLabs litsents serverist alla laadida. 
- Media Playeri otsustab automaatselt, millist litsentsi toomiseks vastavalt kliendi platvormi võimalus. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Saada litsentsi autentimise Turbeloa loomine

Nii castLabs AMS toetavad JWT (JSON Web Turbeloa) Turbeloa vormingus, mida kasutatakse autoriseerida litsentsi. 

###<a name="jwt-token-in-ams"></a>JWT luba AMS 

Järgmises tabelis kirjeldatakse AMS JWT luba. 

Väljaandja|Valitud väljaandja tekstistringi Secure Turbeloa teenus (STS)
---|---
Sihtrühma|Sihtrühma tekstistringi kasutatud STS
Nõuded|Kogum, nõuded
NotBefore|Luba kehtivust käivitamine
Lõppemist|Lõpeta kehtivust luba
SigningCredentials|Võti, mis on jagada PlayReady litsentsi Server, castLabs litsentsi serveri ja STS, võib põhjus olla sümmeetriline või asümmeetriline võti.

###<a name="jwt-token-in-castlabs"></a>JWT luba castLabs

Järgmises tabelis kirjeldatakse JWT luba castLabs. 

Nimi|Kirjeldus
---|---
optData|JSON string, mis sisaldab teavet teie kohta. 
CRT|JSON string, mis sisaldab teavet varade, litsentsi teave ja taasesitus õigused.
IAT|On praeguse kuupäeva ja kellaaja epohhi sisse.
JTI|Ainuidentifikaator selle märgiks (iga luba kasutada ainult üks kord castLabs süsteemi) kohta.

##<a name="sample-solution-set-up"></a>Valimi lahenduse häälestamine 

[Valimi lahendus](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) koosneb kahest projektide:

-   Konsooli rakendus, mida saab kasutada nii PlayReady ja Widevine juba sissevõetava vara, DRM piirangute seadmine.
-   Veebirakenduse, et käed läbi märgid, mida võib mõni STS KEELEVÄLJA lihtsustatud versiooni.


Konsooli rakenduse kasutamiseks tehke järgmist.

1.  Saate muuta app.config häälestamine AMS mandaat, castLabs mandaat, STS konfigureerimine ja ühiskasutuses võti.
2.  Laadige vara AMS sisse.
3.  Funktsiooni UUID toomine üleslaaditud varade ja muuta joone 32 Program.cs faili:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Kasutada mõne AssetId nime panemise vara castLabs süsteemis (joon 44 failis Program.cs).

    Peate määrama AssetId **castLabs**; See peab olema kordumatu tähtedest ja numbritest koosnev string.

5.  Käivitage programm.


Web rakenduse (STS) kasutamiseks tehke järgmist.

1.  Muuta web.config häälestus castlabs kaupmees ID-d, STS konfiguratsiooni ja ühiskasutuses klahvi.
2.  Juurutada Azure veebisaitide.
3.  Liikuge veebisaidi.

##<a name="playing-back-a-video"></a>Video esitamise

Taasesitus video krüptitud levinud krüptimise (PlayReady ja/või Widevine), saate [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Konsooli rakenduse käivitamisel kajastuvad sisu võtme ID ja näidata URL-i.

1.  Avage uus vahekaart ja käivitage oma STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Minge [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Kleepige URL-i streaming.
4.  Klõpsake nuppu **Täpsemad suvandid** ruut.
5.  Valige rippmenüüst **kaitse** PlayReady ja/või Widevine.
6.  Kleepige luba, et saite oma STS luba tekstiväljale. 
    
    CastLab litsentsi server ei pea selle "esitaja =" eesliite ette luba. Seega palun eemaldada, et enne esitamist luba.
7.  Värskendage mängija.
8.  Video tuleks esitamine.


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
