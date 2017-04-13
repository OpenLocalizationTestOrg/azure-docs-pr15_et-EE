<properties 
    pageTitle="Kuidas luua Web Appi Redis vahemälu | Microsoft Azure'i" 
    description="Siit saate teada, kuidas luua Web Appi Redis vahemälu" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Kuidas luua Redis vahemälu Web App

> [AZURE.SELECTOR]
- [.NET-I](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET-I](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Selle õpetuse näitab, kuidas luua ja juurutada ASP.net-i veebirakenduse, web appi teenuses Azure rakenduse Visual Studio 2015 abil. Proovi taotluse kuvatakse meeskonnatöö statistika andmebaasist ja kuvab Azure'i Redis vahemälu talletada ja vahemälu andmete toomiseks kasutada võimalust. Kui täidate õpetuse peate töötava web Appis, mis loeb ja kirjutab andmebaasi, optimeeritud Azure'i Redis vahemälu ja Azure majutada.

Saate teada:

-   Kuidas luua ASP.net-i MVC 5 web rakenduse Visual Studios.
-   Kuidas pääseda juurde andmete üksuse raames abil andmebaasist.
-   Kuidas parandada andmete läbilaskevõime ja vähendada andmebaasi laadi talletamine ja Azure Redis vahemälu abil andmete toomine.
-   Kasutamine on Redis sorditud Sea ülemise 5 meeskondadel toomiseks.
-   Kuidas ette Azure ressursse rakenduse ressursihaldur malli abil.
-   Kuidas avaldada rakenduse Azure Visual Studio abil.

## <a name="prerequisites"></a>Eeltingimused

Õpetuse lõpuleviimiseks peab teil olema järgmine kohustuslik tarkvara.

-   [Azure'i konto](#azure-account)
-   [Visual Studio 2015 Azure'i SDK .net-i jaoks](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure'i konto

Peate Azure'i konto õpetuse lõpuleviimiseks. Sa saad:

* [Avage Azure'i konto tasuta](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Saate proovida makstud Azure'i teenuste kasutatavate krediidi summa liitmisel. Isegi juhul, kui tegu on kasutanud, saate selle konto ja tasuta Azure teenuste ja funktsioonide kasutamine.
* [Aktiveerige Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN-i tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 Azure'i SDK .net-i jaoks

Õpetuse on kirjutatud Visual Studio 2015 [Azure'i SDK .net-i](../dotnet-sdk.md) 2.8.2 või uuem versioon. [Laadige uusimad Azure'i SDK Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio installitakse automaatselt SDK kui te pole seda veel.

Kui teil on Visual Studio 2013, saate [alla laadida uusima Azure'i SDK Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Selles õpetuses illustratsioonid, võivad olla teistsugused mõned ekraanid.

>[AZURE.NOTE] Sõltuvalt sellest, mitu SDK sõltuvuste teil juba on teie arvutis, SDK installimine võib võtta kaua aega, minutid pool tundi või rohkem.

## <a name="create-the-visual-studio-project"></a>Visual Studio projekti loomine

1. Avage Visual Studio ja valige **fail**, **Uus** **Projekt**.

2. Laiendage **Visual C#** **mallide** loendi, valige **pilveteenuste**ja klõpsake **ASP.net-i veebirakenduse**. Veenduge, et valitud oleks **.NET Frameworki 4.5.2** .  Tippige **nimi** rühmitusaluse **ContosoTeamStats** ja klõpsake nuppu **OK**.
 
    ![Projekti loomine][cache-create-project]

3. Valige **MVC** projekti tüüp. Tühjendage märkeruut **Host pilveteenuses** . Saate õpetuse [Azure ressursside](#provision-the-azure-resources) ja [avaldada Azure rakenduse](#publish-the-application-to-azure) edaspidised toimingut. Näiteks ettevalmistamise rakenduse teenuse web app Visual Studio jättes **Host pilves** , mis on märgitud, leiate [Azure'i rakenduse teenus, ASP.net-i ja Visual Studio abil Web Appsi kasutamise alustamine](../app-service-web/web-sites-dotnet-get-started.md).

    ![Projekti malli valimine][cache-select-template]

4. Klõpsake nuppu **OK** , et luua projekt.

## <a name="create-the-aspnet-mvc-application"></a>ASP.net-i MVC rakenduse loomine

Selle õpetuse jaotises saate luua lihtsa rakendus, mis loeb ja kuvab meeskonnatöö statistika andmebaasist.

-   [Kui seate mudeli lisamine](#add-the-model)
-   [Selle domeenikontrolleri lisamine](#add-the-controller)
-   [Vaadete konfigureerimine](#configure-the-views)

### <a name="add-the-model"></a>Kui seate mudeli lisamine

1. Paremklõpsake **Solution**Exploreris **mudelite** ja valige **Lisa**, **klassi**. 

    ![Mudeli lisamine][cache-model-add-class]

2. Sisestage `Team` klassi nimi ja klõpsake nuppu **Lisa**.

    ![Mudeli klassi lisamine][cache-model-add-class-dialog]

3. Asendada selle `using` laused ülaservas olevat `Team.cs` fail on järgmine abil laused.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Asendada määratluse soovitud `Team` klassi järgmised koodilõigu, mis sisaldab ajakohastatud `Team` klassi määratlus kui ka mõne muu üksuse raames helper tunnid. Koodi esimene lähenemine üksuse raames, mida kasutatakse selle õpetuse kohta leiate lisateavet teemast [koodi esmalt uue andmebaasi](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Topeltklõpsake **Solution Exploreris** **web.config** selle avamiseks.

    ![Web.config][cache-web-config]

3.  Lisage järgmised ühendusstringi soovitud `connectionStrings` jaotis. Ühendusstringi nimi peab vastama nime üksuse raames andmebaasi kontekstis ainekursust, mis on `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Pärast seda, lisades selle `connectionStrings` jaotis peaks välja nägema järgmine näide.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Selle domeenikontrolleri lisamine

1. Vajutage klahvi **F6** projekti koostamiseks. 
2. **Solution Exploreris**, paremklõpsake **kontrollerid** kausta ja valige **Lisa**, **kontrolleril**.

    ![Selle domeenikontrolleri lisamine][cache-add-controller]

3. Valige **MVC 5 kontrolleril abil üksuse raames vaatega**ja klõpsake nuppu **Lisa**. Kui kuvatakse tõrketeade pärast nupu **Lisa**, veenduge, et saate projekti esmalt ehitatud.

    ![Selle domeenikontrolleri klassi lisamine][cache-add-controller-class]

5. Valige ripploendist **mudeli klassi** **meeskonna (ContosoTeamStats.Models)** . Valige ripploendist **andmete kontekstis klassi** **TeamContext (ContosoTeamStats.Models)** . Tippige `TeamsController` väljale **domeenikontrolleri** nimi (kui see pole täidetakse automaatselt). Klõpsake käsku **Lisa** kontrolleril klassi loomine ja lisamine vaikimisi vaated.

    ![Selle domeenikontrolleri konfigureerimine][cache-configure-controller]

4. **Solution Exploreris**, laiendage **Global.asax** ja topeltklõpsake selle avamiseks **Global.asax.cs** .

    ![Global.asax.cs][cache-global-asax]

5. Lisage järgmised kaks abil laused, teise faili ülaosas laused abil.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Lisage järgmine rida koodi lõpus olevat `Application_Start` meetod.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. Laiendage **Solution Exploreris** `App_Start` ja topeltklõpsake `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Asendage `controller = "Home"` sisse järgmine kood on `RegisterRoutes` meetod `controller = "Teams"` nagu on näidatud järgmises näites.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Vaadete konfigureerimine

1. **Solution Exploreris**, laiendage kaust **vaadete** ja seejärel **jagatud** kaust ja topeltklõpsake **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Sisu muuta, on `title` elementi ja Asenda `My ASP.NET Application` koos `Contoso Team Stats` nagu on näidatud järgmises näites.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. Rakenduses on `body` jaotises värskendada esimene `Html.ActionLink` lause ja Asenda `Application name` koos `Contoso Team Stats` ja asendada `Home` koos `Teams`.
    -   Enne:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Pärast:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Koodi muudatusi][cache-layout-cshtml-code]

4. Vajutage **Klahvikombinatsiooni Ctrl + F5** koostamine ja käivitage rakendus. Selle rakenduse versioon loeb tulemused otse andmebaasist. Pange tähele automaatselt lisatud **MVC 5 kontrolleril abil üksuse raames vaatega** tellingud rakendusse toimingud **Loo uus**, **Redigeeri**, **üksikasjad**ja **kustutamine** . Õpetuse järgmise jaotise lisate Redis vahemälu optimeerida juurdepääs andmetele ja esitada rakendusele funktsioone.

![Rakenduse Starter][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Rakenduse kasutamiseks Redis vahemälu konfigureerimine

Selle õpetuse jaotises Konfigureerige valimi rakendus talletada ja tuua Azure'i Redis vahemälu näiteks Contoso meeskonnatöö statistika [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) vahemälu kliendi abil.

-   [Rakenduse kasutamiseks StackExchange.Redis konfigureerimine](#configure-the-application-to-use-stackexchangeredis)
-   [TeamsController klassi tulemeid vahemälu või andmebaasi värskendamine](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Värskendage töötamiseks vahemälu meetodite loomine, redigeerimine ja kustutamine](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Meeskondadel Index vaade töötamiseks vahemälu värskendamine](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Rakenduse kasutamiseks StackExchange.Redis konfigureerimine

1. Visual Studio abil StackExchange.Redis Nugeti pakett konfigureerimiseks kliendi rakendus, paremklõpsake **Solution** Exploreris projekti ja valige **Haldamine NuGet-paketid**. 

    ![Nugeti pakettide haldamine][redis-cache-manage-nuget-menu]

2. Tippige väljale Otsing tekst **StackExchange.Redis** , valige soovitud versiooni tulemuste seast ja klõpsake nuppu **Installi**.

    ![StackExchange.Redis Nugeti pakett][redis-cache-stack-exchange-nuget]

    Nugeti pakett allalaaditavate failide ja lisab nõutud komplekti viited klientrakenduse juurdepääsu Azure Redis vahemälu StackExchange.Redis vahemälu kliendi jaoks. Kui eelistate kasutada keeruka nimega versiooni **StackExchange.Redis** kliendi teek, valige **StackExchange.Redis.StrongName**; muul juhul valige **StackExchange.Redis**.

3. **Solution Exploreris**, laiendage kausta **kontrollerid** ja topeltklõpsake **TeamsController.cs** selle avamiseks.

    ![Meeskondadel kontrolleril.][cache-teamscontroller]

4. Lisage järgmised kaks lauseid **TeamsController.cs**abil.

        using System.Configuration;
        using StackExchange.Redis;

5. Lisage järgmised kaks atribuudid on `TeamsController` klassi.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Luua nimega oma arvutis olevasse faili `WebAppPlusCacheAppSecrets.config` ja selle kohta, kuhu ei märgitud sisse rakenduse proovi kood tuleks te otsustate selle sisse kuskil paigutada. Selles näites on `AppSettingsSecrets.config` fail asub aadressil `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Redigeerimine on `WebAppPlusCacheAppSecrets.config` faili ja lisada järgmised sisu. Kui käivitate rakenduse kohalik seda teavet kasutatakse ühenduse loomiseks oma Azure'i Redis vahemälu eksemplari. Allpool olevat õpetuse saate ettevalmistamise eksemplari Azure'i Redis vahemälu ja värskendada vahemälu nimi ja parool. Kui te ei plaani kohalikult valimi rakenduse käivitamiseks võite jätkata loomine ja selle faili edaspidised juhiseid, mis viitavad faili, kuna Azure juurutamisel taotlus otsib vahemälu ühendusteabe Web App rakenduse sätet, mitte seda faili. Kuna selle `WebAppPlusCacheAppSecrets.config` on juurutatud Azure oma rakendusega, et te ei vaja seda juhul, kui te ei kavatse kohalikult rakenduse käivitada.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Topeltklõpsake **Solution Exploreris** **web.config** selle avamiseks.

    ![Web.config][cache-web-config]

3. Lisage järgmine `file` atribuut on `appSettings` element. Kui kasutasite erinevad faili nime või asukohta, asendada need näites need väärtused.
    -   Enne:`<appSettings>`
    -   Pärast:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    ASP.net-i käitusaja ühendab välise juurdehindlus on faili sisu on `<appSettings>` element. Runtime ignoreerib faili atribuut, kui määratud faili ei leita. Oma saladusi (ühendusstringi vahemälu) pole rakenduse lähtekoodi kaasatud. Kui juurutate oma veebirakenduse Azure'i, kuvatakse `WebAppPlusCacheAppSecrests.config` faili ei juurutatud (see on, mida soovite). On mitu võimalust neid saladusi Azure määramiseks ja selles õpetuses need on konfigureeritud automaatselt teie eest kui olete [Azure ressursside](#provision-the-azure-resources) edaspidised õpetuses samm. Azure'i saladusi töötamise kohta leiate lisateavet teemast [head tavad juurutamisel paroole ja muud delikaatset teavet ASP.net-i ja Azure'i rakendust Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>TeamsController klassi tulemeid vahemälu või andmebaasi värskendamine

Selles näites saate tuua meeskonnatöö statistika andmebaasist või vahemälust. Meeskonnatöö statistika on talletatud nimega on sarjadesse jaotatud vahemälu `List<Team>`, ja ka kogumina sorditud abil Redis andmetüübid. Kui üksuste allalaadimine on sorditud kogum, saate tuua mõned, kõik või päringu teatud üksusi. Selles näites kuvatakse päringu ülemise 5 meeskonnad järjestatud võitnud sorditud määramine.

>[AZURE.NOTE] Ei ole vaja hoida meeskonnatöö statistika mitme vormingutes vahemälu kasutada Azure Redis vahemälu. Selle õpetuse kasutab mõned erineval viisil ja eri tüüpi andmete vahemälu andmete abil saate näidata mitme vormingud.



1. Lisage järgmine lauseid abil soovitud `TeamsController.cs` faili ülaosas teiste laused abil.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Asendage praegune `public ActionResult Index()` meetodi pärast rakendamist.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Lisage järgmised kolm meetodid on `TeamsController` klassi rakendada soovitud `playGames`, `clearCache`, ja `rebuildDB` toimingu tüüpi lisatud eelmise koodilõigu vahetamise lause.

    Funktsiooni `PlayGames` meetod värskendab meeskonnatöö statistika jäljendamine Season (aastaaeg), visualiseerimine, salvestab tulemused andmebaasi ja kustutab nüüd aegunud andmete vahemälu.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    Funktsiooni `RebuildDB` meetod reinitializes andmebaas on vaikimisi seatud meeskonnad, genereerib statistika neid ja kustutab nüüd aegunud andmete vahemälu.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    Funktsiooni `ClearCachedTeams` meetod eemaldab vahemälust kõik vahemälus talletatud meeskonnatöö statistika.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Lisage neli järgmistest meetoditest, et selle `TeamsController` klassi rakendamiseks mitmesuguseid viise ning allalaadimise meeskonnatöö statistika vahemälu ja andmebaasi. Eri viiside annab vastuseks `List<Team>` mis kuvatakse siis vaadet.

    Funktsiooni `GetFromDB` meetod loeb meeskonna statistika andmebaasist.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    Funktsiooni `GetFromList` meetod loeb meeskonnatöö statistika nimega on sarjadesse jaotatud vahemälu `List<Team>`. Kui vahemälu vahele, meeskonnatöö statistika andmebaasist loetud ja edaspidiseks kasutamiseks seejärel vahemälus talletatud. Selles näites kirjutit JSON.NET sariväljaanne serialiseerida .NET objektid ja sealt vahemälu. Lisateabe saamiseks vaadake, [Kuidas töötada .NET objektide Azure'i Redis vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    Funktsiooni `GetFromSortedSet` meetod loeb meeskonnatöö statistika vahemällu talletatud sorditud kogum. Kui vahemälu vahele, meeskonnatöö statistika andmebaasist loetud ja sorditud kogumina vahemälus talletatud.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    Funktsiooni `GetFromSortedSetTop5` meetod loeb top 5 meeskondadel vahemällu talletatud sorditud määramine. See käivitab vahemälu olemasolu kontrollimiseks on `teamsSortedSet` võti. Kui see võti pole olemas, on `GetFromSortedSet` meetodit nimetatakse lugeda meeskonnatöö statistika ja salvestaks need vahemälu. Järgmine esitama vahemällu talletatud sorditud määramine päringu Ülemiste 5 meeskonnad, mis on tagastatud.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Värskendage töötamiseks vahemälu meetodite loomine, redigeerimine ja kustutamine

Selle valimi osana loodud tellingud kood sisaldab lisamine, redigeerimine ja kustutamine meeskondadel võimalusi. Iga kord meeskond on lisatud, redigeerida või eemaldada, saab andmete vahemälu aegunud. Selles jaotises saate muuta järgmised kolm võimalust tühjendage vahemällu talletatud meeskondadel nii, et vahemälu ei saa sünkroonitud andmebaasi.

1. Liikuge sirvides soovitud `Create(Team team)` meetod on `TeamsController` klassi. Kõne lisamine soovitud `ClearCachedTeams` meetod, nagu on näidatud järgmises näites.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Liikuge sirvides soovitud `Edit(Team team)` meetod on `TeamsController` klassi. Kõne lisamine soovitud `ClearCachedTeams` meetod, nagu on näidatud järgmises näites.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Liikuge sirvides soovitud `DeleteConfirmed(int id)` meetod on `TeamsController` klassi. Kõne lisamine soovitud `ClearCachedTeams` meetod, nagu on näidatud järgmises näites.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Meeskondadel Index vaade töötamiseks vahemälu värskendamine

1. **Solution Exploreris**, laiendage kausta **vaateid** , siis **meeskondadel** kaust ja topeltklõpsake **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Faili ülaosas otsige järgmine lõik element.

    ![Tabelis][cache-teams-index-table]

    See on luua uus meeskond link. Asendage lõik elemendi järgmises tabelis. Selles tabelis on toiming uue meeskonna loomine, uue season, visualiseerimine ja esitamine, vahemälu tühjendamine, toomine meeskonnad vahemälust mitmeid vorme, toomine andmebaasist meeskonna ja värske näidisandmetega andmebaasi taastamine.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Liikuge kerides **Index.cshtml** faili ja lisage järgmine `tr` element, nii et see on viimase rea viimast tabeli faili.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    See kuvab väärtuse `ViewBag.Msg` mis sisaldab aruande oleku kohta praeguse toimingu, mis on määratud toimingu lingid eelmises juhises klõpsamisel.   

    ![Olekuteade][cache-status-message]

4. Vajutage klahvi **F6** projekti koostamiseks.

## <a name="provision-the-azure-resources"></a>Azure'i ressursside

Rakenduse Azure majutada, peate esmalt ette Azure'i teenused, mida teie rakendus nõuab. Selles õpetuses valimi rakendus kasutab järgmisi Azure teenuseid.

-   Azure'i Redis vahemälu
-   Rakenduse teenuse Web App
-   SQL-andmebaas

Nende teenuste juurutamine uude või olemasolevasse ressursirühm teie valitud, klõpsake nuppu Järgmine **Deploy Azure** .

[! [Juurutada Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

See **Deploy Azure** nupp kasutab [loomine Web Appi pluss Redis vahemälu pluss SQL-andmebaasi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Kiirjuhend](https://github.com/Azure/azure-quickstart-templates) malli ette järgmisi teenuseid ja ühendusstringi seadmine SQL-andmebaasi ja rakenduse sätet Azure'i Redis vahemälu ühendusstring.

>[AZURE.NOTE] Kui teil pole Azure'i konto, saate [luua tasuta Azure'i konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) vaid paar minutit.

Klõpsake nuppu **Deploy Azure** suunab teid Azure portaali ja käivitab ressursse, mis on kirjeldatud malli abil loomise protsess.

![Azure'i juurutamine][cache-deploy-to-azure-step-1]

1. Enne **kohandatud juurutus** , valige Azure tellimuse kasutada, ja valige ressursi olemasolevasse rühma või luua uue ja ressursside rühma asukoha määramine.
2. Enne **Parameetrid** , määrake administraatori konto nimi (**ADMINISTRATORLOGIN** - ei kasuta **administraator**), administraatori parooli (**ADMINISTRATORLOGINPASSWORD**) ja andmebaasi nimi (**DATABASENAME**). Muud parameetrid on konfigureeritud tasuta rakenduse teenuse majutusteenuse leping ja alumise maksumus suvandid ei tule tasuta taseme SQL-andmebaasi ja Azure Redis vahemälu.
3. Soovi korral muid sätteid muuta või säilitada vaikeväärtused ja klõpsake nuppu **OK**.


![Azure'i juurutamine][cache-deploy-to-azure-step-2]

1. Klõpsake nuppu **Läbivaatus juriidilised tingimused**.
2. **Ostu** enne kasutustingimustega ja klõpsake nuppu **osta**.
3. Ressursside ettevalmistamise alustamiseks klõpsake nuppu **Loo** **kohandatud juurutus** enne.

Juurutamise edenemise kuvamiseks klõpsake olekuala ikoon ja seejärel **juurutamise alustamine**.

![Juurutamise alustamine][cache-deployment-started]

Juurutamise olekut saate vaadata enne **Microsoft.Template** .

![Azure'i juurutamine][cache-deploy-to-azure-step-3]

Kui ettevalmistamise on lõpule jõudnud, saate avaldada oma rakenduse Visual Studio Azure.

>[AZURE.NOTE] Mis võib tekkida ebausaldusväärsete vigu kuvatakse **Microsoft.Template** enne. Levinud vigade on liiga palju SQL-i serverite või liiga palju tasuta rakenduse teenus hosting plaanid tellimuse kohta. Mis tahes tõrgete lahendamine ja taaskäivitage protsess, klõpsates **ümberkorraldamine** **Microsoft.Template** tera või **Deploy Azure** nupp selles õpetuses.

## <a name="publish-the-application-to-azure"></a>Azure'i rakenduse avaldamine

Õpetuse selles etapis tuleb teil avaldada Azure rakendus ja käivitage see pilveteenuses.

1. Paremklõpsake **ContosoTeamStats** projekti Visual Studio ja valige käsk **Avalda**.

    ![Avaldamine][cache-publish-app]

2. Klõpsake **Microsoft Azure'i rakendust Service**.

    ![Avaldamine][cache-publish-to-app-service]

3. Valige tellimus, kasutada, kui loote Azure ressursse, laiendage ressursirühm, mis sisaldab ressursse, valige soovitud Web App ja siis nuppu **OK**. Kui kasutasite **Deploy Azure** nupp oma veebirakenduse nimi algab **veebisaidi** järgneb mõned täiendavad märgid.

    ![Valige Web App][cache-select-web-app]

4. **Kinnitada ühenduse** sätete kinnitamiseks klõpsake nuppu ja seejärel klõpsake nuppu **Avalda**.

    ![Avaldamine][cache-publish]

    Mõne hetke pärast on avaldamise protsessi lõpuleviimine ja brauseri käivitatakse töötab proovi taotluse. Kui te saate DNS-i tõrke kontrollimise või avaldamine ja ebausaldusväärsete Azure ressursid rakenduse hiljuti on lõppenud, oodake pisut ja proovige uuesti.

    ![Lisatud vahemälu][cache-added-to-application]

Järgmises tabelis kirjeldatakse iga toimingu linki proovi rakendusest.

| Toiming                  | Kirjeldus                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Loo uus              | Saate luua uue meeskonna.                                                                                                                                               |
| Esita Season (aastaaeg)             | Season (aastaaeg) visualiseerimine ja esitamine, värskendage meeskonnatöö statistika ja kõik aegunud meeskonnatöö andmete vahemälu.                                                                          |
| Vahemälu tühjendamine             | Meeskonnatöö statistika vahemälu tühjendamine                                                                                                                             |
| Loendi vahemälust         | Saate tuua meeskonnatöö statistika vahemälu. Kui vahemälu vahele, statistika andmebaasi laadimine ja vahemällu salvestamine edaspidiseks kasutamiseks.                                        |
| Sorditud vahemälust määramine   | Tuua meeskonnatöö statistika kasutades sorditud vahemälu. Kui vahemälu vahele, statistika andmebaasi laadimine ja sorditud määratud vahemällu salvestamine.  |
| Top 5 meeskondadel vahemälust  | Saate tuua ülemise 5 meeskondadel kasutades sorditud vahemälust. Kui vahemälu vahele, statistika andmebaasi laadimine ja sorditud määratud vahemällu salvestamine. |
| DB alla laadida            | Saate tuua meeskonnatöö statistika andmebaasist.                                                                                                                       |
| Taastada DB              | Andmebaasi taastada ja uuesti meeskonna näidisandmetega.                                                                                                        |
| Redigeeri / üksikasjad / kustutamine | Rühma redigeerimine, meeskond üksikasjade kuvamine, kustutamine meeskond.                                                                                                             |


Katsetage eri allikatest pärit andmete toomine klõpsake mõned toimingud. Pole aega kulub mitmesuguseid viise ning andmete toomisel andmebaas ja vahemälu erinevused.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Kustutage ressursse, kui olete lõpetanud, rakenduse

Kui olete lõpetanud, valimi kuueosalisest rakendusega, saate kustutada Azure ressursside kasutada maksumuse ja ressursse säästa. Kui kasutate nupp **Deploy Azure'i** [Azure ressursside](#provision-the-azure-resources) jaotises ja kõik teie ressursid sisalduvad sama ressursirühm, võite kustutada need koos ühe toiminguga kustutades ressursirühma.

1. [Azure'i portaali](https://portal.azure.com) sisse logida ja klõpsake nuppu **ressurss rühmad**.
2. Tippige oma ressursirühm nime **... üksuste filtreerimine** tekstiväli.
3. Klõpsake **…** ressursirühma paremal.
4. Klõpsake nuppu **Kustuta**.

    ![Kustutamine][cache-delete-resource-group]

5. Tippige oma ressursi rühma nimi ja klõpsake nuppu **Kustuta**.

    ![Kustutamise kinnitamine][cache-delete-confirm]

Mõne hetke pärast ressursirühma ja kõik selle keskkonnas ressursid on kustutatud.

>[AZURE.IMPORTANT] Võtke arvesse, et kustutada ressursirühma on pöördumatu ja ressursirühma ja kõik ressursid see jäädavalt kustutatud. Veenduge, et ei kustutate kogemata valesti ressursirühm või ressursse. Kui olete loonud hosting selle ressursi olemasolevasse rühma sees valimi ressursse, saate kustutada iga ressursi eraldi nende vastavate labad.

## <a name="run-the-sample-application-on-your-local-machine"></a>Valimi rakenduse käivitada oma kohalikus arvutis

Käivitage rakendus kohalikult teie arvutis, peate Azure'i Redis vahemälu eksemplar, kus teie andmete vahemällu. 

-   Kui teil on avaldatud Azure rakenduse eelmises jaotises kirjeldatud, saate kasutada Azure Redis vahemälu eksemplari, mis on ette valmistatud juhise.
-   Kui teil on mõne muu olemasoleva Azure'i Redis vahemälu eksemplari, saate mis selle valimi töötama.
-   Kui teil on vaja luua Azure'i Redis vahemälu eksemplar, saate selle juhiste loomine [vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Kui teil on valitud või loodud vahemälu kasutama, liikuge sirvides vahemälu Azure portaali ja tuua [hosti nimi](cache-configure.md#properties) ja [kiirklahvide](cache-configure.md#access-keys) vahemälu. Juhised leiate teemast [vahemälusätete konfigureerimine Redis](cache-configure.md#configure-redis-cache-settings).

1. Avage soovitud `WebAppPlusCacheAppSecrets.config` [konfigureerimine rakenduse kasutamiseks Redis vahemälu](#configure-the-application-to-use-redis-cache) etapil selles õpetuses teie valitud Editori kasutamise ajal loodud faili.

2. Redigeerimine on `value` atribuut ja asendada `MyCache.redis.cache.windows.net` [hosti nimi](cache-configure.md#properties) oma vahemälu ja määrake, kas [põhi- või klahvi](cache-configure.md#access-keys) vahemälu parool.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Vajutage **Klahvikombinatsiooni Ctrl + F5** käivitage rakendus.

>[AZURE.NOTE] Pange tähele, et kuna rakendus, sh andmebaasi, töötab kohalikult ja Redis vahemälu majutab Azure, vahemälu võidakse kuvada täita andmebaas. Parima jõudluse tagamiseks tuleks klientrakendusega ja Azure Redis vahemälu eksemplari samasse asukohta. 

## <a name="next-steps"></a>Järgmised sammud

-   Lisateave [ASP.net-i](http://asp.net/) saidi [ASP.net-i MVC 5 töötamise alustamine](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) .
-   Rohkem näiteid loomise rakendus teenuses ASP.net-i Web App leiate [loomine ja juurutamine ASP.net-i web app Azure'i rakendust Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 ühendamine [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Lisateavet quickstarts HealthClinic.biz demo, leiate [Azure'i Arendaja tööriistad Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Lisateavet [koodi esmalt uue andmebaasi](https://msdn.microsoft.com/data/jj193542) lähenemine üksuse raames, mida kasutatakse selles õpetuses.
-   Lisateavet [veebirakenduste teenuses Azure rakendus](../app-service-web/app-service-web-overview.md).
-   Siit saate teada, kuidas [kuvari](cache-how-to-monitor.md) vahemälu Azure'i portaalis.

-   Azure'i Redis vahemälu premium funktsioonid
    -   [Kuidas seadistada püsimine Premium Azure'i Redis vahemälu jaoks](cache-how-to-premium-persistence.md)
    -   [Kuidas seadistada rühmitamise Premium Azure'i Redis vahemälu jaoks](cache-how-to-premium-clustering.md)
    -   [Kuidas konfigureerida virtuaalse võrgu Premium Azure'i Redis vahemälu tugi](cache-how-to-premium-vnet.md)
    -   Teemast [Azure Redis vahemälu KKK](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) suurus, läbilaskevõime ja läbilaskevõime koos premium vahemälu kohta lisateabe saamiseks.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

