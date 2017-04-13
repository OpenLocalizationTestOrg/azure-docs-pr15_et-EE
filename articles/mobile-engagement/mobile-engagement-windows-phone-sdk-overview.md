<properties 
    pageTitle="Windows Phone Silverlighti SDK ülevaade" 
    description="Windows Phone Silverlighti SDK Azure mobiilsideseadmete kaasamine ülevaade"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlighti SDK ülevaade Azure mobiilsideseadmete kaasamine

Kuidas integreerida Azure Mobile Windows Phone Silverlighti rakenduse osalemine üksikasjade vaatamiseks, alustage siit. Kui soovite seda proovida esmalt, veenduge, et meie [õpetuse 15 minutit](mobile-engagement-windows-phone-get-started.md).

Klõpsake soovitud [SDK sisu](mobile-engagement-windows-phone-sdk-content.md) kuvamiseks

##<a name="integration-procedures"></a>Toimingute integreerimine

1. Alustage siit: [Kuidas integreerida Mobile Windows Phone Silverlighti rakenduse osalemine](mobile-engagement-windows-phone-integrate-engagement.md)

2. Teatiste: [Kuidas integreerida REACHi (teatised) Windows Phone Silverlighti rakenduse](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Lepingu täitmise sildistamine: [Täpsemalt Mobile kaasamine sildistamine API Windows Phone Silverlighti rakenduse kasutamise kohta](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Väljalaskemärkmed

###<a name="330-04192016"></a>3.3.0 (04/19/2016)
Osa *MicrosoftAzure.MobileEngagement* Nugeti paketti **v3.4.0**

-   Lisatud "TestLogLevel" API-luba või Keela/filtreerimine konsooli logid kiiratava SDK.

Varasema versiooni kohta leiate teemast [täielik väljalaskemärkmed](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Toimingute täiendamine

Kui teil juba on integreeritud vanemat versiooni meie SDK rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Teil võib olla mitu toimingute jälgimiseks, kui teil on vastamata SDK mitut versiooni. Lugege teemat [Täiendamine toimingute](mobile-engagement-windows-phone-upgrade-procedure.md)lõpule viidud. Näiteks kui migreerite 0.10.1 0.11.0 tuleb esmalt kasutage toimingut ", et 0.10.1 0.9.0" siis korras ", et 0.11.0 0.10.1".

###<a name="from-200-to-330"></a>Et 3.3.0 2.0.0 kaudu

####<a name="test-logs"></a>Logide testimine

Praegu saab konsooli logid toodeti SDK lubatud/keelatud/filtreeritud. See kohandamiseks atribuudi värskendamiseks `EngagementAgent.Instance.TestLogEnabled` üks väärtus, mis on saadaval on `EngagementTestLogLevel` loendamine, näiteks:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Vanema versiooni uuendamine

Vt [toimingute täiendamine](mobile-engagement-windows-phone-upgrade-procedure.md)
 
