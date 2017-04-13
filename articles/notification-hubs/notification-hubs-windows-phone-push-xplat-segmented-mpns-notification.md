<properties
    pageTitle="Teatis jaoturi abil saate saata uudiseid (Windows Phone)"
    description="Kasutage Azure'i teatis jaoturi sildi registreerimise uudiseid saatmiseks Windows Phone rakendus."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Uudiseid saata teate jaoturi abil

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Ülevaade

Selles teemas näidatakse, kuidas leviedastuse server uudiste teatiste abil Windows Phone 8.0/8.1 Silverlighti rakenduse Azure'i teatis jaoturi abil. Kui olete suunatud Windowsi poest või Windows Phone 8.1 rakendus, vaadake [Ühes kohas Windowsi](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) versiooni. Kui olete lõpetanud, kuvatakse server Uudiste kategooriad olete huvitatud registreerida ja vastu võtta ainult Tõuketeatiste kategooriate. Selle stsenaariumi on levinud mustri palju rakendused, kus on teatised saadetakse rühmad kasutajate, mis on varem huvi nende, nt RSS-lugeja, rakendused muusika ventilaatoreid jne.

Leviedastuse stsenaariumid on lubatud, sh ühe või mitme _siltide_ loomisel registreerimise teate jaoturi. Kui sildi, saadetakse teatised, kuvatakse kõigis seadmetes, mis on registreeritud sildi jaoks teatist. Kuna sildid on lihtsalt stringid, ei pea neid ette valmistada ette. Siltide kohta lisateabe saamiseks vaadake [teatis jaoturi marsruutimist ja sildi avaldised](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Eeltingimused

Selles teemas koostab loodud [teatis jaoturi alustamine]rakenduse. Selles õpetuses enne olema juba lõpetanud [teatis jaoturi alustamine].

##<a name="add-category-selection-to-the-app"></a>Kategooria valik lisamine rakendusse

Esimene samm on Kasutajaliidese elementidele lisada oma olemasoleva põhilehe, mis võimaldavad kasutajal valida kategooriate registreerida. Seadmes on talletatud kasutaja poolt valitud kategooriad. Rakenduse käivitamisel luuakse seadme registreerimine oma teatise keskuses koos valitud kategooria sildid.

1. Avage MainPage.xaml projekti fail ja seejärel asendada nimega **ruudustiku** elementide `TitlePanel` ja `ContentPanel` järgmine kood:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. Projekti luua uue nimega **teatised**klassi lisada **avaliku** muutja klassi määratluse ning seejärel lisada uue koodi faili **abil** järgmistest:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Kopeerige järgmine kood uued **teatised** .

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    See tund kasutab isoleeritakse salvestusruumi kategooriate uudiseid, et seade on saada talletamiseks. See sisaldab ka viise nende kategooriate [malli](notification-hubs-templates-cross-platform-push-messages.md) teate registreerimine abil registreerida.


4. Failis App.xaml.cs projekti lisada järgmised atribuudi klassimärkmiku **rakendus** . Asendada selle `<hub name>` ja `<connection string with listen access>` oma teatise jaoturi nimi ja ühendusstringi *DefaultListenSharedAccessSignature* varem saadava jaoks kohatäited.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Kuna identimisteavet, mida levitatakse kliendi rakendus pole üldjuhul turvaline, peaks oma kliendi rakendusega jaotada kuulata juurdepääsu võti. Rakenduse teatised, kuid olemasolevate registreerimise registreerida ei saa muuta access võimaldab kuulata ja teatised ei saa saata. Täielik juurdepääs kasutatakse turvalise kirjutamata teenuses saatmise teatised ja muuta olemasolevaid registreerimise.

5. Lisage oma MainPage.xaml.cs järgmine rida:

        using Windows.UI.Popups;

6. Lisage MainPage.xaml.cs projekti faili järgmisel viisil:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    See meetod loob kategooriate loendit ja kasutab **teatised** klassi kohalik salvestusruum loendi salvestada ja teie teate jaoturi vastavate siltide registreerida. Kategooriate muutmisel uuesti registreerimise luua uusi kategooriaid.

Teie rakendus on nüüd võimalik salvestada kategooria kohalik salvestamine seade ja registreerida jaoturi teatis iga kord, kui kasutaja muudab valikut kategooriad.

##<a name="register-for-notifications"></a>Teatiste register

Teate jaoturi käivitamisel on talletatud kohalik salvestusruumi kategooriate kasutamist registreerida järgmist.

> [AZURE.NOTE] Kuna kanali URI määratud, Microsoft Push teatise teenuse (MPNS) saate igal ajal muuta, peaksite Registreeruge teatised tihti, et vältida tõrkeid teatis. Selles näites registrite teatiste iga kord, kui rakendus käivitub. Sageli töötavad rakendused, rohkem kui üks kord päevas, saate ilmselt vahele jätta registreerimise säilitada läbilaskevõime kui vähem kui päev on möödunud eelmise registreerimise.


1. App.xaml.cs faili avada ja lisada **asünkroonse** muutja **Application_Launching** meetod ja asendada teatis jaoturi registreerimine kood, mida te lisatud [Alustamine jaoturi teatis] järgmine kood:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    See tagab, et iga kord, kui rakendus käivitub see toob kohalikku kategooriaid ja nende kategooriate registreerimise taotleb.

2. MainPage.xaml.cs projekti faili, lisage järgmine kood, mis rakendab **OnNavigatedTo** meetod.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Pärast kinnitust, et teie kategooriad olid tellimus on lõpule viidud, käivitage konsooli rakendus, et saada teatisi iga kategooria. Kontrollige, kas saate ainult teate kategooria, mida olete tellinud.

    ![][3]

Selles teemas on lõpule viidud.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Teatis jaoturi kasutamise alustamine]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

