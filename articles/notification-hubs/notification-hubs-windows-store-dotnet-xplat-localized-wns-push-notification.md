<properties
    pageTitle="Teatis jaoturi lokaliseeritud server uudiste õpetus"
    description="Saate teada, kuidas Azure'i teatis jaoturi abil saate saata lokaliseeritud server uudiste teatised."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Lokaliseeritud uudiseid saata teate jaoturi abil

> [AZURE.SELECTOR]
- [Windowsi poe C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS-i](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Ülevaade

Selles teemas näidatakse, kuidas **malli** funktsioon Azure'i teatis jaoturi abil leviedastus server uudiste teatised, mis on lokaliseeritud keel ja seadmest. Selles õpetuses käivitate Windowsi poe rakenduse [Kasutamine teatis jaoturi saata uudiseid]loodud. Kui olete lõpetanud, on võimalik kasutajaks olete huvitatud kategooriad, määrata teateid ja vastu võtta ainult Tõuketeatiste valitud kategooria selles keeles keel.


On selle stsenaariumi koosneb kahest osast.

- Windowsi poe rakendus võimaldab kliendi seadmed, määrake sobiv keel ja tellimine eri server uudiste kategooriaid;

- funktsiooni tagaandmebaas edastab teated, **silt** ja **malli** feautres, Azure teatis jaoturi abil.



##<a name="prerequisites"></a>Eeltingimused

Olema juba lõpetanud õpetuse [Kasutamine teatis jaoturi saata uudiseid] ja on saadaval, koodi, sest selles õpetuses põhineb otse koodi.

Samuti vajate Visual Studio 2012 või uuem versioon.


##<a name="template-concepts"></a>Malli mõisted

[Kasuta teatis jaoturi saata uudiseid] ehitatud rakenduse kasutatud **Sildid** tellida erineva kategooria teatised.
Paljudes rakendustes siiski suunata mitmest turgude ja nõuavad lokaliseerimine. See tähendab, et teadete, ise sisu on lokaliseeritud ja seadmete õige komplektile toimetatud kohale.
Selles teemas näitame hõlpsalt esitamisel lokaliseeritud server uudiste teatised teatis jaoturi funktsioon **malli** kasutamise kohta.

Märkus: loomine mitme versiooni igal sildil on üks võimalus saata lokaliseeritud teatised. Näiteks inglise, prantsuse ja mandariini toetamiseks peame kolme eri sildid maailma uudiseid: "world_en", "world_fr" ja "world_ch". Me oleks siis saata lokaliseeritud versiooni maailma uudised iga järgmisi silte. Selles teemas kasutame Mallid vältimiseks leviku sildid ja nõue mitme sõnumite saatmiseks.

Mallid on kõrge, nii, et määrata, kuidas konkreetse seadme peaksid saama teatise. Mall määrab täpse last viidates atribuudid, mis pole teie rakenduse tagaandmebaas saadetud sõnumile. Meie puhul saadame lokaadi diagnostika sõnum, mis sisaldab kõiki toetatud keeltes:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Siis me tagada, et seadmed registreerida Mall, mis viitab õige atribuut. Näiteks järgmised malli koos vastavate sildid, register Windowsi poe rakendus, mis soovib lihtsa töölauateatisega teate:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Mallid on väga võimas funktsiooni kohta lisateavet meie [Mallid](notification-hubs-templates-cross-platform-push-messages.md) artikkel. 


##<a name="the-app-user-interface"></a>Rakenduse kasutajaliides

Me nüüd muuta teemas [Kasutamine teatis jaoturi saata uudiseid] saata lokaliseeritud uudiseid mallide kasutamine loodud katkestamine News kasutamine.

Windowsi poe rakenduse:

Oma MainPage.xaml kaasata lokaadi liitboksi muutmiseks tehke järgmist.

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Koostamise kliendi Windowsi poe rakendus

1. Teatiste oma tunni, lisada oma *StoreCategoriesAndSubscribe* ja *SubscribeToCateories* meetodite lokaadi parameeter.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Pange tähele, et *RegisterNativeAsync* meetodi asemel me kutsume *RegisterTemplateAsync*: meil on registreerimisel malli sõltub lokaadi teatud teatis vorming. Pakume malli ("localizedWNSTemplateExample") nimi, kuna võib soovime registreerida rohkem kui üks mall (näiteks üks töölauateatisega teatised) ja paanide ja läheb vaja nende nime, et värskendada või need kustutada.

    Pange tähele, kui seadme registreerib mitme malli sama silti, on sissetuleva sõnumi suunamise põhjustavad sildi mitme teatistes toimetada seade (üks iga malli). Selline käitumine on kasulik, kui sama loogilise sõnum on tulemuseks mitme visuaalsete teatiste, näiteks nähtaval märgi nii töölauateatis Windowsi poe rakenduses.

2. Lisage salvestatud lokaadi toomiseks järgmisel viisil:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. Rakenduses oma MainPage.xaml.cs värskendada oma nupp klõpsake sündmuseohjuri lokaadi liitboks jooksva väärtuse toomine ja kõne teated klassi, et nagu on näidatud:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Lõpuks App.xaml.cs faili, veenduge, et värskendada oma `InitNotificationsAsync` meetod tuua lokaadi ja kasutada siis, kui tellimise:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Teie tagaandmebaas lokaliseeritud teatiste saatmine

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Uudiseid saata teate jaoturi abil]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
