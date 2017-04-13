<properties
    pageTitle="Veebirakenduste kasutamise alustamine Azure Mobile kaasamine | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Kasutusanalüüsi ja push teatised Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Azure'i Mobile kaasamine Web Appsi kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas näidatakse, kuidas kasutada Azure Mobile kaasamine mõista teie Web Appi kasutamine.

Selle õpetuse kasutamiseks on vaja järgmist:

+ Visual Studio 2015 või mõni muu editor teie valitud
+ [Web SDK](http://aka.ms/P7b453) 

Selle Web SDK eelvaade on ainult Analytics toetab praegu ja ei toeta saatmise brauseris või rakenduse Tõuketeatiste veel. 

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Häälestamise Mobile kaasamine veebirakenduse jaoks

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse esitab "lihtsa ühendamine," mis on minimaalne vajalik andmete kogumine määramine.

Loome lihtsa veebirakenduse integreerimine näidata, kuigi juhiste veebirakendusega, luuakse ka väljaspool Visual Studio Visual Studio. 

###<a name="create-a-new-web-app"></a>Uue veebirakenduse loomine

Järgmiste juhiste korral eeldatakse Visual Studio 2015 kasutamine kuigi juhiseid sarnanevad Visual Studio varasemates versioonides. 

1. Käivitage Visual Studio ja valige Kuva **Avaleht** **Uue projekti**.

2. Märkige hüpikaknas **Web** -> **ASP.net-i veebirakenduse**. Täitke rakenduse **nime**, **asukoha** ja **lahenduse nimi**, ja seejärel klõpsake nuppu **OK**.

3. Valige **tühi** jaotises **ASP.net-i 4.5 Mallid** hüpikaknas **Valige mall** ja klõpsake nuppu **OK**. 

Nüüd olete loonud uue tühja veebirakenduse projekti kuhu me Azure Mobile kaasamine Web SDK integreerida.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Ühenduse loomine rakenduse Mobile kaasamine taustväärtus

1. Saate luua uue kausta nimega **JavaScripti** teie lahendus ja Web SDK JS faili **azure-engagement.js** lisada. 

2. Lisage uus fail nimega **main.js** selle JavaScripti kausta järgmine kood. Veenduge, et värskendada ühendusstring. See `azureEngagement` Web SDK meetodite juurdepääsuks kasutatakse objekti. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio js-failidega][1]

##<a name="enable-real-time-monitoring"></a>Reaalajas jälgimise lubamine

Andmete saatmine ja tagada, et kasutajad on aktiivne käivitamiseks peate saatma vähemalt üks tegevuse Mobile kaasamine taustväärtus. Tegevuse kontekstis web app on veebilehele. 

1. Tuntud **home.html** teie lahendus uue lehe loomine ja selle seadmine avalehte veebirakenduse jaoks. 
2. Lisage kaks JavaScripti, lisasime eelnevalt kirjeldatud sellelt lehelt, lisades sees keha tag järgmist. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Värskendada keha tag helistamiseks EngagementAgent's `startActivity` meetod
        
        <body onload="engagement.agent.startActivity('Home')">

4. Siin on teie **home.html** näevad
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Kasutusanalüüsi laiendamine

Siin on kõik meetodid Web SDK, mille abil saate Analyticsi praegu saadaval.

1. Tegevuste veebilehed:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Sündmused
        
        engagement.agent.sendEvent(name, extras);

3. Tõrked

        engagement.agent.sendError(name, extras);

4. Tööde haldamine

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

