<properties
    pageTitle="Azure'i Mobile kaasamine Web SDK integreerimine | Microsoft Azure'i"
    description="Uusimate värskenduste ja Azure Mobile kaasamine Web SDK kord"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Sujuv veebirakenduse Azure Mobile kaasamine

> [AZURE.SELECTOR]
- [Windowsi universaalne](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Lihtsaim viis analüüsi- ja jälgimise Azure Mobile kaasamine oma veebirakenduse funktsioonide aktiveerimine on kirjeldatud selles artiklis toodud juhised.

Järgige Logi aruanded, mida on vaja arvutada kõik kasutajad, seansid, tegevused, jookseb ja technicals statistikat aktiveerimiseks. Rakenduse sõltuv statistika, nt sündmusi, tõrgete ja töö, peate aktiveerima aruannete käsitsi Azure Mobile kaasamine API abil. Lisateabe saamiseks vaadake, [Kuidas täpsemalt Mobile kaasamine sildistamine API veebirakenduse kasutama](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Sissejuhatus

[Azure'i mobiilsideseadmete kaasamine Web SDK alla laadida](http://aka.ms/P7b453).
JavaScripti ühe failina, saadetakse Mobile kaasamine Web SDK Azure'i engagement.js, mis tuleb kaasata igale leheküljele saidi või web rakenduse.

> [AZURE.IMPORTANT] Enne, kui käivitate selle skripti, peate käivitama skript või kood koodilõigu, kirjutate Mobile kaasamine rakenduse konfigureerimiseks.

## <a name="browser-compatibility"></a>Veebibrauseri ühilduvus

Mobile kaasamine Web SDK kasutab kohalikke JSON kodeerimise ja dekodeerimise Lisaks domeenide AJAXI taotlusi (tuginedes W3C CORS specification). See ühildub järgmiste brauseritega:

* Microsoft Edge 12 +
* Internet Explorer 10
* Firefox 3.5 +
* Chrome'i 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfigureerimine mobiilsideseadmete kaasamine

Kirjutage skripti, mis loob globaalse `azureEngagement` JavaScripti objekti, nagu järgmises näites. Kuna saidile võib-olla hulgidiagrammide lehtede, näites eeldatakse, et see skript sisaldab iga lehe. Selles näites on nimega objekti JavaScripti `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Funktsiooni `connectionString` väärtus kuvatakse rakenduse Azure'i portaalis.

> [AZURE.NOTE] `appVersionName`ja `appVersionCode` on valikulised. Soovitame siiski, et analüüsi saab versiooniteabe protsessi, nende konfigureerimine.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Lehtede Mobile kaasamine skriptide kaasamine
Mobile kaasamine skriptide lehtedele lisada ühel järgmistest viisidest:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Või selline:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias (pseudonüüm)

Pärast Mobile kaasamine Web SDK skripti laaditakse, loob see juurdepääs Tarkvaraarenduskomplektist API-de **kaasamine** pseudonüüm. Ei saa kasutada seda alias määratleda SDK konfigureerimine. Selle pseudonüümi kasutatakse seda dokumentatsiooni abimaterjalina.

Arvestage, et kui vaikimisi pseudonüüm vastuollu mõne muu Globaalne muutuja lehelt, te saate ümber määratleda konfiguratsiooni järgmiselt enne laadimist Mobile kaasamine Web SDK.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Tavaline teatamine

Tavaline aruandluse Mobile kaasamine hõlmab seansi taseme statistika, nt statistikat kasutajad, seansid, tegevused ja jookseb.

### <a name="session-tracking"></a>Seansi jälgimine

Seansi Mobile kaasamine on jagatud tegevuste jada, iga määratletud nimi.

Klassikaline veebisaidil, soovitame teil deklareerida muu tegevuse saidi igal lehel. Praeguse lehe on kunagi muutub veebisait või rakenduse, võiksite jälgida tegevuste väiksemad, näiteks lehe.

Mõlemal juhul käivitamine või praeguse kasutaja tegevuste muutmiseks, kõne on `engagement.agent.startActivity` funktsioon. Näiteks:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Mobile kaasamine server automaatselt lõpetab on avatud seansi kolme minuti jooksul pärast selle rakenduse leht on suletud.

Teise võimalusena saate lõpetada seansi käsitsi, helistades `engagement.agent.endActivity`. Seda määrab praeguse kasutaja tegevuse "Jõudeoleku."  Seansi lõpeb 10 sekundit hiljem, v.a juhul, kui uus kõne `engagement.agent.startActivity` kasutataks seanss.

Saate konfigureerida 10-sekundiline viivitus global suhete objekti, järgmiselt:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Te ei saa kasutada `engagement.agent.endActivity` klõpsake soovitud `onunload` tagasihelistamise Kuna te ei saa helistada AJAXI selles etapis.

## <a name="advanced-reporting"></a>Täpsem aruandlus

Soovi korral, kui soovite aruande rakendusele vastav sündmusi, tõrgete ja töö, peate kasutama Mobile kaasamine API. Pääsete Mobile kaasamine API kaudu soovitud `engagement.agent` objekti.

Pääsete juurde kõigile rakenduses Mobile kaasamine Mobile kaasamine API võimalusi. API on üksikasjalikult kirjeldatud artiklis [Täpsemalt Mobile kaasamine sildistamine API veebirakenduse kasutamise kohta](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>AJAXI kõnede jaoks kasutatavate URL-ide kohandamine

Saate kohandada Mobile kaasamine Web SDK kasutab URL-ide. Näiteks uuesti Logi URL (SDK lõpp-punkti jaoks logimine), võite ignoreerida konfiguratsiooni umbes järgmine:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Kui teie URL-i funktsioonid tagastavad string, mis algab `/`, `//`, `http://`, või `https://`, ei kasutata vaikesätted värviskeemi. Vaikimisi on `https://` skeem kasutatakse neid URL-idele. Kui soovite kohandada vaike värviskeemi, alistada konfiguratsiooni, umbes järgmine:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
