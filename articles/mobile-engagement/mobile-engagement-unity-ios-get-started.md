<properties
    pageTitle="Alustamine Azure Mobile kaasamine ühtsuse iOS-i juurutamiseks"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Analytics ja tõuketeatised ühtsuse rakenduste juurutamist iOS-seadmete jaoks."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Alustamine Azure Mobile kaasamine ühtsuse iOS-i juurutamiseks

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas kirjeldatakse, kuidas Azure Mobile kaasamine abil saate ülevaate oma rakenduse kasutamine ja kuidas Tõuketeatiste, et saata segmenditud kasutajad ühtsuse rakenduse juurutamisel iOS-seadmes.
Selle õpetuse kasutab klassikaline ühtsuse tööks sõnu "pall" õpetuse lähtepunktina. Mida peaks järgige selles [õpetuses](mobile-engagement-unity-roll-a-ball.md) enne jätkamist me eksponeerivad allpool õpetuses Mobile kaasamine integratsiooniga. 

Selle õpetuse kasutamiseks on vaja järgmist:

+ [Ühtsuse redaktor](http://unity3d.com/get-unity)
+ [Mobiilse kaasamine ühtsuse SDK](https://aka.ms/azmeunitysdk)
+ XCode redaktor

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Mobile kaasamine oma iOS-i rakenduse häälestamine

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

1. Avage SDK kaust ja update **EngagementConfiguration** skriptifail soovitud **iOS-i\_ühenduse\_STRINGI** abil saate varem saadud Azure portaali ühendusstring.  

    ![][73]

2. Salvestage fail. 

###<a name="configure-the-app-for-basic-tracking"></a>Põhilised jälgimise jaoks rakenduse konfigureerimine

1. **PlayerController** skripti, mis on seotud Playeri objekti redigeerimiseks avada. 

2. Lisage järgmine kasutamine lause:

        using Microsoft.Azure.Engagement.Unity;

3. Lisage järgmised selle `Start()` meetod
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Juurutada ja käivitage rakendus

1. IOS-seadmes ühenduse arvuti. 

2. Avage **Fail -> sätted koostamine** 

    ![][40]

3. Valige **iOS-i** ja seejärel klõpsake **Vahetamise platvorm**

    ![][41]

    ![][42]

4. Klõpsake **Playeri sätted** ja sisestage kehtiv komplekt identifikaator. 

    ![][53]

5. Lõpuks klõpsake **Koostamine ja käivitamine**

    ![][54]

6. Teil palutakse talletamiseks iOS-i paketi kausta nimi. 

    ![][43]

7. Kui kõik läheb hästi, tuleb koostada projekt ja peaks avama XCode rakenduse kohta. 

8. Veenduge, et **esitlusega sidumiseks identifikaator** on õige projekti.  

    ![][75]

10. Käivitage rakendus XCode nii, et paketi juurutatakse seadmesse ja hakkamasaamiseks ühtsuse peaksite nägema oma telefonis! 

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil suhelda oma kasutajate ja jõuda tõuketeatisi ja rakenduse sõnumside kampaaniat kontekstis. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Te ei pea tegema mis tahes täiendavaid konfiguratsiooni rakenduse teatisi ja see on juba häälestatud seda.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
