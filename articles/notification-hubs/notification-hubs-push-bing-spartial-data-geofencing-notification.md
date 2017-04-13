<properties
    pageTitle="Azure'i teatis jaoturi ja Bingi ruumiliste andmete Geo eraldatud Tõuketeatiste | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas Azure'i teatis jaoturi ja Bingi ruumiliste andmete asukoht põhineb Tõuketeatiste esitamisel."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="tõuketeatised teatis, vajutage teatis"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Azure'i teatis jaoturi ja Bingi ruumiliste andmete Geo eraldatud tõuketeatised
 
 > [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

Selles õppetükis saate teada, kuidas esitamisel asukoht põhineb Tõuketeatiste Azure'i teatis jaoturi ja Bingi ruumilise andmetega, kasutada rakenduses ühes kohas Windowsi platvormi kaudu.

##<a name="prerequisites"></a>Eeltingimused
Kõigepealt peate veenduge, et olete kõik soovitud tarkvara ja teenuste eeltingimused:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) või uuem versioon ([Kogukonnafoorumi väljaande](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) teha ka). 
* [Azure'i SDK](https://azure.microsoft.com/downloads/)uusim versioon. 
* [Bingi kaartide Arenduskeskus konto](https://www.bingmapsportal.com/) (saate luua tasuta ja seostada oma Microsofti konto). 

##<a name="getting-started"></a>Alustamine

Alustame projekti loomine. Visual Studios, käivitage uue projekti tüüp **Tühja rakenduse (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Kui projekti loomise on lõpule jõudnud, peaks teil olema rakmed rakenduse ise. Nüüd kõik geo hoides infrastruktuuri seadmine. Kuna me selle Bingi teenuseid kasutada, on avalik REST API näitaja, mis võimaldab teil päringu teatud asukohta paneelid:

    http://spatial.virtualearth.net/REST/v1/data/
    
Peate selle tööle saamiseks järgmisi määramine:

* **Andmeallika ID** ja **Andmeallika nimi** – Bingi kaardid API-ga andmeallikate sisaldada erinevaid bucketed metaandmed, näiteks asukohtade ja business tööaeg. Lisateavet saate lugeda nende siin. 
* **Üksuse nimi** – üksus, mida soovite kasutada, kui mõni teatamise. 
* **Bingi kaartide API võti** – see on oluline, mida te saada varem Bingi Arenduskeskus konto loomisel.
 
Iga ülaltoodud elementide teeme suure sukelduda häälestamise kohta.

##<a name="setting-up-the-data-source"></a>Andmeallika seadistamine

Mida teha Arenduskeskus Bingi kaardid. Klõpsa **andmeallikate** ülemisel navigeerimisribal ja valige **Andmeallikate haldamine**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Kui te ei ole Bingi kaartide API-ga enne töötanud, tõenäoliselt seal ei saa praegu andmeallikatega lihtsalt luua uus andmeallikas üles andmed klõpsates. Veenduge, et te täitke kõik nõutavad väljad.

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Võib tekkida – mis on andmefaili ja mida peaks teil olema üleslaadimise? Selleks et katse, saame lihtsalt kasutada toru-põhise valimi, et raamid San Francisco vee ala:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Ülaltoodud tähistab see üksus:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Lihtsalt kopeerida ja kleepida Ülaltoodud stringi uue faili salvestamine **NotificationHubsGeofence.pipe**ja laadige see Bingi Arenduskeskus.

>[AZURE.NOTE]Teilt võidakse määrata uue tootenumbri **juhtslaidi klahvi** , mis erineb **Päringu klahvi**. Lihtsalt loomine armatuurlaua kaudu uue tootenumbri ja värskendage andmeallika üles lehel.

Kui laadite faili, peate veenduge, et andmeallika avaldamist. 

Avage **Andmeallikate haldamine**, just nagu me ei kohale, andmeallika loendist otsimine ja klõpsake **Avalda** veerus **toimingud** . Sisse, peaksite nägema oma andmeallika **Avaldatud andmeallikate** menüüs:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Kui klõpsate nuppu **Redigeeri**, siis saab kiiresti vaadata, millised asukohad toodud selle:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Selles etapis portaali ei kuvata teile geofence, et lõime – kõik, mida läheb vaja on määratud asukohas on õige läheduses piirid.

Nüüd on teil andmeallika nõuded. Vaadake lisateavet taotluse URL-i API kõne, Arenduskeskus Bingi kaardid, klõpsake **andmeallikate** ja valige **Andmeallika teave**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**Päringu URL** on, mida me pärast siin. See on lõpp-punkti, mille vastu saaksime täita päringuid, kontrollige, kas seade on praegu piires asukoht või mitte. Selle kontrolli sooritamiseks läheb vaja lihtsalt käivitada toomine kõne vastu päringu URL-i lisatud järgmiste parameetrite abil:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Olete täpsustades, saame seadmest ja Bingi kaartide target punkti automaatselt on arvutused näha, kas see on selle geofence nii. Kui täidate taotluse brauserit (või cURL), saate standard JSON vastus:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Selle vastus juhtub ainult siis, kui on tegelikult määratud piiridesse. Kui see pole, saate mõne tühja **tulemite** ämber:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Kuidas häälestada rakendus UWP

Nüüd kui olete valmis andmeallika, alustame kallal UWP rakendus, mida me bootstrapped varasemas versioonis.

Kõigepealt tuleb meil lubada asukoht teenuste meie rakenduse. Selleks topeltklõpsake `Package.appxmanifest` file Exploreris **Lahendus**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Paketi atribuudid vahekaarti, mis avanud, klõpsake **võimaluste** ja veenduge, et valite **asukoht**.

![](./media/notification-hubs-geofence/vs-package-location.png)

Asukoha võimalus on deklareeritud, looge uus kaust teie lahendus nimega `Core`, ja lisage uus fail kogumahtu, nimetatakse `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Funktsiooni `LocationHelper` klassi on üsna lihtne sel hetkel – kõik see on, et saaksime hankida kasutaja asukoha süsteemis API:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Lugege lisateavet selle kohta, kuidas kasutaja asukoha ametlik [MSDN-i dokumendi](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx)UWP rakendustes.

Asukoha omandamise tegelikult töötab vaatamiseks avage oma põhilehe servas koodi (`MainPage.xaml.cs`). Looge uus sündmuseohjuri jaoks soovitud `Loaded` sündmuse on `MainPage` ehitaja:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Sündmuseohjuri rakendamine on järgmine:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Pange tähele, et saaksime tühjad selle sündmuseohjuri asünkroonse Kuna `GetCurrentLocation` on awaitable ja seetõttu nõuab täidetakse asünkroonse kontekstis. Lisaks, kuna teatud tingimustel me sattuda nullväärtusega asukoht (nt asukoht teenused on keelatud või rakendus on keelatud juurdepääsu asukohta õigused), peame veenduma, et see õigesti nullväärtusega registreerida.

Käivitage rakendus. Veenduge, et olete andnud juurdepääsu asukoht.

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Kui rakendus käivitab, teil peaks olema näeksid koordinaate **väljundi** aknas:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Nüüd teate, et asukoht omandamise töötab – julgelt eemaldada Loaded jaoks sündmuseohjuri test, kuna me ei kasuta seda enam.

Järgmise sammuna jäädvustada muudatuste kohta. Minge tagasi selle `LocationHelper` klassi ja lisage jaoks sündmuseohjuri `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Rakendamisel kuvatakse asukoha koordinaadid **väljundi** aknas:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Funktsiooni taustväärtus seadistamine

Laadige [.NET Taustväärtus valimi GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Kui allalaadimine on lõpule jõudnud, avage soovitud `NotifyUsers` kausta ja seejärel – selle `NotifyUsers.sln` fail.

Määrake soovitud `AppBackend` **Käivitus projekti** project ja käivitage see.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekti on juba konfigureeritud tõuketeatisi saatmiseks target seadmed, nii, et peate tegema mõlemat ainult – vahetada välja õige ühendus tekstistringi jaoks teate jaoturi ja lisada piiri ID saata teate ainult siis, kui kasutaja on selle geofence.

Konfigureerimiseks ühendusstring, on `Models` kausta avamine `Notifications.cs`. Funktsiooni `NotificationHubClient.CreateClientFromConnectionString` funktsioon peaks sisaldama teavet oma teate jaoturi, mida te võite [Azure portaali](https://portal.azure.com) (ilme **sätteid** **Juurdepääsupoliitikaid** tera sees). Salvestage värskendatud konfiguratsioonifail.

Nüüd tuleb luua Bingi kaardid API tulem mudel. Lihtsaim viis selleks on, paremklõpsake soovitud `Models` kausta **lisamine** > **klassi**. Nime `GeofenceBoundary.cs`. Kui lõpetanud, kopeerige soovitud JSON API vastust, mida me arutada esimene jaotis ja Visual Studio kasutamine **redigeerimine** > **Teisiti kleepimine** > **Kleebi JSON nimega tunnid**. 

Sel viisil tagada, et objekti sarjadesse täpselt nii, nagu see oli mõeldud. Oma tulemuseks klassi Sea näeb selline:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Järgmiseks Avage `Controllers`  >  `NotificationsController.cs`. Läheb vaja, saate konto target pikkus- ja laiuskraadide postitus kõne. Selleks lihtsalt lisada kaks tekstistringi funktsioon allkiri – `latitude` ja `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Luua uue nimega Projecti klassi `ApiHelper.cs` – kasutame Bingi kontrollida ühenduse osutage piiri ristmikel. Rakendada mõne `IsPointWithinBounds` funktsioon umbes järgmine:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Veenduge, et te varem saadud (sama kehtib API võti) Bingi Arenduskeskus päringu URL-i lõpp-punkti API asendada. 

Kui päringu tulemused, mis tähendab, et määratud punkti piires geofence, et me tagasi `true`. Kui tulemusi ei esine Bingi on meile, on punkti väljaspool otsing paneeli, nii, et saaksime tagasi `false`.

Olles tagasi kohas `NotificationsController.cs`, luua märge enne lülitit lause:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Nii teate on ainult saata, kui punkt piires.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Tõuketeatiste UWP rakenduse testimine

Naasmine rakenduse UWP meil peaks nüüd võimalik testida teatised. Sees on `LocationHelper` klassi, looge uus funktsioon – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Vaheta soovitud `POST_URL` lõime eelmises jaotises oma juurutatud veebirakenduse asukohta. Nüüd on kohalik käivitamiseks OK, kuid töötades juurutamine avaliku versiooni, peate selle välise pakkujaga majutada.

Vaatame nüüd veenduge, et registreerib UWP minirakendus Tõuketeatiste. Visual Studio, klõpsake nuppu **projekti** > **talletada** > **poe rakendus seostada**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Pärast sisselogimist kontole arendaja, veenduge, et valige olemasolev rakendus või looge uus ja paketi seostada. 

Avage soovitud Arenduskeskus ja avage äsja loodud rakendus. Klõpsake valikut **teenused** > **Tõuketeatised** > **Live'i sait**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Saidil, vaadake üle **Salajane rakenduse** ja **Paketi SID**. Te peate nii Azure'i portaalis – avage oma teate jaoturi ja klõpsake siis nuppu **sätted** > **Teatis teenuste** > **Windows (WNS)** ja sisestage väljadele nõutav teave.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klõpsake **Salvesta**.

Paremklõpsake **Solution Exploreris** **Viited** ja valige **Halda Nugeti paketid**. Peame lisada **Microsoft Azure'i teenus siini hallatavate teek** – lihtsalt otsida `WindowsAzure.Messaging.Managed` ja lisamine projekti.

![](./media/notification-hubs-geofence/vs-nuget.png)

Testimiseks saame luua selle `MainPage_Loaded` sündmuseohjuri uuesti ja selle koodilõigu lisamine:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Ülaltoodud registreerib teate jaoturi rakendus. Olete valmis! 

Klõpsake `LocationHelper`sisse selle `Geolocator_PositionChanged` sündmuseohjuri, saate lisada tükk vöötkood paneb jõuga sees on geofence asukoht:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Kuna me õigest (mis võib olla piirides hetkel) real koordinaate ja kasutate eelmääratletud testi väärtused, näeme värskendamisel kuvatakse teade:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Mis saab edasi?

On paar juhised selle kohta, peate Lisaks veenduge, et lahendus on tootmise valmis jälgimiseks.

Kõigepealt peate geofences dünaamiline tagamiseks. See nõuab teatud lisatööd Bingi API-ga, üles laadida uut piirides olemasolevas andmeallikas. Teema kohta lisateabe saamiseks lugege [Bingi ruumilise andmete Services API dokumentatsiooni](https://msdn.microsoft.com/library/ff701734.aspx) .

Teiseks, kui töötate tagamaks, et kohaletoimetamisega on lõpetatud õige osalejatele, võiksite suunata neid [sildistamine](notification-hubs-tags-segment-push-message.md)kaudu.

Lahendus ülaltoodud kirjeldatakse olukorda, kus peate mitmesuguseid sihtplatvormid, et me ei piiranud geofencing süsteemi kohased võimaluste abil. See tähendab, et Universal Windowsi platvormi pakub võimaluste [geofences õige kontorist-of-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)tuvastamiseks.

Teatis jaoturi võimaluste kohta lisateabe saamiseks vaadake meie [dokumentatsiooni portaalis](https://azure.microsoft.com/documentation/services/notification-hubs/).
