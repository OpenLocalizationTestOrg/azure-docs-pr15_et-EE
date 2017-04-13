<properties
    pageTitle="Windows SDK ühes kohas integreerimine"
    description="Windows universaalne integratsioon SDK Azure mobiilsideseadmete kaasamine"                                     
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Windows SDK ühes kohas integreerimine Azure mobiilsideseadmete kaasamine

Selles dokumendis kirjeldatakse kõik integratsioon ja konfiguratsiooni suvandid saadaval Azure Mobile kaasamine Windows universaalne SDK.

## <a name="prerequisites"></a>Eeltingimused

Selles õpetuses enne peate esmalt täitma meie [15-minutiline õpetuse](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Täpsemad funktsioonid

### <a name="reporting-features"></a>Aruandlusfunktsioonide
Saate lisada järgmisi funktsioone:

1. [Aruandlusteenuste Lisavalikud](mobile-engagement-windows-store-advanced-reporting.md)

2. [Konfiguratsiooni Täpsemad suvandid](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Teatised

[Kuidas integreerida REACHi (teatised) Windows universaalne rakenduse](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Sildi lepingu rakendamine:

[Täpsem Mobile kaasamine sildistamine API ühes kohas Windowsi rakenduse kasutamise kohta](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Väljalaskemärkmed

###<a name="340-04192016"></a>3.4.0 (04/19/2016)

-   Saavutamiseks ülekatet täiustused.
-   Lisatud "TestLogLevel" API-luba või Keela/filtreerimine konsooli logid kiiratava SDK.
-   Fikseeritud-tegevuse teatised suunamise esimese tegevusega ei kuvata rakenduse käivitamine.

Varasemates versioonides, vt [täielik väljalaskemärkmed](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Toimingute täiendamine

Kui teil juba on integreeritud vanemat versiooni kaasamine rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Kui te vastamata SDK mitmes versioonis, peate järgima mitu korda. Lugege teemat [Täiendamine toimingute](mobile-engagement-windows-store-upgrade-procedure.md)lõpule viidud. Näiteks kui migreerite 0.10.1 0.11.0 tuleb esmalt kasutage toimingut ", et 0.10.1 0.9.0" siis korras ", et 0.11.0 0.10.1".

###<a name="from-330-to-340"></a>Et 3.4.0 3.3.0 kaudu

####<a name="test-logs"></a>Logide testimine

Praegu saab konsooli logid toodeti SDK lubatud/keelatud/filtreeritud. Atribuudi värskendamiseks kohandamiseks `EngagementAgent.Instance.TestLogEnabled` ühte saadaolevatest väärtused on `EngagementTestLogLevel` loendamine, näiteks:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Ressursid

REACHi ülekatet on täiustatud. See on osa SDK Nugeti pakett ressursse.

Täiendamisel ilmneb SDK uue versiooni, saate valida, kas soovite hoida olemasolevaid faile oma ressurssi kaustast ülekate või mitte.

* Kui eelmine ülekatet töötab teie eest või teil on integreerimine on `WebView` elementide käsitsi, siis saate otsustate oma väljumist faile, see ikkagi tööd.
* Uue ülekatet värskendamiseks asendada kogu `overlay` kausta oma ressursid uue SDK pakett (UWP rakendused: pärast versiooniuuendust, saate uue kausta ülekatet KASUTAJAPROFIILI %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Uue ülekatet abil kirjutab eelmise versiooni tehtud kohandused.

### <a name="upgrade-from-older-versions"></a>Vanema versiooni uuendamine

Vt [toimingute täiendamine](mobile-engagement-windows-store-upgrade-procedure.md)
