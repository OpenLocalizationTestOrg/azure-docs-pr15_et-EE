<properties
    pageTitle="Mobiilirakenduste kirjutamata .net-i serveri SDK töötamise | Azure'i rakendust Service"
    description="Saate teada, kuidas töötada kirjutamata .net-i serveri SDK Azure'i rakenduse teenuse mobiilirakenduste."
    keywords="rakenduse, Azure'i rakendust service, mobiilirakenduse, mobiilsideseadmete teenus, skaala scalable, rakenduse juurutuse azure rakenduse juurutamine"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Azure'i mobiilirakenduste kirjutamata .net-i serveri SDK töötamine

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Selles teemas näidatakse, kuidas kasutada .NET taustväärtus server SDK stsenaariumid Azure'i rakenduse teenuse mobiilirakenduste kohta. Azure'i Mobile'i rakendused SDK abil saate töötada mobiilikliendid ASP.net-i rakenduse kaudu.

>[AZURE.TIP] [.Net-i server Azure'i mobiilirakenduste SDK] [ 2] on avatud allikas github. Hoidla sisaldab kõik lähtekoodi, sh kogu server SDK ühiku test komplekti ja mõned valimi projektid.

## <a name="reference-documentation"></a>Dokumentides

Viide dokumentatsioonist serveri SDK asub siin: [Azure'i Mobile'i rakendused .net-i viide][1].

## <a name="create-app"></a>Kuidas: .net-i mobiilirakenduse kirjutamata loomine

Kui teil on hakanud uue projekti, saate luua rakenduse teenuserakenduse [Azure portaali] või Visual Studio abil. Saate käivitada rakenduse teenuserakenduse kohalikult või projekti avaldada oma pilvepõhisesse rakenduse teenuse mobiilirakenduse kaudu.  

Olemasoleva projekti mobiilsideseadmete võimaluste lisamisel leiate jaotisest [alla laadida ja käivitada SDK](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>.NET taustväärtus, Azure portaali loomine

Mõnda rakendust Service mobiilsideseadmete kirjutamata loomiseks tehke ühte [Kiirjuhend õpetuse] [ 3] või toimige järgmiselt:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Tagasi _Alustamine_ tera, klõpsake jaotises **looge uus tabel API**, valige väärtus **C#** **Taustväärtus keel**. Klõpsake nuppu **Laadi alla**, ekstrakti tihendatud projekti faile kohalikku arvutisse ja avage lahenduse Visual Studios.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>.NET taustväärtus, kasutades Visual Studio 2013 ja Visual Studio 2015 loomine

Installige [Azure'i SDK .net-i] [ 4] (versioon 2.9.0 või uuem versioon) on Azure mobiilirakenduste projekti loomiseks Visual Studio. Kui olete installinud SDK, luua ASP.net-i rakenduse, tehes järgmist:

1. Avage dialoogiboks **Uue projekti** ( *failist* > **Uus** > **projekti...**).
2. Laiendage **Mallid** > **Visual C#**ja valige **Web**.
3. **ASP.net-i veebirakenduse**valimine
4. Täitke projekti nime. Klõpsake nuppu **OK**.
5. Klõpsake jaotises _ASP.net-i 4.5.2 Mallid_, valige **Azure Mobile'i rakendus**. Märkige ruut **Host pilves** mobiilsideseadmete kirjutamata loomiseks pilves, kuhu saate selle projekti avaldada.
6. Klõpsake nuppu **OK**.

## <a name="install-sdk"></a>Kuidas: alla laadida ja käivitada SDK

SDK on saadaval [NuGet.org]. See pakett sisaldab SDK kasutamise alustamiseks base funktsioonidele. SDK lähtestada, peate **HttpConfiguration** objekti toiminguid.

### <a name="install-the-sdk"></a>SDK installimine

SDK installimiseks paremklõpsake serveri projekti Visual Studios, valige **Halda Nugeti paketid**, [Microsoft.Azure.Mobile.Server] paketi otsida, siis klõpsake nuppu **Installi**.

###<a name="server-project-setup"></a>Project serveri käivitamine

.NET taustväärtus serveri projekti on lähtestatud sarnaselt muude ASP.net-i projektide, sh hõlmav OWIN käivitus tund. Veenduge, et teil on viidatud Nugeti pakett `Microsoft.Owin.Host.SystemWeb`. Visual Studio see tund lisamiseks paremklõpsake serveri projekti ja valige **Lisa** > 
**Uus üksus**ja seejärel **Web** > **Üldine** > **OWIN käivitus klassi**.  Klassi on loodud järgmine atribuut:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

Klõpsake soovitud `Configuration()` OWIN käivitus klassi meetodiga **HttpConfiguration** objekti abil konfigureerida Azure mobiilirakenduste keskkonnas.
Järgmises näites lähtestab serveri projekt pole lisatud funktsioonide abil:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Üksikute funktsioonide lubamiseks peate kõne pikendamise võimalused **MobileAppConfiguration** objekti enne **loetletust**. Näiteks järgmine kood lisab vaikimisi marsruudib kõik API kontrollerid, mis on atribuut `[MobileAppController]` lähtestamisel:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Serveri Kiirjuhend: Azure'i portaalis kõned **UseDefaultConfiguration()**. See võrdub järgnevalt:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Laiend meetodid on:

* `AddMobileAppHomeController()`leiate Azure'i mobiilirakenduste vaikimisi avalehe.
* `MapApiControllers()`kohandatud API võimalusi pakub jaoks kujundatud WebAPI kontrollerid on `[MobileAppController]` atribuut.
* `AddTables()`pakub kaardistada soovitud `/tables` tabeli vastutavatele lõpp-punktid.
* `AddTablesWithEntityFramework()`lühike vaba vastenduse on selle `/tables` lõpp-punktid abil üksuse raames vastavalt kontrollerid.
* `AddPushNotifications()`annab lihtsa meetodi registreerimise teatis jaoturi seadmed.
* `MapLegacyCrossDomainController()`pakub arengu standard CORS päised.

### <a name="sdk-extensions"></a>SDK laiendid

Järgmised Nugeti põhinev laiendamise paketid pakkuda erinevate mobiilsideseadmete funktsioonid, mis võivad kasutada rakenduse. Lähtestamisel laiendid lubada **MobileAppConfiguration** objekti abil.

- [Microsoft.Azure.Mobile.Server.Quickstart] toetab põhilised mobiilirakenduste häälestamine. Lisatud konfiguratsiooni, helistades lähtestamisel **UseDefaultConfiguration** laiendamine meetod. Sellele laiendile sisaldab jälgimise laiendid: teatised, autentimine, üksus, tabelid, domeenide ja Avaleht paketid. See pakett kasutab Mobile'i rakendused Kiirjuhend Azure'i portaalis saadaval.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   rakendab vaikimisi *selle mobiilirakendus on tööks lehe* root veebisaidi jaoks. Lisada konfiguratsiooni, helistades   **AddMobileAppHomeController** laiendamine meetod.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   klassid andmetega töötamiseks ja loetleb üles andmete kohaletoimetamisel. Lisada konfiguratsiooni, helistades **AddTables** laiendamine meetod.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   võimaldab üksuse raames juurdepääsu andmetele SQL-andmebaasis. Lisada konfiguratsiooni, helistades **AddTablesWithEntityFramework** laiendamine meetod.

- [Microsoft.Azure.Mobile.Server.Authentication] võimaldab autentimist ja loetleb üles OWIN vahevara, kasutatakse sõned kinnitamiseks. Lisada konfiguratsiooni, helistades **AddAppServiceAuthentication**  
   ja **IAppBuilder**. **UseAppServiceAuthentication** pikendamise võimalused.

- [Microsoft.Azure.Mobile.Server.Notifications] lubab tõuketeatised ja määratleb tõuketeatised registreerimise lõpp-punkti. Lisada konfiguratsiooni, helistades **AddPushNotifications** laiendamine meetod.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   loob kontrolleril, mis aitab andmete pärand veebibrauserite kaudu oma Mobile'i rakendus. Lisada konfiguratsiooni, helistades   **MapLegacyCrossDomainController** laiendamine meetod.

- [Microsoft.Azure.Mobile.Server.Login] pakub AppServiceLoginHandler.CreateToken() meetod, mis on kohandatud autentimine stsenaariumid ajal staatiline meetod.   

## <a name="publish-server-project"></a>Kuidas: project serveri avaldamine

Selles jaotises kirjeldatakse, kuidas Visual Studio .NET taustväärtus projekti avaldamiseks. Saate juurutada kirjutamata projekti Git või kaetud [Azure'i rakendust Service juurutamise dokumentatsiooni](../app-service-web/web-sites-deploy.md)teiste meetodite abil.

1. Visual Studio taastada projekti taastamine NuGet-paketid.

2. Lahenduste Explorer, paremklõpsake projekt, klõpsake nuppu **Avalda**. Esimest korda avaldate, peate määratlema avaldamise profiili. Kui teil juba on määratletud profiili, saate selle valimiseks ja klõpsake nuppu **Avalda**.

2. Kui teilt küsitakse, valige avalda target, klõpsake käsku **Microsoft Azure'i rakendust Service** > **järgmise**, (vajadusel) ja seejärel logige sisse oma Azure kasutajanimi ja parool. 
   Visual Studio allalaaditavate failide ja turvaliselt salvestab teie avaldada otse Azure'i sätted.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Valige oma **tellimuse**, **Ressursside tüübi** valimine **vaates**, laiendage **Mobile'i rakendus**ja klõpsake oma mobiilirakenduse kirjutamata ja siis nuppu **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Kontrollige avalda profiili teave ja klõpsake nuppu **Avalda**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Kui teie kirjutamata Mobile'i rakendus on nüüd avaldatud, kuvatakse sihtleht, mis näitab edu.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Kuidas: tabeli kontrolleril määratlemine

Tabeli kontrolleril esitamist SQL tabeli mobiilikliendid määratleda.  Tabeli domeenikontrolleri konfigureerimine nõuab kolm toimingut:

1. Saate luua andmete objekti (DTO) klassi.
2. Tabeliviide Mobile DbContext klassi konfigureerimine.
3. Saate luua tabeli kontrolleril.

Soovitud andmete edastamiseks objekti (DTO) on tavaline C# objekti mis pärib `EntityData`.  Näiteks:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Funktsiooni DTO kasutatakse SQL-andmebaasi tabeli määratlus.  Andmebaasi kirje loomiseks lisage soovitud `DbSet<>` DbContext, kasutate atribuut.  Projekti vaikemalli Azure'i mobiilirakenduste, nimetatakse funktsiooni DbContext `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Kui teil on installitud Azure'i SDK, nüüd saate luua malli tabeli kontrolleril järgmiselt:

1. Paremklõpsake kontrollerid kausta ja valige käsk **Lisa** > **kontrolleril …**.
2. **Azure'i Mobile'i rakendused tabeli kontrolleril** suvand ja seejärel klõpsake nuppu **Lisa**.
3. **Selle domeenikontrolleri lisamine** dialoogiboksis:
    * **Mudeli klassi** rippmenüü, valige oma uue DTO.
    * Valige rippmenüüst **DbContext** Mobile teenuse DbContext klassi.
    * Selle domeenikontrolleri nimi on teie jaoks loodud.
4. Klõpsake nuppu **Lisa**.

Kiirjuhend serveri projekt sisaldab näiteks lihtsa **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Kuidas: tabeli suuruse lehtede nummerdamine

Vaikimisi tagastab Azure'i mobiilirakenduste kohta taotluse 50 kirjet.  Piipar tagab, et kliendi SEO üles oma UI lõime ega liiga pikk, server tagada hea kasutaja kogemus. Tabeli Piipar suuruse muutmiseks suurendada "lubatud päringu suurus" serveripoolne ja kliendipoolne lehe suuruse serveripoolne "lubatud päringu suurus" on kohandatud abil soovitud `EnableQuery` atribuut:

    [EnableQuery(PageSize = 500)]

Veenduge, et selle PageSize on sama või suurem kui kliendi poolt küsitud.  Vaadake kindla kliendi juhendid dokumentatsioonist kliendi lehe suuruse muutmise kohta.

## <a name="how-to-define-a-custom-api-controller"></a>Kuidas: kohandatud API kontrolleril määratlemine

Kohandatud API kontrolleril pakub põhifunktsioone abil oma mobiilirakenduse kirjutamata, asetades lõpp. Mobile kohased API kontrolleril [MobileAppController] atribuut abil saate registreerida. Funktsiooni `MobileAppController` atribuut registreerib marsruudi, häälestab Mobile'i rakendused JSON serializer ja lülitab [klientrakenduse versiooni kontrollimine](app-service-mobile-client-and-server-versioning.md).

1. Visual Studio, paremklõpsake kontrollerid kausta ja seejärel klõpsake nuppu **Lisa** > **kontrolleril**, valige **Web API 2 kontrolleril&mdash;tühja** ja klõpsake nuppu **Lisa**.

2. Sisestama **domeenikontrolleri nimi**, näiteks `CustomController`, ja klõpsake nuppu **Lisa**.

3. Uus kontrolleril klassi faili, lisage järgmine lause abil:

        using Microsoft.Azure.Mobile.Server.Config;

4. **[MobileAppController]** atribuut rakendamine API kontrolleril klassi määratluse, nagu järgmises näites:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. Lisage App_Start/Startup.MobileApp.cs faili, kõne **MapApiControllers** laiendamine meetod, nagu järgmises näites:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Saate kasutada ka funktsiooni `UseDefaultConfiguration()` laiend meetodi asemel `MapApiControllers()`. Mis tahes kontrolleril, mis ei ole **MobileAppControllerAttribute** rakendatud pääseb siiski kliendid, kuid see võib-olla ei olema õigesti kliendid, mis tahes mobiilirakenduse kliendi SDK.

## <a name="how-to-work-with-authentication"></a>Kuidas: autentimine töötamine

Azure'i mobiilirakenduste kasutab rakenduse autentimise / luba secure oma mobiilse taustväärtus.  Selles jaotises kuvatakse sooritamiseks järgmisi autentimine seotud toiminguid .NET taustväärtus serveri projektis.

+ [Kuidas: lisamine projekti serveri autentimine](#add-auth)
+ [Kuidas: rakenduse kasutamine kohandatud autentimine](#custom-auth)
+ [Kuidas: Too autenditud kasutaja teave](#user-info)
+ [Kuidas: piirata volitatud kasutajate juurdepääs andmetele](#authorize)

### <a name="add-auth"></a>Kuidas: lisamine projekti serveri autentimine

**MobileAppConfiguration** objekt ulatub ja konfigureerides OWIN vahevara saate lisada oma Project Serveri autentimine. Kui installite [Microsoft.Azure.Mobile.Server.Quickstart] paketi ja **UseDefaultConfiguration** laiend meetod, saate vahele jätta – juhis 3.

1. Visual Studio, installige [Microsoft.Azure.Mobile.Server.Authentication] pakett.

2. Startup.cs projekti faili, lisage järgmine rida koodi **konfiguratsiooni** meetod alguses:

        app.UseAppServiceAuthentication(config);

    See OWIN vahevara komponent kinnitatakse sõned välja rakenduse teenuse seotud lüüs.

3. Lisage soovitud `[Authorize]` atribuut kontrolleril või meetod, mis nõuab autentimist. 

Autentida klientidele oma mobiilirakenduste taustväärtus kohta leiate teemast [rakenduse lisamine autentimine](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Kuidas: rakenduse kasutamine kohandatud autentimine

Kui te ei soovi kasutada mõne rakenduse teenuse autentimise/luba, saate rakendada oma login süsteem. Installige [Microsoft.Azure.Mobile.Server.Login] pakett abistamiseks autentimise Turbeloa loomine.  Sisestage oma kood valideerimise kasutajatunnust. Näiteks võib kontrollida andmebaasi soolatud ja räsitud paroolid. Järgmises näites, on `isValidAssertion()` meetod (määratletud mujal) vastutab kontrolli.

Kohandatud autentimine on esitatud mõne ApiController loomise ja asetades `register` ja `login` toimingud. Kliendi peaksite kasutama kohandatud Kasutajaliideseelemendid kasutaja teabe kogumine.  Teave esitatakse siis API standard HTTP POST kõne. Kui server kinnitatakse kinnitus, märgiks on välja antud, kasutades funktsiooni `AppServiceLoginHandler.CreateToken()` meetod.  ApiController **ei tohi** kasutada funktsiooni `[MobileAppController]` atribuut. 

Näide `login` toiming:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Eelmises näites LoginResult ja LoginResultUser on nõutavad atribuudid asetades sarjadesse jaotatav objektid. Kliendi loodab login vastuste tagastatakse JSON objektina vormi:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Funktsiooni `AppServiceLoginHandler.CreateToken()` meetod sisaldab _sihtrühma_ ja on _väljaandja_ parameeter. Mõlema parameetri on seatud URL oma rakenduse juures HTTPS-skeemi kasutavad. Samuti on soovitatav määrata _secretKey_ olema rakenduse väärtus kasutaja sisselogimine võti. Levitage allkirjastamiseks võti kliendi, nagu seda saab kasutada mint võtmed ja jäljendada kasutajatel. Saate hankida allkirjastamiseks võti ajal rakenduse teenuses korraldaja viitamine on _veebisaidi\_AUTH\_ALLKIRJASTAMINE\_klahvi_ muutuja. Kohaliku silumine kontekstis vajadusel järgige [kohaliku silumine autentimise](#local-debug) jaotises tuua võti ja salvestada selle rakenduse sätteks on.

Väljastatud loa võib sisaldada ka muud nõuded ja aegumiskuupäeva.  Minimaalselt, väljastatud loa peab sisaldama nõude teema (**sub**).

Saate toetada standard klient `loginAsync()` meetod ülekoormuse marsruutimiseks autentimist.  Kui kliendi helistab `client.loginAsync('custom');` sisselogimiseks peab olema oma marsruutimiseks `/.auth/login/custom`.  Saate määrata kohandatud autentimine kontrolleril, kasutades protsessi `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Abil soovitud `loginAsync()` lähenemine tagab, et iga järgmise kõne teenusega seotud autentimise luba.

###<a name="user-info"></a>Kuidas: Too autenditud kasutaja teave

Kui kasutaja on autenditud rakenduse teenus, pääsete määratud kasutaja ID-d ja muud teavet .net-i koodi kirjutamata. Kasutajateabe saab autoriseerimine otsuseid soovitud taustväärtus. Järgmine kood hangib taotluse seostatud kasutaja ID:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SID on saadud teenusepakkuja kohased kasutaja ID-d ja staatiline antud kasutaja ja sisselogimise pakkuja.  SID on sobimatu autentimine märkide null.

Rakenduse teenuse saate ka taotleda kindlate taotluste login teenusepakkujalt. Iga identiteedipakkuja saate rohkem teavet identiteedipakkuja SDK abil.  Näiteks saate Facebooki Graph API sõprade teavet.  Saate määrata taotluste, mis on nõutav pakkuja tera Azure'i portaalis. Mõned nõuded on vaja konfigureerimise identiteedipakkuja juures.

Järgmine kood nõuab **GetAppServiceIdentityAsync** laiend meetod saada sisselogimisteave, mis sisaldavad juurdepääsu luba, mis on vaja teha taotlusi vastu graafik Facebook API:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Lisamine, kasutades avalduse `System.Security.Principal` **GetAppServiceIdentityAsync** laiend meetodit.

### <a name="authorize"></a>Kuidas: piirata volitatud kasutajate juurdepääs andmetele

Me ilmnes eelmises jaotises, kuidas tuua autenditud kasutaja kasutaja ID-d. Juurdepääsu piiramiseks andmed ja muud ressursid selle väärtuse alusel. Näiteks kasutajanimi veeru lisamine tabelite ja filtreerimise päringutulemite kasutaja ID abil on lihtne viis piirata tagastatud andmeid vaid autoriseeritud kasutajad. Järgmine kood tagastab andmeridade ainult siis, kui SID vastab TodoItem tabeli veerus kasutajanimi väärtus:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

Funktsiooni `Query()` meetod tagastab mõne `IQueryable` mis saab manipuleerida LINQ käsitlema filtreerimine.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Kuidas: lisada push teatised serveri projekti

Tõuketeatiste lisamine projekti server **MobileAppConfiguration** objekt ulatub ja teatis jaoturi kliendi loomine.

1. Visual Studios, paremklõpsake server project ja klõpsake nuppu **Halda Nugeti pakettide**, otsida `Microsoft.Azure.Mobile.Server.Notifications`, klõpsake nuppu **Installi**. 

2. Korrake seda toimingut installimiseks on `Microsoft.Azure.NotificationHubs` paketi, mis sisaldab teatise jaoturi kliendi teek.

3. Klõpsake App_Start/Startup.MobileApp.cs, ja lisage **AddPushNotifications()** laiendamine meetod kõne lähtestamisel:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Järgmine kood, mis loob teatise jaoturi kliendi lisamiseks tehke järgmist.

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Nüüd saate teatise jaoturi kliendi Tõuketeatiste saatmiseks registreeritud seadmed. Lisateabe saamiseks vt [Lisa Tõuketeatiste app](app-service-mobile-ios-get-started-push.md). Teatis jaoturi kohta leiate lisateavet teemast [Teatise jaoturi ülevaade](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Kuidas: luba suunatud tõuketeatised siltide kasutamine

Teatis jaoturi võimaldab teil saata suunatud teatiste abil kindla registreerimise siltide abil. Mitme sildi luuakse automaatselt.

* Installi ID tuvastab kindla seadme.
* Kasutaja Id, mis põhineb volitatud SID tuvastab mõne kindla kasutaja.

Installi ID pääseb juurde **MobileServiceClient**atribuudi **installationId** .  Järgmises näites kujutatakse installi ID-d sildi lisamine teatud installimist klõpsake teate jaoturi abil:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Tõuketeatised teatise registreerimise käigus kliendi esitatud sildid ignoreerib selle kirjutamata installimise loomisel. Kliendi installimine siltide lisamiseks lubamiseks peate looma kohandatud API, mis lisab sildid, kasutades eelmises mustri. 

Vt [Kliendi lisatud tõuketeatised teatis siltide] [ 5] näitena valimis rakenduse teenuse mobiilirakenduste lõplikus Kiirjuhend.

##<a name="push-user"></a>Kuidas: saada Tõuketeatiste autenditud kasutaja

Kui autenditud kasutaja registreerib Tõuketeatiste, lisatakse automaatselt kasutaja ID sildi registreerimise. Selle sildiga abil saate saata Tõuketeatiste registreeritud selle isiku kõigis seadmetes. Järgmine kood saab taotluse kasutaja SID ja saadab sellele isikule iga seadme registreerimine malli tõuketeatised teate:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Registreerumisel Tõuketeatiste autenditud klient, veenduge, et autentimine on lõpule viidud enne, kui proovite registreerimise. Lisateabe saamiseks lugege teemat [kasutajatele Push] [ 6] jaoks .NET taustväärtus valimis rakenduse teenuse mobiilirakenduste lõplikus Kiirjuhend.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Kuidas: silumine ja .net-i Server SDK tõrkeotsing

Azure'i rakendust Service pakub paljusid silumine ja tõrkeotsingu tehnikate ASP.net-i rakendusi.

- [Azure'i rakenduse teenuse jälgimine](../app-service-web/web-sites-monitor.md)
- [Azure'i rakenduse teenuses diagnostikalogimise lubamiseks](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Rakenduse Azure teenuse Visual Studio tõrkeotsing](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Logimine

Saate kirjutada rakenduse teenuse diagnostikalogid, kasutades standard ASP.net-i Jälita kirjutamist. Enne kui saate kirjutada logid, peate lubama diagnostika oma Mobile'i rakendus taustväärtus.

Diagnostika lubada ja kirjutage logid:

1. Järgige [diagnostika lubamise kohta](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Lisage järgmine kood faili lause kasutamine:

        using System.Web.Http.Tracing;

3. Looge Jälita kirjutaja kirjutamise .NET taustväärtus diagnostikalogid, et järgmiselt:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Uuesti avaldada oma project serveri ja juurdepääs mobiilirakenduse kirjutamata koos logimine tee koodi käivitada.

5. Laadige alla ja hindate logid, nagu on kirjeldatud [kohta: allalaadimine logid](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Kohaliku autentimise silumine

Kohalik muudatuste kontrollimiseks enne avaldamine pilveteenuses rakenduse käivitamine Enamik Azure'i mobiilirakenduste taustaprogrammid, vajutage klahvi *F5* ajal rakenduses Visual Studio. Kuid on mõned täiendavad kaalutlused autentimist kasutades.

Peab teil olema pilvepõhist mobiilirakenduse abil rakenduse teenuse autentimise/autoriseerimine konfigureeritud ja teie klient peab olema määratud alternatiivse login host cloud lõpp-punkti. Oma kliendi platvormi teatud etapid dokumentatsioonist.

Veenduge, et teie mobiilne taustväärtus paigaldatud [Microsoft.Azure.Mobile.Server.Authentication] installitud. Rakenduse OWIN käivitus tunni, lisage järgmised pärast `MobileAppConfiguration` on seotud teie `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Eelmises näites, peaksite konfigureerima _authAudience_ ja _authIssuer_ rakenduse sätted iga Web.config failis olema URL oma rakenduse juures HTTPS-skeemi kasutavad. Samuti on soovitatav määrata _authSigningKey_ olema rakenduse väärtus kasutaja sisselogimine võti. Saada allkirjastamiseks võti:

1. Liikuge oma rakenduse [Azure'i portaal] 
2. Klõpsake nuppu **Tööriistad**, **Kudu**, **avage**.
3. Klõpsake saidi Kudu halduse **keskkonnas**.
4. Otsige väärtus _veebisaidi\_AUTH\_ALLKIRJASTAMINE\_klahvi_. 

Teie kohaliku rakenduse config _authSigningKey_ parameetri allkirjastamiseks klahvi abil.  Oma mobiilse taustväärtus praegu valmis valideerimiseks sõned käivitamisel kohalikult, mis hangib kliendi luba pilvepõhine lõpp-punkti.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure'i portaal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

