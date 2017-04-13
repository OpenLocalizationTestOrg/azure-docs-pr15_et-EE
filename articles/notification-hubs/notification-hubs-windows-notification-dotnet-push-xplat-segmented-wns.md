<properties
    pageTitle="Teatis jaoturi abil saate saata uudiseid (Windows universaalne)"
    description="Sildid registreerimise Azure teatis jaoturi abil saate saata uudiseid ühes kohas Windowsi rakenduse."
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

# <a name="use-notification-hubs-to-send-breaking-news"></a>Uudiseid saata teate jaoturi abil


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Ülevaade

Selles teemas näidatakse, kuidas Azure'i teatis jaoturi abil leviedastuse server uudiste teatiste abil või Windowsi poe rakenduse Windows Phone 8.1 (mitte Silverlight). Kui olete suunatud Windows Phone 8.1 Silverlighti, vaadake [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) versioon. Kui olete lõpetanud, kuvatakse server Uudiste kategooriad olete huvitatud registreerida ja vastu võtta ainult Tõuketeatiste kategooriate. See stsenaarium on palju rakenduste levinud mustri, kus on teatised rühmad kasutajate, mis on varem kinnitanud huvi nende, nt RSS-lugeja, muusika ventilaatoreid, rakendused ja nii edasi saata. 

Leviedastuse stsenaariumid on lubatud, sh ühe või mitme _siltide_ loomisel registreerimise teate jaoturi. Kui sildi, saadetakse teatised, kuvatakse kõigis seadmetes, mis on registreeritud sildi jaoks teatist. Kuna sildid on lihtsalt stringid, ei pea neid ette valmistada ette. Siltide kohta lisateabe saamiseks vaadake [teatis jaoturi marsruutimist ja sildi avaldised](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Eeltingimused

Selles teemas koostab loodud [teatis jaoturi alustamine]rakenduse[get-started]. Selles õpetuses enne olema juba lõpetanud [Alustamine teatis jaoturi][get-started].

##<a name="add-category-selection-to-the-app"></a>Kategooria valik lisamine rakendusse

Esimene samm on Kasutajaliidese elementidele lisada oma olemasoleva põhilehe, mis võimaldavad kasutajal valida kategooriate registreerida. Seadmes on talletatud kasutaja poolt valitud kategooriad. Rakenduse käivitamisel luuakse seadme registreerimine oma teatise keskuses koos valitud kategooria sildid.

1. Avage MainPage.xaml projekti fail ja seejärel kopeerige järgmine kood **ruudustiku** elementi:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Paremklõpsake **ühiskasutuses** projekti ja uue nimega **teatised**klassi **avaliku** muutja lisamine ainekursuse määratluse ja seejärel lisage **abil** järgmistest kood uue faili.

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopeerige järgmine kood uued **teatised** .

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    See tund kasutab kohalik salvestusruum uudiseid, mis on selle seadme kategooriates talletamiseks. Pange tähele, et *RegisterNativeAsync* meetodi asemel me kutsume *RegisterTemplateAsync* kategooriate malli registreerimise abil registreerida. 
    
    Pakume malli ("simpleWNSTemplateExample") nimi, kuna võib soovime registreerida rohkem kui üks mall (näiteks üks töölauateatisega teatised) ja paanide ja läheb vaja nende nime, et värskendada või need kustutada.

    Pange tähele, kui seadme registreerib mitme malli sama silti, on sissetuleva sõnumi suunamise põhjustavad sildi mitme teatistes toimetada seade (üks iga malli). Selline käitumine on kasulik, kui sama loogilise sõnum on tulemuseks mitme visuaalsete teatiste, näiteks nähtaval märgi nii töölauateatis Windowsi poe rakenduses.

    Mallide kohta leiate lisateavet teemast [Mallid](notification-hubs-templates-cross-platform-push-messages.md).




4. Failis App.xaml.cs projekti lisada **rakenduse** klassi atribuudi järgmist:

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Selle atribuudi abil luua ja kasutada **teatised** eksemplari.

    Ülaltoodud kood asendada selle `<hub name>` ja `<connection string with listen access>` oma teatise jaoturi nimi ja ühendusstringi *DefaultListenSharedAccessSignature* varem saadava jaoks kohatäited.

    > [AZURE.NOTE] Kuna identimisteavet, mida levitatakse kliendi rakendus pole üldjuhul turvaline, peaks oma kliendi rakendusega jaotada kuulata juurdepääsu võti. Rakenduse teatised, kuid olemasolevate registreerimise registreerida ei saa muuta access võimaldab kuulata ja teatised ei saa saata. Täielik juurdepääs kasutatakse turvalise kirjutamata teenuses saatmise teatised ja muuta olemasolevaid registreerimise.

5. Lisage oma MainPage.xaml.cs järgmine rida:

        using Windows.UI.Popups;

6. Lisage MainPage.xaml.cs projekti faili järgmisel viisil:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    See meetod loob kategooriate loendit ja kasutab **teatised** klassi kohalik salvestusruum loendi salvestada ja teie teate jaoturi vastavate siltide registreerida. Kategooriate muutmisel uuesti registreerimise luua uusi kategooriaid.

Teie rakendus on nüüd võimalik salvestada kategooria kohalik salvestamine seade ja registreerida jaoturi teatis iga kord, kui kasutaja muudab valikut kategooriad.

##<a name="register-for-notifications"></a>Teatiste register

Teate jaoturi käivitamisel on talletatud kohalik salvestusruumi kategooriate kasutamist registreerida järgmist.

> [AZURE.NOTE] Kuna kanali URI määratud Windowsi teatise teenuse (WNS) saate igal ajal muuta, peaksite Registreeruge teatised tihti, et vältida tõrkeid teatis. Selles näites registreerib teavitamise iga kord, kui rakendus käivitub. Sageli töötavad rakendused, rohkem kui üks kord päevas, saate ilmselt vahele jätta registreerimise säilitada läbilaskevõime kui vähem kui päev on möödunud eelmise registreerimise.

1. App.xaml.cs faili avada ja värskendada **InitNotificationsAsync** meetodit kasutada funktsiooni `notifications` klassi tellimise põhinevad kategooriad.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    See muudab veenduge, et iga kord, kui rakendus käivitub see toob kategooriad kohalikku ja taotleb registeration, nende kategooriate. **InitNotificationsAsync** meetod on loodud osana [Alustamine teatis jaoturi] [ get-started] õpetuse.

3. Failis MainPage.xaml.cs projekti lisamine *OnNavigatedTo* meetod järgmine kood:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    See värskendab põhilehe varem salvestatud kategooriad oleku põhjal.

Rakendus on nüüd valmis ja saate talletada kategooria seadme registreerimiseks jaoturi teatis iga kord, kui kasutaja muudab valikut kategooriad kasutatav kohalik salvestusruum. Järgmiseks me määratleb taustväärtus, mis saavad saata selle rakenduse kategooria teatised.

##<a name="sending-tagged-notifications"></a>Sildistatud teatiste saatmine

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Käivitage rakendus ja luua teatised

1. Visual Studio, vajutage klahvi F5 koostada ja käivitage rakendus.

    ![][1]

    Teate, et rakenduse kasutajaliides annab lülitab kogum, mille abil saate valida tellida kategooriad.

2. Ühe või mitme kategooriad lülitab lubamine ja seejärel klõpsake nuppu **Telli**.

    Rakenduse valitud kategooria teisendab sildid ja taotleb uue seadme registreerimine jaoks siltidega teatise keskuse kaudu. Registreeritud kategooriad on tagastatud ja kuvatakse dialoogiboksi.

    ![][19]

4. Saata uus teatis soovitud taustväärtus ühel järgmistest viisidest:

    + **Konsooli rakendus:** konsooli rakenduse käivitamine.

    + **Java/PHP:** käivitage rakendus/skripti.

    Teatiste valitud kategooria kuvatakse töölauateatisega teatised.

    ![][14]

##<a name="next-steps"></a>Järgmised sammud

Selles õppetükis me õppinud, kuidas edastada uudiseid kategooria järgi. Võtke arvesse, täites ühte järgmised õppetükid, mis täpsemalt teatis jaoturi stsenaariumide esile tõsta.

+ [Teatis jaoturi abil lokaliseeritud uudiseid leviedastus]

    Saate teada, kuidas laiendada Server Uudised rakenduse lubamiseks saatmise lokaliseeritud teatised.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Teatis jaoturi abil lokaliseeritud uudiseid leviedastus]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
