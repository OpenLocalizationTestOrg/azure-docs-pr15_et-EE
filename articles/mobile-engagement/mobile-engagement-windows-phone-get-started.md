<properties
    pageTitle="Azure'i Mobile kaasamine for Windows Phone Silverlighti Appsi kasutamise alustamine"
    description="Saate teada, kuidas kasutada Azure Mobile kaasamine Kasutusanalüüsi ja push teatised Windows Phone Silverlighti rakendused."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Azure'i Mobile kaasamine for Windows Phone Silverlighti Appsi kasutamise alustamine

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Selles teemas näidatakse, kuidas kasutada Azure Mobile kaasamine mõista teie rakenduse kasutamise ja saata Tõuketeatiste segmenditud Windows Phone Silverlighti rakenduse kasutajatele.
Selle õpetuse näitab lihtsa leviedastuse stsenaariumi, kasutades Mobile allikaid. Saate luua tühja Windows Phone Silverlighti rakendus, mis kogub lähteandmete ja tõuketeatised Microsoft tõuketeatised teatise teenuse (MPNS) abil saab.

> [AZURE.NOTE] Kui olete suunatud Windows Phone 8.1 (mitte Silverlight), vaadake [Windowsi universaalne õpetuse](mobile-engagement-windows-store-dotnet-get-started.md).

Selle õpetuse kasutamiseks on vaja järgmist:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nugeti pakett

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Rakenduse Windows Phone Mobile kaasamine häälestamine

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

Selle õpetuse esitab "lihtsa integratsioon", mis on nõutav andmete kogumine ja tõuketeatised teatise saatmine minimaalsete määramine. Täielik integratsioon dokumentatsiooni kohta leiate [Mobile kaasamine Windows Phone SDK integreerimine](mobile-engagement-windows-phone-sdk-overview.md)

Loome lihtsa rakenduse Visual Studio näidata integreerimine.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Windows Phone Silverlighti uue projekti loomine

Järgmiste juhiste korral eeldatakse Visual Studio 2015 kasutamine kuigi juhiseid sarnanevad Visual Studio varasemates versioonides. 

1. Käivitage Visual Studio ja valige Kuva **Avaleht** **Uue projekti**.

2. Märkige hüpikaknas **Windows 8** -> **Windows Phone** -> **Tühja rakendust (Windows Phone Silverlight)**. Täitke rakenduse **nimi** ja **lahenduse nimi**ja seejärel klõpsake nuppu **OK**.

    ![][1]

3. Saate valida, kas **Windows Phone 8.0** või **Windows Phone 8.1**suunata.

Nüüd olete loonud uue Windows Phone Silverlighti rakenduse kuhu me Azure Mobile kaasamine SDK integreerida.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Rakenduse ühenduse kirjutamata Mobile kaasamine

1. Installige [MicrosoftAzure.MobileEngagement] Nugeti pakett projektis.

2. Avatud `WMAppManifest.xml` (jaotises kausta Atribuudid) ja veenduge, et on (lisamine neid, kui nad pole) sisse selle `<Capabilities />` silt:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Nüüd kleepige varem kopeeritud oma Mobile kaasamine rakenduse ühendusstringi ja kleepige see soovitud `Resources\EngagementConfiguration.xml` faili vahel on `<connectionString>` ja `</connectionString>` Sildid:

    ![][3]

4. Klõpsake soovitud `App.xaml.cs` faili:

    lisamine. Lisage soovitud `using` lause:

            using Microsoft.Azure.Engagement;

    b. Rakenduses SDK lähtestada selle `Application_Launching` meetod:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Lisada järgmist funktsiooni `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Reaalajas jälgimise lubamine

Andmete saatmine ja tagada, et kasutajad on aktiivne käivitamiseks peate saatma Mobile kaasamine taustväärtus vähemalt ühe ekraanikuva (tegevus).

1. MainPage.xaml.cs, lisage soovitud `using` lause:

        using Microsoft.Azure.Engagement;

2. Asendage **Avaleht**, mis on enne **PhoneApplicationPage**, alus klassi **EngagementPage**.

        class MainPage : EngagementPage 
    
3. Klõpsake oma `MainPage.xml` faili:

    lisamine. Lisage oma nimeruumid deklaratsiooni.

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Asendage `phone:PhoneApplicationPage` XML-i sildi nimi koos `engagement:EngagementPage`.

##<a id="monitor"></a>Rakenduse kasutajaga reaalajas jälgimine

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Tõuketeatised ja sõnumside rakenduse lubamine

Mobile kaasamine võimaldab teil suhelda ja saavutamiseks kasutajate tõuketeatised ja-rakenduse sõnumside kampaaniat kontekstis. Selle mooduli nimetatakse REACHi Mobile kaasamine portaalis.
Järgmistes jaotistes häälestada rakenduse neid vastu võtta.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Lubage oma rakenduse MPNS tõuketeatised vastuvõtmiseks

Uued võimalused, kui soovite lisada oma `WMAppManifest.xml` faili:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>REACHi SDK lähtestamine

1. Klõpsake `App.xaml.cs`, kõne `EngagementReach.Instance.Init();` **Application_Launching** funktsiooni, paremklõpsake pärast agent lähtestamine:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. Klõpsake `App.xaml.cs`, kõne `EngagementReach.Instance.OnActivated(e);` **Application_Activated** funktsiooni, paremklõpsake pärast agent lähtestamine:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Kõik on valmis. Nüüd me veenduge, et teil on õigesti hüüdis selle lihtsa integreerimine.

##<a id="send"></a>Rakenduse teatise saatmine

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Nüüd peaks nähtaval olema teatis kuvatakse teie seadmes kui teatis – rakenduse kui rakendus on avatud, muidu töölauateatise, näiteks järgmine: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
