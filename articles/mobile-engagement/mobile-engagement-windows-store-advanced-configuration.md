<properties
    pageTitle="Täpsemad Windows universaalne rakendused kaasamine SDK konfigureerimine"
    description="Universaalne rakendused Windows Azure'i Mobile kaasamine Lisavalikud konfigureerimine"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Täpsemad Windows universaalne rakendused kaasamine SDK konfigureerimine

> [AZURE.SELECTOR]
- [Universaalne Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

See toiming kirjeldab erinevaid võimalusi konfiguratsiooni Azure Mobile kaasamine Androidi rakenduste konfigureerimine.

## <a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Täiustatud konfigureerimine

### <a name="disable-automatic-crash-reporting"></a>Keela automaatne krahh teatamine

Saate keelata automaatse krahhi aruandluse funktsioon allikaid. Seejärel töötlemata erandi ilmnemisel kaasamine ei tee midagi.

> [AZURE.WARNING] Kui teil on selle funktsiooni keelata, siis kui ka töötlemata krahhi korral oma rakenduses kaasamine ei saada krahh **ja** sulgege seansi ja -tööde haldamine.

Automaatse krahhi aruandluse keelamiseks kohandada oma konfiguratsioon, sõltuvalt sellest, saate selle deklareeritud.

#### <a name="from-engagementconfigurationxml-file"></a>Kaudu `EngagementConfiguration.xml` fail

Aruande krahh määramine `false` vahel `<reportCrash>` ja `</reportCrash>` sildid.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Kaudu `EngagementConfiguration` objekti käitusajal

Aruande krahh väärtuseks false, mis on teie EngagementConfiguration objekti abil.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Reaalajas aruandlus keelamine

Vaikimisi logib reaalajas kaasamine teenuse aruandeid. Kui rakenduse aruannete logid sageli, on parem puhvri logid ja teatada need kõik korraga tavalise põhjal. Seda nimetatakse "lõhkemise režiimi".

Selleks helistada meetodit.

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument on väärtus **millisekundites**. Iga kord, kui soovite uuesti aktiveerida reaalajas logimine, helistada, või väärtuse 0 ilma iga parameetri meetodit.

Sarivõte veidi suureneb aku, kuid mõjutab kaasamine kuvari: kõik seansid ja -tööde haldamine kestus ümardatakse lõhkemist lävi (seega seansid ja -tööde haldamine lühem kui lõhkemist lävi ei pruugi olla nähtavad). Soovitame kasutada lõhkemist piirmäära enam kui 30000 (30s). Salvestatud logid on piiratud 300 üksust. Kui saatmine on liiga pikk, kaotate mõned logid.

> [AZURE.WARNING] Burst lävi ei saa konfigureerida ajavahemikul väiksem kui üks teine. Kui te seda teete, SDK kuvatakse tõrke jälitusteave ja automaatselt lähtestatakse vaikeväärtus, null sekundit. See käivitab SDK logid reaalajas teatada.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
