<properties 
    pageTitle="Windows universaalne rakendused SDK sisu" 
    description="Teave ühes kohas rakendused Windows SDK sisu Azure Mobile kaasamine"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-content"></a>Windows universaalne rakendused SDK sisu

Selle dokumendi loendid ja kirjeldatakse juurutatud SDK oma rakenduse sisu.

##<a name="the-resources-folder"></a>Funktsiooni `/Resources` kaust

See kaust sisaldab ressursse, mis tuleb Mobile allikaid. Saate kohandada neid vastavalt oma rakenduse.

- `EngagementConfiguration.xml`: Konfiguratsioonifail Mobile kaasamine, see on, kus saate kohandada Mobile kaasamine sätted (Mobile kaasamine ühendusstring, aruande krahh...).

### <a name="html-folder"></a>/HTML kausta

- `EngagementNotification.html`: Mida `Notification` veebi Kuva HTML-i kujundus-rakenduse lindid jaoks.

- `EngagementAnnouncement.html`: Mida `Announcement` veebi Kuva HTML-i kujundus-rakenduse interstitsiaalne vaadetes.

### <a name="images-folder"></a>/images kausta

- `EngagementIconNotification.png`: Kaubamärgi ikoon kuvatakse vasakus nurgas teatise asendamiseks oma kaubamärgi ikooni.

- `EngagementIconOk.png`: Mida `Ok` ikooni REACHi sisu lehtede nupule toimingu või valideerimine.

- `EngagementIconNOK.png`: Mida `NOK` ikooni kasutada, kui nupp Valideeri REACHi sisu lehed on keelatud.
 
- `EngagementIconClose.png`: Mida `Close` ikooni REACHi teatised ja sisu nuppu unusta.

### <a name="overlay-folder"></a>/overlay kausta

- `EngagementPageOverlay.cs`: Ülekatet lehe lisamine kaasamine eest vastutav saavutamiseks-rakenduse UI oma laps.
  
