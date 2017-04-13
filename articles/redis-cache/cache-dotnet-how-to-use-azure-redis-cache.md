<properties 
    pageTitle="Kuidas kasutada Azure Redis vahemälu | Microsoft Azure'i" 
    description="Saate teada, kuidas oma Azure rakenduste Azure Redis vahemälu jõudluse parandamiseks" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Kuidas kasutada Azure Redis vahemälu

> [AZURE.SELECTOR]
- [.NET-I](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET-I](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Sellest juhendist näitab, kuidas **Azure'i Redis vahemälu**kasutamise alustamine. Microsoft Azure'i Redis vahemälu põhineb populaarsed avatud allika Redis vahemälu. See annab teile juurdepääsu turvaline, sihtotstarbeline Redis vahemälu, haldab Microsoft. Azure'i Redis vahemälu abil loodud vahemälu on kättesaadav, mis tahes rakendusest Microsoft Azure'i sees.

Microsoft Azure'i Redis vahemälu on saadaval järgmised astme.

-   **Lihtne** – üks sõlm. Mitme suurused 53 GB.
-   **Standard** -kaks sõlme primaarne/koopia. Mitme suurused 53 GB. 99,9% SLA.
-   **Premium** – esmase kaks sõlme koopia kuni 10 shards abil. Mitme 6 GB 530 GB suurused (kontakt rohkem). Kõik Standard taseme funktsioonid ja veel sh tugi [Redis kobar](cache-how-to-premium-clustering.md), [Redis püsimine](cache-how-to-premium-persistence.md)ja [Azure virtuaalse võrgu](cache-how-to-premium-vnet.md). 99,9% SLA.

Iga taseme erineb nii funktsioonid ja hinnakirjad. Hinnad kohta lisateabe saamiseks lugege teemat [Vahemälu hinnad üksikasjad][].

Sellest juhendist näitab, kuidas kasutada [StackExchange.Redis][] C abil\# kood. Stsenaariumid, kaetud kaasata **loomine ja konfigureerimine vahemälu**, **konfigureerida vahemälu kliendid**ja **lisamine ja eemaldamine objektid vahemälu**. Azure'i Redis vahemälu kasutamise kohta lisateabe saamiseks vaadake [Järgmist][] jaotist. Üksikasjaliku juhendi koostamise soovitud MVC ASP.net-i vahemälu Redis web app, vaadake, [Kuidas luua Web Appi abil Redis vahemälu](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Azure'i Redis vahemälu kasutamise alustamine

Azure'i Redis vahemälu alustamine on lihtne. Alustamiseks ettevalmistamine ja konfigureerida vahemälu. Järgmisena saate konfigureerida vahemälu kliendid nii, et nad pääsevad vahemälu. Kui vahemälu kliendid on konfigureeritud, saate nendega tööd alustada.

-   [Vahemälu loomine][]
-   [Vahemälu kliendid konfigureerimine][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Vahemälu luua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Juurdepääs vahemälu pärast selle loomist

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Vahemälu konfigureerimise kohta lisateabe saamiseks vaadake, [Kuidas konfigureerida Azure Redis vahemälu](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Vahemälu kliendid konfigureerimine

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Kui kliendi projekti on konfigureeritud lubama vahemällu, saate kasutada võtteid, mis on kirjeldatud järgmistes lõikudes vahemälu töötamiseks.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Vahemälu töötamine

Selle jaotise juhised on kirjeldatud, kuidas teha levinud toiminguid ja vahemälu.

-   [Ühenduse loomine vahemälu][]
-   [Lisamine ja tuua objektide vahemälu][]
-   [.Net-i vahemälu objektidega töötamine](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Ühenduse loomine vahemälu

Programmiliselt töötada vahemälu, peate vahemälu viide. Lisage järgmine tekst, millest mis tahes faili, mida soovite kasutada StackExchange.Redis kliendi juurdepääsu Azure Redis vahemälu üles.

    using StackExchange.Redis;

>[AZURE.NOTE] Kliendi StackExchange.Redis nõuab .NET Framework 4 või uuem versioon.

Azure'i Redis vahemälu ühendus haldab selle `ConnectionMultiplexer` klassi. See tund on mõeldud ühiskasutusse antud ja uuesti kasutada kogu klientrakenduse ja ei ole vaja luua kohta tegevuse alusel. 

Ühenduse Azure'i Redis vahemälu ja tagastada eksemplari on ühendatud `ConnectionMultiplexer`, kõne staatiline `Connect` meetod ja jõustamine vahemälu lõpp-punkti ja klahvi, nagu järgmises näites. Kasutage loodud parool parameetrina Azure'i portaal.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Hoiatus: Kunagi poe identimisteabe lähtekoodi. Selle valimi lihtsa säilitamiseks olen neid lähtekoodi näidatakse. Lisateavet selle kohta, kuidas mandaadi talletamine teemast [Kuidas rakenduse stringide ja ühenduse stringide töö][] .

Kui te ei soovi kasutada SSL-i, kas seada `ssl=false` või argument on `ssl` parameeter.

>[AZURE.NOTE] Pordi SSL on vaikimisi uue vahemälu jaoks keelatud. Juhised pordi SSL lubamise kohta leiate jaotisest [Accessi pordid](cache-configure.md#access-ports).

Ühiskasutuse üks lähenemine on `ConnectionMultiplexer` oma rakenduse eksemplari on staatiline atribuut, mis tagastab ühendatud eksemplari, sarnaselt järgmise näite. See võimaldab jutulõnga ohutu lähtestada ainult ühe ühendatud `ConnectionMultiplexer` eksemplari. Nendes näidetes `abortConnect` väärtuseks false, mis tähendab, et kõne õnnestub isegi juhul, kui ühenduse Azure'i Redis vahemälu ei asu. Üks klahv funktsioon `ConnectionMultiplexer` on, et see automaatselt taastamine ühenduvuse vahemällu üks kord võrguprobleemi või muud põhjused on lahendatud.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Täpsemad ühendussuvandid konfigureerimise kohta leiate lisateavet teemast [StackExchange.Redis konfiguratsiooni mudel][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Kui ühendus on loodud, tagastab viite redis vahemälu andmebaas, helistades selle `ConnectionMultiplexer.GetDatabase` meetod. Tagastatud objekt on `GetDatabase` meetod on kerge läbiv objekt ja ei pea olema talletatud.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Nüüd, kui teate, kuidas ühendust Azure Redis vahemälu ja tagastab viite vahemälu andmebaas, Vaatame lähemalt töötamine vahemälu.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Lisamine ja tuua objektide vahemälu

Üksusi saab salvestatud ja tuua vahemälu kaudu, kasutades funktsiooni `StringSet` ja `StringGet` meetodid.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis enamik andmete Redis stringid, kuid neid stringe võib sisaldada mitut tüüpi andmeid, sh sarjadesse jaotatud binaarandmeid, mida saab kasutada kui objektide talletamise .net-i vahemälu poed.

Kui helistate `StringGet`, kui objekt on olemas, tagastatakse, ja kui see ei sobi, `null` tagastatakse. Sel juhul saate tuua väärtus soovitud andmeallika ja selle vahemälu edaspidiseks kasutamiseks salvestada. Seda nimetatakse vahemälu-kõrvale mustri.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Üksuse aegumise vahemälu, saate määrata selle `TimeSpan` parameeter `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>.Net-i vahemälu objektidega töötamine

Azure'i Redis vahemälu saab vahemälu .NET objektide kui ka lihtsad andmetüübid, kuid enne .NET objekti saab kopeerida peab seeriasertide. See on rakenduse arendaja ülesanne ja kaudu arendaja on serializer valik.

Üks lihtne viis serialiseerida objektid on kasutada funktsiooni `JsonConvert` sariväljaanne meetodite [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) ja serialiseerida ja sealt JSON. Järgmises näites on kujutatud saamiseks ja määrata, kasutades mõnda `Employee` objekti.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid, järgige neid linke Lisateavet Azure Redis vahemälu.

-   Leiate Azure'i Redis vahemälu ASP.net-i teenusepakkuja.
    -   [Azure'i Redis seansi oleku pakkuja](cache-aspnet-session-state-provider.md)
    -   [Azure'i Redis vahemälu ASP.net-i väljundi vahemälu pakkuja](cache-aspnet-output-cache-provider.md)
-   [Luba vahemälu diagnostika](cache-how-to-monitor.md#enable-cache-diagnostics) saate vahemälu seisundi [jälgimine](cache-how-to-monitor.md) . Saate vaadata mõõdikud Azure'i portaalis ja võite ka [alla laadida ja vaadata](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) neid teie valitud tööriista abil.
-   Tutvuge [StackExchange.Redis vahemälu kliendi dokumentatsiooni][].
    -   Azure'i Redis vahemälu pääseb juurde paljud Redis kliendid ja arengu keeled. Lisateavet leiate teemast [http://redis.io/clients][].
-   Azure'i Redis vahemälu saate kasutada ka kolmanda osapoole teenuste ja muude tööriistade nagu Redsmin ja Redis Desktop Manager.
    -   Redsmin kohta leiate lisateavet teemast [Azure Redis ühendusstringi toomiseks ja kasutada seda Redsmin][].
    -   Pääsete juurde ja Azure Redis vahemälu GUI [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)abil andmete uurimine.
-   Dokumentatsioonist [redis][] ja lugege [redis andmetüübid][] ja [15 minuti tutvustus Redis andmetüüpide][]kohta.



<!-- INTRA-TOPIC LINKS -->
[Järgmised sammud]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Vahemälu loomine]: #create-cache
[Configure the cache]: #enable-caching
[Vahemälu kliendid konfigureerimine]: #NuGet
[Working with Caches]: #working-with-caches
[Ühenduse loomine vahemälu]: #connect-to-cache
[Lisamine ja tuua objektide vahemälu]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Kuidas hankida Azure'i Redis ühendusstringi ja seda kasutada koos Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[Mudeli StackExchange.Redis konfigureerimine]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Vahemälu hinnad üksikasjad]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis vahemälu kliendi dokumentatsioon]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis andmetüübid]: http://redis.io/topics/data-types
[15 minuti tutvustus Redis andmetüübid]: http://redis.io/topics/data-types-intro

[Kuidas töötavad rakenduse stringide ja ühendusstringi]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


