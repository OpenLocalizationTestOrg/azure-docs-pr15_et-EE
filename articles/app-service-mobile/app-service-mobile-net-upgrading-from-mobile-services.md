<properties
    pageTitle="Azure'i rakendust Service versioonile mobiilsideseadmete teenuste"
    description="Saate teada, kuidas hõlpsasti uuendada oma Mobile teenuste rakenduse App teenuse mobiilirakenduse"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Uuendada oma olemasoleva .NET Azure mobiilsideteenuse rakenduse teenus

Rakenduse teenuse Mobile on uus võimalus koostada mobiilirakenduste kaudu Microsoft Azure'i. Lisateavet leiate teemast [millised mobiilirakenduste?].

Selles teemas kirjeldatakse, kuidas võtta kasutusele Azure Mobile teenused olemasoleva .NET taustväärtus rakenduse uus rakendus teenuse Mobile'i rakendused. Kui teete seda uuendada, olemasoleva Mobile teenuste rakenduse saate jätkata.   Kui teil on vaja uuendada Node.js kirjutamata rakenduse viidata [üleminekut Node.js Mobile teenuste](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Mobiilse taustväärtus uuendamisel Azure'i rakendust Service on juurdepääs kõik rakenduse teenuse funktsioonid ja vastavalt [rakenduse teenuse hinnad], Mobile teenuste hinnad on arve.

##<a name="migrate-vs-upgrade"></a>Migreerimine ja täiendamine

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Soovitatav on selle [migreerimine](app-service-mobile-migrating-from-mobile-services.md) enne uuendamist läbi. Sellisel viisil mõlemad versioonid rakenduse sellele sama rakenduse teenuse kavandamine ja tekkida täiendava tasuta.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Mobile'i rakendused .net-i serveri SDK täiustused

Täiendamine versiooniks uue [Mobile'i rakendused SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) pakub järgmised eelised:

- Rohkem paindlikkust Nugeti eelnevuste. Majutusteenuse keskkonnas enam pakub oma versiooni NuGet-paketid abil saate alternatiivne ühilduvad versioonid. Juhul, kui on uue kriitilised veaparandused või Mobile Server SDK või sõltuvused turbevärskendusi, peate värskendama oma teenuse käsitsi.

- Rohkem paindlikkust mobiilsideseadmete SDK. Saate otseselt määrata, millised funktsioonid ja marsruudib on häälestatud, nt autentimine, tabeli API-d ja tõuketeatised registreerimise lõpp-punkti. Lisateabe saamiseks vaadake, [Kuidas kasutada .net-i server Azure'i mobiilirakenduste SDK](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Muud tüüpi ASP.net-i projekti ja marsruudib tugi. Nüüd saate majutada MVC ja veebiteenuste domeenikontrollerid sama projekti mobiilsideseadmete kirjutamata projekti.

- Uus rakendus teenuse autentimine funktsioone, mis võimaldab teil kasutada levinud autentimise konfigureerimine web ja mobiilirakendused tugi.

##<a name="overview"></a>Tavaline versioonitäienduse ülevaade

Paljudel juhtudel üleminekut on sama lihtne kui üleminek uue mobiilirakenduste server SDK ja uuestipublitseerimist oma koodi peale Mobile'i rakendus uue eksemplari. Puuduvad, kuid mõnel juhul, milleks on vaja täiendavat konfiguratsiooni, nt täpsemalt autentimise stsenaariumid ja töötamise ajastatud tööde haldamine. Kõik need käsitletakse hiljem jaotised.

>[AZURE.TIP] Soovitatav on, et lugeda ja mõista selles teemas ülejäänud täielikult enne versiooniuuendust. Märkige üles kõiki funktsioone kasutada all kuvatud.

Mobile Services kliendi SDK-d on **Uus mobiilirakenduste server SDK ühildub** . Jätkumise oma rakenduse pakkumiseks peaks muudatuste avaldamine pole praegu serveeritakse avaldatud kliendid saidil. Selle asemel loote uue mobiilirakenduse, mis toimib duplikaat. Selle rakenduse saate paigutada samale rakenduse teenusleping proovima täiendavaid rahalisi kulusid.

Siis on rakenduse kaks versiooni: üks, mis jääb samaks, ja pakutakse avaldatud rakenduste loodusest, ja teise, mida saate siis täiendada ja target uue klientrakenduse väljaandes. Saate teisaldada ja testige oma tempos oma koodi, kuid veenduge, et kõik teete veaparandused saada rakendatud nii. Kui tunnete, et soovitud arvu looduses rakendused on värskendatud üle uusimale versioonile klient, saate kustutada algse migreeritud rakenduse kui soovite.

Täieliku ülevaate versiooniuuenduse jaoks on järgmine:

1. Looge uus Mobile'i rakendus
2. Värskendage projekti kasutama uut Server SDK-d
3. Vabastage uue klientrakenduse versiooni
4. (Valikuline) Teie algne migreeritud eksemplari kustutamine

##<a name="mobile-app-version"></a>Teise rakenduse eksemplari loomine
Esimene samm üleminekut on luua mobiilirakenduse ressurss, mis kuvatakse majutada oma rakenduse uus versioon. Kui teil on juba viiakse olemasoleva mobiilsideseadmete teenuse, mida soovite luua selle versiooni sama majutusteenuse lepingu. Avage [Azure portaali] ja liikuge migreeritud rakenduse. Märkige üles rakendus teenuse leping see töötab.

Järgmiseks luua teine rakenduse eksemplari [.NET taustväärtus loomise juhiseid](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Kui teil palutakse teil valida rakenduse teenusleping või "hosting lepingu" valige migreeritud rakenduse leping.

Tõenäoliselt soovite kasutada sama andmebaasi ja teate jaoturi, nagu tegite Mobile teenused. Kopeerige need väärtused, avades [Azure portaali] ja liikumine algse rakendus ja seejärel klõpsake nuppu **sätted** > **rakenduse sätted**. Kopeerige jaotises **Ühendusstringi**, `MS_NotificationHubConnectionString` ja `MS_TableConnectionString`. Liikuge versioonitäienduse uut saiti ja kleepige need, mis tahes olemasolevaid väärtuste ülekirjutamise. Korrake seda toimingut rakenduse sätted rakenduse teie vajadustele. Kui ei kasuta migreeritud teenus, saate lugeda ühendusstringi ja rakenduse sätete **konfigureerimine** menüü Mobile teenuste jaotise [Azure klassikaline portaalis].

ASP.net-i projekti rakenduse kopeerimiseks ja selle avaldada oma uus sait. Rakendust värskendatakse uue URL-i klientrakenduse koopia, saate kontrollida, kas kõik toimib ootuspäraselt.

## <a name="updating-the-server-project"></a>Project serveri värskendamine

Mobiilirakenduste pakub uus [Mobile'i rakendus serveri Tarkvaraarenduskomplektist] , mis sisaldab palju samu funktsioone, mis Mobile Services käitusaja. Kõigepealt tuleb eemaldada kogu viiteid Mobile teenuste paketid. Nugeti paketi manager, saate otsida `WindowsAzure.MobileServices.Backend`. Enamik rakendused kuvatakse mitu pakettide siin, sh `WindowsAzure.MobileServices.Backend.Tables` ja `WindowsAzure.MobileServices.Backend.Entity`. Sellisel juhul alustada madalaimate paketi puus sõltuvus nagu `Entity`, ja eemaldage see. Vastava viiba kuvamisel eemaldage kõik sõltuvad paketid. Korrake seda toimingut, kuni olete eemaldanud `WindowsAzure.MobileServices.Backend` ise.

Selles etapis on teil projekt, mis pole enam viitab Mobile teenuste SDK-d.

Järgmiseks tuleb lisada viited SDK Mobile'i rakendused. Seda versioonitäiendust enamik arendajate soovite alla laadida ja installida selle `Microsoft.Azure.Mobile.Server.Quickstart` paketti, nagu see tõmbab kogu nõutud määramine.

On üsna vähe koostaja tõrgete tuleneb funktsiooni SDK-d erinevused, kuid need on lihtne aadress ja ülejäänud selles jaotises asuvad.

### <a name="base-configuration"></a>Base konfigureerimine

Seejärel WebApiConfig.cs, saate asendada:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

abil

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Kui soovite uue .net-i serveri SDK ja Lisa/eemalda funktsioonid rakenduse kohta lisateavet, lugege teemat [.net-i serveri SDK kasutamise kohta] .

Kui teie rakendus teeb autentimist funktsioone kasutada, peate ka mõne OWIN vahevara registreerimiseks. Sel juhul tuleks ülaltoodud konfigureerimiskoodi viimine OWIN käivitus uutel.

1. Lisage Nugeti pakett `Microsoft.Owin.Host.SystemWeb` kui see pole juba kaasatud projekti.
2. Visual Studio projekti paremklõps ja valige käsk **Lisa** -> **Uus üksus**. Valige **Web** -> **Üldine** -> **OWIN käivitus klassi**.
3. Nihutage eespool kood MobileAppConfiguration kaudu `WebApiConfig.Register()` abil soovitud `Configuration()` käivitus uutel meetodit.

Veenduge, et selle `Configuration()` meetod lõpus on küsimus:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

On autentimisega seotud täiendavad muudatused, mis asuvad allpool jaotises täielik autentimist.

### <a name="working-with-data"></a>Andmetega töötamine

Mobile teenused, mobiilirakenduse nimi oli skeemi vaikenime üksuse raames häälestus.

Tagamaks, et teil on sama nagu varem, kasutage järgmist määramiseks skeemiga DbContext rakenduse viitab skeemiga:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Veenduge, et teil määrata, kui te ei tee eeltoodud MS_MobileServiceName. Kui rakenduse kohandatud see varem, saate sisestada ka teise skeemi nimi.

### <a name="system-properties"></a>Süsteemiatribuudid

#### <a name="naming"></a>Nime andmise

Azure Mobile teenused serveris SDK Süsteemiatribuudid sisaldavad alati kahekordsete allkriipsu (`__`) eesliide atribuudid:

- __createdAt
- __updatedAt
- __deleted
- __version

Mobile Services kliendi SDK-d on teisiti loogika sõelumine Süsteemiatribuudid selles vormingus.

Azure'i Mobile rakendustes Süsteemiatribuudid enam on erivormingu ja on järgmised nimed:

- createdAt
- updatedAt
- kustutatud
- versioon

Mobiilirakenduste kliendi SDK-d kasutada uut atribuutide nimesid nii, et muudatusi on vaja kliendi kood. Siiski kui teete otse ülejäänud kõnede teenust siis tuleks muuta oma päringuid vastavalt sellele.

#### <a name="local-store"></a>Kohalikku salve

Muudatustest Süsteemiatribuudid nimed tähendab, et ühenduseta sünkroonimine kohaliku andmebaasi Mobile teenuste ei ühildu mobiilirakenduste kohta. Võimaluse korral tuleks vältida üleminekut rakenduste Mobile teenuste mobiilirakenduste kuni pärast ootel muudatusi saadetud serveriga klient. Seejärel täiendatud rakenduse peaksite kasutama uue andmebaasi failinimi.

Kui kliendi rakendus on versioonile Mobile teenustepakkujate mobiilirakenduste ajal on ootel muudatusi ühenduseta toiming järjekorda, siis süsteemi andmebaasi tuleb värskendada kasutama uut veerunimede. IOS-i, klõpsake selle saavutamiseks kerge migratsioon abil saate muuta veergude nimed. Android ja .net-i hallatavate klient, kirjutage kohandatud SQL-i ümber nimetada teie andmete objekti tabelite veerud.

IOS, on teil vaja muuta oma Core andmete skeem teie andmete üksuste vastavaks järgmist. Pange tähele, et atribuudid `createdAt`, `updatedAt` ja `version` ei ole enam mõne `ms_` eesliide:

| Atribuut |  Tüüp   | Märkus                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID        | String, mis on tähistatud nõutav  | primaarvõtme remote poes         |
| createdAt | Kuupäev    | (valikuline) kaardid createdAt süsteemi atribuut         |
| updatedAt | Kuupäev    | (valikuline) kaardid updatedAt süsteemi atribuut         |
| versioon   | String  | (valikuline) kasutatakse konfliktide, kaardid versiooni tuvastamiseks |

#### <a name="querying-system-properties"></a>Päringute Süsteemiatribuudid

Azure'i Mobile teenustes Süsteemiatribuudid ei saadeta vaikimisi, kuid ainult siis, kui palutakse, kasutades päringu string `__systemProperties`. Seevastu Azure'i mobiilirakenduste süsteemi atribuudid on **alati valitud** , kuna objektimudel serveri SDK osa nendega.

See muudatus mõjutab peamiselt kohandatud domeeni haldurid, nt pikendamine rakendusi `MappedEntityDomainManager`. Mobile teenused, kui klient küsib kunagi mis tahes Süsteemiatribuudid, on võimalik kasutada mõnda `MappedEntityDomainManager` mis tegelikult vastendamine kõik atribuudid. Azure Mobile'i rakendused, kuvatakse vastendamata atribuutidest põhjustada tõrke toomine päringute.

Lihtsaim viis probleemi lahendamiseks on muuta oma DTOs nii, et nad pärivad `ITableData` asemel `EntityData`. Lisage soovitud `[NotMapped]` atribuut välju, mida vahele jätta.

Näiteks järgmine määratleb `TodoItem` pole Süsteemiatribuudid abil:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Märkus: kui teil tekib tõrgete `NotMapped`, lisada selle komplekti `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobile teenuste sisalduv mõned tugi CORS ümbriste ASP.net-i CORS lahendus. Et anda arendaja rohkem võimalusi, nii et saate kasutada otse [ASP.net-i CORS tugi](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)on eemaldatud see mähkimine kiht.

Probleemse CORS kasutamisel on see on `eTag` ja `Location` päised lubama selleks kliendi SDK-d töötaks õigesti.

### <a name="push-notifications"></a>Tõuketeatised
Tõuketeatised, on peamised üksus, mille leiate puuduvad serveri SDK PushRegistrationHandler klassi. Registreerimise käsitletakse pisut teisiti mobiilirakenduste ja tagless registreerimise on vaikimisi sisse lülitatud. Siltide haldamine võib teostada, kasutades kohandatud API-d. Lugege lisateavet [registreerimisel silte](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) juhiseid.

### <a name="scheduled-jobs"></a>Ajastatud tööd
Ajastatud tööd on koondatud mobiilirakenduste kohta, et mis tahes olemasolevaid tööd, mis on teie .NET taustväärtus kuvatakse, peate eraldi. Üheks võimaluseks on plaanitud [Web töö] mobiilirakenduse kood saidi loomiseks. Võite häälestamine kontrolleril, mis hoiab teie töö kood ning konfigureerida [Azure Scheduler] tabab seda lõpp-punkti oodatud ajakavas.

### <a name="miscellaneous-changes"></a>Logimise.
Kõik ApiControllers, mis on tarbitud mobiiliklienti, peab olema nüüd selle `[MobileAppController]` atribuut. See pole enam, et teised ei mõjuta mobiilsideseadmete formatters minna ApiControllers vaikimisi kaasatud.

Funktsiooni `ApiServices` objekt pole enam SDK osa. Juurdepääs Mobile'i rakendus sätted, võite kasutada järgmist:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Samuti logimine on nüüd täita standard ASP.net-i Jälita kirjutamist abil:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Kaalutlused autentimine

Mobile teenuste komponentide autentimine on teisaldatud kohe rakenduse teenuse autentimise ja luba funktsiooni. Saate teada, lubamise selle saidi [lisamine oma mobiilirakenduse autentimise](app-service-mobile-ios-get-started-users.md) teema lugemise kohta.

Mõned pakkujate AAD, Facebook ja Google, nt peaks oskama kasutada olemasoleva registreerimise kopeerimine rakenduse kaudu. Peate lihtsalt liikuge identiteedipakkuja juures portaal ja registreerimise uue ümbersuunamise URL-i lisamine. Kliendi ID ja salajane konfigureerimist rakenduse teenuse autentimise/autoriseerimine.

### <a name="controller-action-authorization"></a>Selle domeenikontrolleri toimingu autoriseerimine
Esinemisvõimaluse soovitud `[AuthorizeLevel(AuthorizationLevel.User)]` atribuut peab nüüd standard ASP.net-i abil muuta `[Authorize]` atribuut. Lisaks kontrollerid on nüüd anonüümne vaikimisi nagu muude rakenduste ASP.net-i.
Kui kasutate mõnda muud AuthorizeLevel suvandid, nt administraator või rakendust, Pange tähele, et need on kadunud. Selle asemel saate häälestamine AuthorizationFilters ühiskasutusega saladusi või AAD teenuse peamise lubamiseks-teenus nõuab turvaline konfigureerimine.

### <a name="getting-additional-user-information"></a>Täiendavad kasutaja teabe saamine

Saate täiendavat teavet, sh Accessi sõned kaudu soovitud `GetAppServiceIdentityAsync()` meetod:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Lisaks kui rakenduse võtab sõltuvused kasutaja ID-d, nt salvestades andmebaasi, on oluline märkida, et kasutaja ID-de vahel Mobile'i rakendus teenuse mobiilirakenduste erinevad. Mobile teenuste kasutaja ID-d, saate siiski küll saada. Kõik ProviderCredentials alaliikide on kasutajanimi atribuut. Nii jätkates enne näites:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Kui teie rakendus sõltuvusi kasutaja ID-d, on oluline, et saate kasutada sama registreerimine pakkujaga identiteedi võimalusel. Kasutaja ID-d on tavaliselt rakendatud rakenduse registreerimine, mida kasutati, nii, et uus registreerimise tutvustus võib põhjustada probleeme sobitamine kasutajad oma andmetega.

### <a name="custom-authentication"></a>Kohandatud autentimine

Kui teie rakendus kasutab kohandatud autentimine lahenduse, mida soovite veenduge, et täiendatud saidil on juurdepääs süsteem. Järgige uue kohandatud autentimise [.net-i serveri SDK ülevaade] integreerida teie lahendus. Pange tähele, et kohandatud autentimine komponendid on endiselt eelvaade.

##<a name="updating-clients"></a>Klientide värskendamine
Kui teil on mõni funktsionaalseid mobiilirakenduse kirjutamata, saate töötada oma kliendi rakendus, mis kasutab seda uue versiooni. Mobiilirakenduste sisaldab ka uus versioon, mille kliendi SDK-d ja sarnaselt serveri uuele versioonile üle, peate kogu viiteid Mobile teenuste SDK-d eemaldage enne installimist Mobile'i rakenduste versioonid.

Üks peamisi muudatusi vahelised erinevused on puhul on ehitajatel pole enam vaja taotluse võti. Saate nüüd lihtsalt läheb Mobile rakenduse URL. Näiteks klõpsake .net-i kliendid, on `MobileServiceClient` ehitaja on nüüd:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Saate lugeda installimist uus SDK-d ja uue struktuur kaudu linkidest kasutamise kohta:

- [iOS-i versioon 3.0.0 või uuem versioon](app-service-mobile-ios-how-to-use-client-library.md)
- [.Net-i (Windows/Xamarin) versioon 2.0.0 või uuem versioon](app-service-mobile-dotnet-how-to-use-client-library.md)

Kui rakenduse Tõuketeatiste kasutada, märkige kindla registreerimise juhiseid iga platvormi, kui seal on olemas mõned muudatused ka.

Kui olete valmis uue klientrakenduse versiooni, proovi uuele versioonile üle viidud serveri projekti suhtes. Pärast kontrollimine, et see toimib, saate vabastada kliendid oma rakenduse uus versioon. Lõpuks, kui klientide olnud võimalus neid värskendusi, saate kustutada rakenduse versiooni Mobile teenused. Selles etapis täielikult versiooniks rakenduse teenuse mobiilirakenduse abil uusima mobiilirakenduste serveri SDK.

<!-- URLs. -->

[Azure'i portaal]: https://portal.azure.com/
[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
[Mis on mobiilirakenduste kohta?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobiilse rakenduse Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure'i ajasti]: /en-us/documentation/services/scheduler/
[Web töö]: ../app-service-web/websites-webjobs-resources.md
[Kuidas kasutada .net-i serveri SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Rakenduse teenuse hinnad]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.Net-i serveri SDK ülevaade]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
