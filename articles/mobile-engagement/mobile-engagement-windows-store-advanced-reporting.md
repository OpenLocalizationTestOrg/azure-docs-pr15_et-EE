<properties
    pageTitle="Windowsi universaalne Täpsemad aruandluse MobileApps kaasamine"
    description="Kuidas integreerida universaalne rakendused Windows Azure'i mobiilsideseadmete kaasamine"                  
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Täpsemad aruandluse kaasamine universaalne rakendused Windows SDK

> [AZURE.SELECTOR]
- [Universaalne Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Selles teemas kirjeldatakse täiendavad aruannete stsenaariumid ühes kohas Windowsi rakenduse. Need stsenaariumid hõlmavad suvandid, mida saate rakendada rakendusele loodud õpetusega [Alustamine](mobile-engagement-windows-store-dotnet-get-started.md) .

## <a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Selles õpetuses enne peate esmalt täitma [Alustamine](mobile-engagement-windows-store-dotnet-get-started.md) õpetuse, mis on teadlikult otsest ja lihtsat. Selles õpetuses käsitletakse lisasuvandid, saate valida.

## <a name="specifying-engagement-configuration-at-runtime"></a>Määrab käitusajal kaasamine konfigureerimine

Kaasamine konfigureerimine on tsentraliseeritud soovitud `Resources\EngagementConfiguration.xml` faili projekti, mis on juhul, kui see on määratud [Alustamine](mobile-engagement-windows-store-dotnet-get-started.md) teema.

Kuid saate selle määrata ka käitusajal: saate helistada enne kaasamine agent lähtestamine järgmisel viisil:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Soovitatav meetod: ülekoormuse oma `Page` tunnid

Kõik nõutud kaasamine arvutada kasutajaid, seansid, tegevused, jookseb ja tehniline statistika logid esitamine aktiveerimiseks tehke kõik teie `Page` sub tunnid pärivad selle `EngagementPage` tunnid.

Siin on näide lehe teie rakendus. Mida saab teha sama rakenduse kõik leheküljed.

### <a name="c-source-file"></a>C# lähtefail

Muutke oma lehe `.xaml.cs` faili:

-   Lisada oma `using` laused:

        using Microsoft.Azure.Engagement;

-   Asendage `Page` koos `EngagementPage`:

**Ilma kaasamine:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Kus lingid:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Kui teie lehe alistab selle `OnNavigatedTo` meetod, veenduge, et kõne `base.OnNavigatedTo(e)`. Muul juhul tegevuse ei ole teatatud (selle `EngagementPage` kõned `StartActivity` sees selle `OnNavigatedTo` meetod).

### <a name="xaml-file"></a>XAML-i fail

Muutke oma lehe `.xaml` faili:

-   Lisage oma nimeruumid deklaratsiooni.

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Asendage `Page` koos `engagement:EngagementPage`:

**Ilma kaasamine:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Kus lingid:**

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a>Vaikimisi sisse lülitatud alistamine

Vaikimisi on selle tunni nime lehe staatusega tegevuse nime, mille ilma täiendava. Kui ainekursuse kasutab järelliide "Leht", kaasamine eemaldab.

Alistada vaikekäitumise nimi, lisage järgmine kood:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Aruande täiendavat teavet oma tegevust, lisage järgmine kood:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Nende meetodite nimetatakse rakenduses selle `OnNavigatedTo` lehe meetod.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatiivne meetod: kõne `StartActivity()` käsitsi

Kui te ei saa või ei soovi ülekoormuse oma `Page` tunnid, selle asemel saate alustada oma tegevuste helistades `EngagementAgent` meetodid otse.

Soovitame helistaja `StartActivity` sees oma `OnNavigatedTo` lehe meetod.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Veenduge, et teie seansi lõpetamine õigesti.
>
> Universaalne Windows SDK kõnede automaatselt selle `EndActivity` meetod, kui rakendus on suletud. Seega on **väga** soovitatav helistamiseks on `StartActivity` iga kord, kui tegevuse kasutaja muutmine ja **mitte kunagi** kõne meetod on `EndActivity` meetod. Seda meetodit teavitab kaasamine server, et praeguse kasutaja on rakendus, mis mõjutavad kõik logid lahkunud.

## <a name="advanced-reporting"></a>Täpsem aruandlus

Teise võimalusena võite aruande teiste meetodite leitud kasutada rakenduse kohased sündmusi, tõrgete ja tööd teha, et `EngagementAgent` klassi. Kaasamine API võimaldab kõik lingid kasutamine täiustatud võimalused.

Lisateabe saamiseks vt [Täpsemalt Mobile kaasamine sildistamine API Windows universaalne rakenduse kasutamise kohta](mobile-engagement-windows-store-use-engagement-api.md).
