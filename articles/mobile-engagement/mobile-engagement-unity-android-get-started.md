<properties
    pageTitle="Azure'i Mobile kaasamine ühtsuse Androidi juurutamise kasutamise alustamine"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Analytics ja tõuketeatised ühtsuse rakenduste juurutamist iOS-seadmete jaoks."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Azure'i Mobile kaasamine ühtsuse Androidi juurutamise kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas kirjeldatakse, kuidas kasutada Azure Mobile kaasamine mõista oma rakenduse kasutamine ja kuidas Tõuketeatiste, et saata segmenditud kasutajad ühtsuse rakenduse Androidi seadme juurutamisel.
Selle õpetuse kasutab klassikaline ühtsuse tööks sõnu "pall" õpetuse lähtepunktina. Mida peaks järgige selles [õpetuses](mobile-engagement-unity-roll-a-ball.md) enne jätkamist me eksponeerivad allpool õpetuses Mobile kaasamine integratsiooniga. 

Selle õpetuse kasutamiseks on vaja järgmist:

+ [Ühtsuse redaktor](http://unity3d.com/get-unity)
+ [Mobiilse kaasamine ühtsuse SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Teie Androidi jaoks mõeldud Mobile kaasamine häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

###<a name="import-the-unity-package"></a>Ühtsuse paketi importimine

1. [Mobile kaasamine ühtsuse paketi](https://aka.ms/azmeunitysdk) ja salvestage see oma arvutisse. 

2. Minge **varad -> impordi paketi -> kohandatud paketi** ja valige ülaltoodud juhises allalaaditud paketi. 

    ![][70] 

3. Veenduge, et kõik failid on valitud, ja klõpsake nuppu **impordi** . 

    ![][71] 

4. Kui importimine on edukalt, kuvatakse projekti imporditud SDK failid.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Funktsiooni EngagementConfiguration värskendamine

1. Avage SDK kaust ja update **EngagementConfiguration** skriptifail on **ANDROIDI\_ühenduse\_STRINGI** abil saate varem saadud Azure portaali ühendusstring.  

    ![][73]

2. Salvestage fail 

3. Käivitada **faili -> kaasamine -> Loo Androidi manifesti**. See on lisandmoodul, mis on lisatud Mobile kaasamine SDK ja klõpsates automaatselt värskendada oma projekti sätted. 

    ![][74]

> [AZURE.IMPORTANT] Veenduge, et käivitada seda iga kord, kui värskendate **EngagementConfiguration** faili muidu teie muudatused ei kajastu rakendus. 

###<a name="configure-the-app-for-basic-tracking"></a>Põhilised jälgimise jaoks rakenduse konfigureerimine

1. **PlayerController** skripti, mis on seotud Playeri objekti redigeerimiseks avada. 

2. Lisage järgmine kasutamine lause:

        using Microsoft.Azure.Engagement.Unity;

3. Lisage järgmised selle `Start()` meetod
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Juurutada ja käivitage rakendus
Veenduge, et teil Androidi SDK enne selle ühtsuse rakenduse juurutamine oma seadmesse teie arvutisse installitud. 

1. Androidi seadme ühenduse arvuti. 

2. Avage **Fail -> sätted koostamine** 

    ![][40]

3. Valige **Androidi** ja seejärel klõpsake **Vahetamise platvorm**

    ![][51]

    ![][52]

4. Klõpsake **pleieri sätted** ja sisestage kehtiv komplekt identifikaator. 

    ![][53]

5. Lõpuks klõpsake **Koostamine ja käivitamine**

    ![][54]

6. Teil palutakse talletamiseks Androidi paketi kausta nimi. 

7. Kui kõik läheb hästi, siis paketi juurutatakse ühendatud seadmesse ja hakkamasaamiseks ühtsuse peaksite nägema oma telefonis! 

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Funktsiooni EngagementConfiguration värskendamine

1. Avage SDK kaust ja update **EngagementConfiguration** skriptifail on **ANDROIDI\_GOOGLE\_arvu** **Google projektinumber** saadud varem Google Cloud arendaja portaali. See on string väärtus seega veenduge, et lisada see topeltjutumärkides. 

    ![][75]

2. Salvestage fail. 

3. Käivitada **faili -> kaasamine -> Loo Androidi manifesti**. See on lisandmoodul, mis on lisatud Mobile kaasamine SDK ja klõpsates automaatselt värskendada oma projekti sätted. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Kui soovite saada teatisi rakenduse konfigureerimiseks

1. **PlayerController** skripti, mis on seotud Playeri objekti redigeerimiseks avada. 

2. Lisage järgmised selle `Start()` meetod

        EngagementReachAgent.Initialize();

3. Nüüd, kui rakendus on värskendatud, juurutada ja käivitage rakendus seadmes kohta allpool toodud juhiseid. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
