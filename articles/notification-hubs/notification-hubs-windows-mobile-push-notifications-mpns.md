<properties
    pageTitle="Tõuketeatiste Azure'i teatis jaoturi abil saatmise kohta Windows Phone | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas Windows Phone 8 või Windows Phone 8.1 Silverlighti rakenduse Tõuketeatiste Azure'i teatis jaoturi abil."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="tõuketeatised teatis, vajutage teatis, windows Phone'i tõuketeatised"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Tõuketeatiste Azure'i teatis jaoturi abil saatmise kohta Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

> [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Selle õpetuse näidatakse, kuidas kasutada Azure teatis jaoturi Windows Phone 8 või Windows Phone 8.1 Silverlighti rakenduse Tõuketeatiste saatmiseks. Kui olete suunatud Windows Phone 8.1 (mitte Silverlight), seejärel viidata [Ühes kohas Windowsi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) versiooni.
Selles õpetuses loote tühja Windows Phone 8 rakendus, mis saab Tõuketeatiste Microsoft tõuketeatised teatise teenuse (MPNS) abil. Kui olete lõpetanud, saate küll leviedastus Tõuketeatiste töötavad rakenduse seadmed teie teate jaoturi abil.

> [AZURE.NOTE] Teatis jaoturi Windows Phone SDK ei toeta soovitud Windows Push teatise teenuse (WNS) kasutamine Windows Phone 8.1 Silverlighti rakendused. WNS (mitte MPNS) kasutamine Windows Phone 8.1 Silverlighti rakendused, järgige [Teatis jaoturi - Windows Phone Silverlighti õpetuse], mis kasutab REST API-d.

Õpetuse näitab lihtsa leviedastuse stsenaarium teatis jaoturi abil sisse.

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ [Visual Studio 2012 kiire for Windows Phone]või uuem versioon.

Selle õpetuse lõpuleviimine on nõutav kõik teatis jaoturi õpetused Windows Phone 8 rakenduste.

##<a name="create-your-notification-hub"></a>Teie teate jaoturi loomine

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klõpsake (jooksul <i>sätted</i>) jaotise <b>Teatis teenuste</b> , klõpsake <b>Windows Phone (MPNS)</b> ja seejärel märkige ruut <b>Luba autentimata tõuketeatised</b> .</p>
</li>
</ol>

&emsp;&emsp;![Azure'i portaal - luba unathenticated tõuketeatised](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Teie jaoturi on nüüd loodud ja konfigureeritud saata autentimata teatis Windows Phone.

> [AZURE.NOTE] Selle õpetuse kasutab MPNS autentimata režiimis. MPNS autentimata režiim on sisse teatised, mille saate saata iga kanali piirangud. Teatis jaoturi toetab [MPNS autenditud režiimi](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) , mis võimaldab teil laadida oma sert.

##<a name="connecting-your-app-to-the-notification-hub"></a>Teate jaoturi rakenduse ühenduse loomine

1. Visual Studios, looge uus rakendus Windows Phone 8.

    ![Visual Studio - uue projekti - Windows Phone rakendus][13]

    Visual Studio 2013 Update 2 või uuemat versiooni, saate selle asemel luua Windows Phone Silverlighti rakenduse.

    ![Visual Studio - uue projekti - Blank App – Windows Phone Silverlighti][11]

2. Visual Studios, paremklõpsake lahendus ja seejärel klõpsake nuppu **Halda NuGet-paketid**.

    Kuvatakse dialoogiboks **Haldamine Nugeti pakettide** .

3. Otsige `WindowsAzure.Messaging.Managed` ja klõpsake nuppu **Installi**ja seejärel nõustuge kasutustingimustega.

    ![Visual Studio - Nugeti Package Manager][20]

    See allalaadimist, installid ja lisab teegi Azure'i sõnumside viide Windowsi, kasutades <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed Nugeti pakett</a>.

4. Avage fail App.xaml.cs ja lisage järgmine `using` laused:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Lisage järgmine kood **Application_Launching** meetod App.xaml.cs ülaosas.

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] **MyPushChannel** väärtus indeks on kasutatud otsingu olemasolev kanal [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) kogumine. Kui seal pole ühte, luua uue kirje nimega.
    
    Veenduge, et lisada nimi oma jaoturi ja nimetatakse **DefaultListenSharedAccessSignature** , mida te saada eelmises jaotises ühendusstring.
    Järgmine kood toob kanali URI rakenduse MPNS ja seejärel registreerib selle kanali URI teie teate jaoturi. Tagatakse ka mis kanali URI on registreeritud oma keskuses teatis iga kord, kui rakendus on käivitatud.

    >[AZURE.NOTE]Selle õpetuse saadab töölauateatise soovitud seade. Paani teatise saatmisel peab kutsuvad **BindToShellTile** meetodi asemel kanal. Toetab nii töölauateatisega ja paani teatised, helistage nii **BindToShellTile** ja **BindToShellToast**.

6. Solution Exploreris Laienda **Atribuudid**, avage soovitud `WMAppManifest.xml` fail, klõpsake vahekaarti **võimaluste** ja veenduge, et märgitud on **ID_CAP_PUSH_NOTIFICATION** võimalus.

    ![Visual Studio - Windows Phone rakenduse võimalused][14]

    See tagab, et teie rakenduse saab saata Tõuketeatiste. Ilma selleta, mis tahes tõuketeatised teatise saatmine rakendusse ei õnnestu.

7. Vajutage soovitud `F5` klahvi, et käivitada rakendus.

    Rakendus kuvatakse registreerimise sõnumi.

8. Sulgege rakendus.  

   >[AZURE.NOTE] Tõuketeatised on töölauateatise vastuvõtmiseks peab rakenduse töötama ei esiplaanil.

##<a name="send-push-notifications-from-your-backend"></a>Oma kirjutamata Tõuketeatiste saatmine

Tõuketeatised saate saata teate jaoturi kaudu mis tahes taustväärtus avaliku <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">ülejäänud kasutajaliidese</a>kaudu. Selles õpetuses saadate Tõuketeatiste kasutades .net-i konsooli rakendus. 

Kuidas saata Tõuketeatiste ASP.net-i WebAPI kirjutamata, mis on integreeritud teatis jaoturi näide leiate [Azure'i teatis jaoturi teavitamise kasutajad .NET taustväärtus](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Näide sellest, kuidas saata Tõuketeatiste [REST API-de](https://msdn.microsoft.com/library/azure/dn223264.aspx)abil kontrollida [teatis jaoturi kaudu Java kasutamise kohta](./notification-hubs-java-push-notification-tutorial.md) ja [Kuidas kasutada teatis jaoturi kaudu PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Paremklõpsake lahenduse, valige **Lisa** ja **Uus projekt …**ja seejärel klõpsake jaotises **Visual C#**, **Windowsi** ja **Konsooli rakendus**ja klõpsake nuppu **OK**.

    ![Visual Studio - uue projekti - konsooli rakendus][6]

    Sellega lisatakse uus Visual C# konsooli rakendus lahenduse kasutamist. Te saate seda teha ka eraldi lahendus.

4. Klõpsake nuppu **Tööriistad**, klõpsake **Teegi Package Manager**ja klõpsake **Package Manager konsooli**.

    Kuvatakse paketi Manager konsooli.

5.  **Paketi Manager konsooli** aknas määrata **vaikimisi projekti** uue projekti konsooli rakendus ja seejärel konsooli aken, käivitage järgmine käsk:

        Install-Package Microsoft.Azure.NotificationHubs

    Sellega lisatakse Azure'i teatis jaoturi SDK abil <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification jaoturi Nugeti pakett</a>viide.

6. Avage soovitud `Program.cs` faili ja lisada järgmine `using` lause:

        using Microsoft.Azure.NotificationHubs;

6. Klõpsake soovitud `Program` klassi, lisada järgmisel viisil:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Veenduge, et asendada selle `<hub name>` portaalis kuvatava teate jaoturi nime kohatäide. Lisaks ühenduse stringi kohatäite asendamiseks ühendusstringi nimetatakse **DefaultFullSharedAccessSignature** , mida te saada jaotises "Konfigureerige oma teate jaoturi."

    >[AZURE.NOTE]Veenduge, et ühendusstringi abil **täielik** juurdepääs **kuulata** juurdepääsu. Kuulata-access stringi ei saa saata Tõuketeatiste õigused.

4. Järgmine rea lisamine oma `Main` meetod:

         SendNotificationAsync();
         Console.ReadLine();

5. Oma opsüsteemi Windows Phone emulaator ja rakenduse sulgenud, määrata vaikimisi käivitus projekt konsooli rakendus projekti ja vajutage siis selle `F5` klahvi, et käivitada rakendus.

    Saate töölauateatis tõuketeatised teatis. Koputage töölauateatisega plakat laadib rakendus.

Kõigi võimalike lõhkelaengu leiate [töölauateatisega kataloogi] ja [paani kataloogi] teemade loendit veebisaidil MSDN-i.

##<a name="next-steps"></a>Järgmised sammud

Selles näites lihtsa eetris Tõuketeatiste Windows Phone 8 kõikides seadmetes. 

Selleks, et suunata kindlatele kasutajatele, vaadake [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele] õpetuse. 

Kui soovite segmendi kasutajate poolt huvirühmade, saate lugeda [Kasutamine teatis jaoturi uudiseid saata]. 

Lisateavet teate jaoturi kasutamine [Teatis jaoturi juhised].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Teatis jaoturi juhised]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Tõuketeatiste kasutajatele teatis jaoturi abil]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Teatis jaoturi abil saate saata uudiseid]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[töölauateatisega kataloog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[paani kataloog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Teatis jaoturi - Windows Phone Silverlighti õpetus]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

