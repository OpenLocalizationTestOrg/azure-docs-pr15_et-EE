<properties 
    pageTitle="Vahemälu migreerimine Redis | Microsoft Azure'i"
    description="Saate teada, kuidas migreerida hallatavate vahemälu teenuserakenduste Azure'i Redis vahemällu"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Azure'i Redis vahemälu hallatavate vahemälu teenuse migreerimine

Migreerimine rakenduste Azure Redis vahemälu Azure'i hallatavate vahemälu teenuse abil on võimalik teha rakenduse, sõltuvalt teie vahemällu rakendus kasutab hallatavate vahemälu teenuse funktsioone minimaalsete muudatuste koos. Funktsiooni API-d ei ole täpselt sama nad on sarnased ja palju oma kood, mis kasutab hallatavate vahemälu teenuse juurdepääsu vahemälu saab uuesti kasutada minimaalsete muudatustega. See teema näitab, kuidas teha vajalikud konfiguratsiooni ja rakenduse muudatused migreerida rakenduste hallatavate vahemälu teenuse kasutamise Azure'i Redis vahemälu ja näitab, kuidas teatud funktsioonid Azure'i Redis vahemälu saab kasutada rakendamiseks hallatavate vahemälu teenuse vahemälu.

## <a name="migration-steps"></a>Migreerimise sammud

Järgmised toimingud on vaja migreerida hallatavate vahemälu teenuserakenduse kasutamiseks Azure'i Redis vahemälu.

-   Azure'i Redis vahemälu vastendamine hallatavate vahemälu teenuse funktsioonid
-   Vali vahemälu pakkumine
-   Vahemälu luua
-   Vahemälu kliendid konfigureerimine
    -   Eemaldage hallatavate vahemälu teenuse konfigureerimine
    -   Vahemälu klient, kasutades StackExchange.Redis Nugeti pakett konfigureerimine
-   Migreerimine hallatavate vahemälu teenuse kood
    -   Ühenduse loomine abil ConnectionMultiplexer klassi vahemälu
    -   Lihtsad Accessi andmetüübid vahemälu
    -   .Net-i vahemälu objektidega töötamine
-   ASP.net-i seansi olek ja väljundi vahemällu Azure'i Redis vahemälu migreerimine 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Azure'i Redis vahemälu vastendamine hallatavate vahemälu teenuse funktsioonid

Azure'i hallatavate vahemälu teenuse ja Azure Redis vahemälu sarnased, kuid nende funktsioonide erinevatel viisidel rakendada. Selles jaotises kirjeldatakse erinevusi ja antakse juhiseid rakendamisel Azure'i Redis vahemälu hallatavate vahemälu teenuse funktsioone.

|Hallatavate vahemälu teenuse funktsioon|Hallatavate vahemälu teenuse tugi|Azure'i Redis vahemälu tugi|
|---|---|---|
|Nimega vahemälu|Vaikimisi vahemälu on konfigureeritud ja Standard- ja Premium vahemälu saab konfigureerida pakkumisi üheksa täiendavad nimega vahemälu ülespoole, kui soovitud.|Azure'i Redis vahemälu on andmebaaside (vaikimisi 16), mida saab rakendada sarnast funktsionaalsust, et nimega vahemälu konfigureeritav arv. Lisateabe saamiseks vt [vaikimisi Redis serveri konfigureerimine](cache-configure.md#default-redis-server-configuration).|
|Kõrge-saadavus|Kõrge-saadavus ette nähtud, vahemälu Standard- ja Premium vahemälu pakkumisi. Kui üksused on kadunud tõrke tõttu, vahemälu üksuste varukoopiaid on veel saadaval. Teisene vahemälu kirjutab tehakse sünkroonselt.|Kõrge-saadavus on saadaval Standard- ja Premium vahemälu pakkumisi, mis on kaks sõlme primaarne/koopia konfiguratsiooni (iga Kildu Premium vahemälu on esmane/koopia paari). Kirjutab koopia tehakse asünkroonselt. Lisateavet leiate teemast [Azure Redis vahemälu hinnad](https://azure.microsoft.com/pricing/details/cache/).|
|Teatised|Võimaldab klientidel asünkroonne teatisi, kui erinevate vahemälu tegevuste nimega vahemälu.|Klientrakendustes saate kasutada Redis pub/sub või [Keyspace teatised](cache-configure.md#keyspace-notifications-advanced-settings) sarnast funktsionaalsust teavitamine saavutamiseks.|
|Kohaliku vahemälu|Salvestab kohalik koopia vahemällu talletatud objektide kliendi kiireks juurdepääsuks.|Klientrakendustes oleks vaja rakendada selle sõnastiku või sarnane andmestruktuur funktsionaalsus.|
|Väljatõstmine poliitika|Ükski või LRU. Vaikepoliitika on LRU.|Azure'i Redis vahemälu toetab järgmised väljatõstmine poliitikad: lenduvad-lru, allkeys-lru, lenduvad juhusliku, allkeys juhusliku, lenduvad ttl noeviction. Vaikepoliitika on lenduvad-lru. Lisateabe saamiseks vt [vaikimisi Redis serveri konfigureerimine](cache-configure.md#default-redis-server-configuration).|
|Aegumise poliitika|Vaikimisi aegumise poliitika on absoluutne ja vaikimisi aegumise intervalli on veel kümme minutit. Saadaval ka libistades ja mitte kunagi.|Vaikimisi vahemälu üksuste aegumise, kuid mõni aegumise saab konfigureerida kohta täitmine alusel, kasutades vahemälu määramine ülekoormuse. Lisateavet leiate teemast [vahemälust objektide lisamine ja too](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Piirkondade ja sildistamine|Piirkondade on alamrühmade vahemällu talletatud üksuste jaoks. Piirkondade toetavad ka marginaale vahemällu talletatud üksuste koos täiendavate kirjeldav stringide nimega sildid. Piirkonnad toetavad võimalus toiminguid otsingu sildistatud üksused selle piirkonna. Kõigi üksuste piirkonnas on ühe sõlme vahemälu kobar.|Redis vahemälu koosneb ühe sõlm (välja arvatud Redis kobar on lubatud) nii, et hallata vahemälu teenuse piirkondade mõiste ei kehti. Redis toetab otsimise ja metamärkide toimingud allalaadimisel klahvid nii siltide saab kinnistatud võtme nimed ja kasutada üksused hiljem kuulata. Näiteks rakendamise abil Redis Suhtlussiltide lahenduse, leiate teemast [rakendamise vahemälu sildistamine koos Redis](http://stackify.com/implementing-cache-tagging-redis/).|
|Sariväljaanne|Hallatavate vahemälu toetab NetDataContractSerializer, BinaryFormatter ja kohandatud sarjadesse jaotaja kasutamine. Vaikimisi on NetDataContractSerializer.|Klientrakendusega serialiseerida .NET objektide enne nende vahemälu, valik serializer kuni kliendi rakenduse arendaja vastutab. Lisateavet ja proovi kood, lugege teemat [.net-i vahemälu objektidega töötamine](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Vahemälu emulaator | Hallatavate vahemälu pakub kohaliku vahemälu emulaator. | Azure'i Redis vahemälu ei saa emulaator, kuid saate [käivitada MSOpenTech ehitada kohalikult redis server.exe](cache-faq.md#cache-emulator) emulaator rakenduseks. |

## <a name="choose-a-cache-offering"></a>Vali vahemälu pakkumine

Microsoft Azure'i Redis vahemälu on saadaval järgmised astme.

-   **Lihtne** – üks sõlm. Mitme suurused 53 GB.
-   **Standard** -kaks sõlme primaarne/koopia. Mitme suurused 53 GB. 99,9% SLA.
-   **Premium** – esmase kaks sõlme koopia kuni 10 shards abil. Mitme 6 GB 530 GB suurused (kontakt rohkem). Kõik Standard taseme funktsioonid ja veel sh tugi [Redis kobar](cache-how-to-premium-clustering.md), [Redis püsimine](cache-how-to-premium-persistence.md)ja [Azure virtuaalse võrgu](cache-how-to-premium-vnet.md). 99,9% SLA.

Iga taseme erineb nii funktsioonid ja hinnakirjad. Funktsioonide asuvad hiljem sellest juhendist ja Lisateavet hindade kohta leiate teemast [Vahemälu hinnad üksikasjad](https://azure.microsoft.com/pricing/details/cache/).

Migreerimise alguspunkt on valige suurus, mis vastab eelmise hallatavate vahemälu teenuse vahemälu suurust ja siis sõltuvalt rakenduse nõuetele skaala üles või alla. Õigete paremas Azure'i Redis vahemälu pakkumise rohkem juhised leiate teemast [mis Redis vahemälu pakkumise ja suurus peaks kasutama?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Vahemälu luua

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Vahemälu kliendid konfigureerimine

Kui vahemälu on loodud ja konfigureeritud, järgmiseks on hallatavate vahemälu teenuse konfiguratsiooni eemaldamine ja lisamine lisamine Azure'i Redis vahemälu konfiguratsiooni ja viitab, et vahemälu kliendid pääsevad vahemälu.

-   Eemaldage hallatavate vahemälu teenuse konfigureerimine
-   Vahemälu klient, kasutades StackExchange.Redis Nugeti pakett konfigureerimine

### <a name="remove-the-managed-cache-service-configuration"></a>Eemaldage hallatavate vahemälu teenuse konfigureerimine

Enne kliendi rakendused saab konfigureerida Azure Redis vahemälu, olemasoleva hallatavate vahemälu teenuse konfigureerimine ja desinstallimine hallatavate vahemälu teenuse Nugeti pakett tuleb komplekti viited eemaldada.

Hallatavate vahemälu teenuse Nugeti paketi desinstallimiseks Paremklõpsake **Solution** Exploreris kliendi projekti ja valige **Halda NuGet-paketid**. Valige sõlme **installitud paketid** ja tippige väljale Otsi installitud paketid W**indowsAzure.Caching** . Valige **Windows** **Azure'i vahemälu** (või **Windows** **Azure'i vahemällu** sõltuvalt versiooni NuGet pakett), klõpsake käsku **desinstalli**ja seejärel klõpsake nuppu **Sule**.

![Azure'i hallatavate vahemälu teenuse Nugeti pakett desinstallimine](./media/cache-migrate-to-redis/IC757666.jpg)

Desinstallimine hallatavate vahemälu teenuse Nugeti pakett eemaldab hallatavate vahemälu teenuse assemblereid ja hallatavate vahemälu teenuse kirjed app.config või web.config, klientrakendust. Kuna kohandatud sätete ei saa eemaldada, kui eemaldate Nugeti pakett, avage web.config või app.config ja veenduge, et järgmisi elemente on täielikult eemaldatud.

Tagada, et selle `dataCacheClients` eemaldatakse kirje on `configSections` element. Eemalda kogu `configSections` elemendi; lihtsalt eemaldada soovitud `dataCacheClients` kirje, kui see on olemas.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Tagada, et selle `dataCacheClients` jaotis on eemaldatud. Funktsiooni `dataCacheClients` jaotis on sarnane järgmises näites.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Kui hallatavad vahemälu teenuse konfigureerimine on eemaldatud, saate konfigureerida vahemälu kliendi järgmises jaotises kirjeldatud.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Vahemälu klient, kasutades StackExchange.Redis Nugeti pakett konfigureerimine

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migreerimine hallatavate vahemälu teenuse kood

Hallatavate vahemälu teenuse sarnaneb API StackExchange.Redis vahemälu kliendi jaoks. Selles jaotises antakse ülevaade erinevused.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Ühenduse loomine abil ConnectionMultiplexer klassi vahemälu

Hallatavate vahemälu teenus, vahemälu ühenduste käsitleja oli selle `DataCacheFactory` ja `DataCache` tunnid. Azure'i Redis vahemälu haldab need ühendused soovitud `ConnectionMultiplexer` klassi.

Lisage järgmine kasutamine lause mis tahes faili, kust soovite juurdepääsu vahemälu üles.

    using StackExchange.Redis
                                
Kui selle nimeruumi ei lahenda, olla kindel, et olete lisanud StackExchange.Redis Nugeti pakett [konfigureerimine vahemälu kliendid](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)kirjeldatud.

>[AZURE.NOTE] Pange tähele, et kliendi StackExchange.Redis .NET Framework 4 või uuem versioon.

Azure'i Redis vahemälu näiteks ühendamiseks kõne staatiline `ConnectionMultiplexer.Connect` meetod ja pass lõpp-punkti ja võti. Ühiskasutuse üks lähenemine on `ConnectionMultiplexer` oma rakenduse eksemplari on staatiline atribuut, mis tagastab ühendatud eksemplari, sarnaselt järgmise näite. See võimaldab jutulõnga ohutu lähtestada ainult ühe ühendatud `ConnectionMultiplexer` eksemplari. Selles näites `abortConnect` väärtuseks false, mis tähendab, et kõne õnnestub isegi juhul, kui ühendus vahemälu ei asu. Üks klahv funktsioon `ConnectionMultiplexer` on, et see automaatselt taastamine ühenduvuse vahemällu üks kord võrguprobleemi või muud põhjused on lahendatud.

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

Vahemälu lõpp-punkti, võtmed ja pordid saab keelest **Redis vahemälu** vahemälu eksemplari puhul. Lisateavet leiate teemast [Redis vahemälu atribuudid](cache-configure.md#properties).

Kui ühendus on loodud, tagastab viite Redis vahemälu andmebaasi helistades selle `ConnectionMultiplexer.GetDatabase` meetod. Tagastatud objekt on `GetDatabase` meetod on kerge läbiv objekt ja ei pea olema talletatud.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

StackExchange.Redis klient kasutab funktsiooni `RedisKey` ja `RedisValue` tüüpi juurdepääsuks ja üksuse vahemällu talletamine. Järgmist tüüpi peale kõige lihtsad keele tüübid, sh stringi, vastendamine ja sageli ei kasutata otse. Redis stringide on kõige olulisemaid tüüpi Redis väärtust ja võib sisaldada mitut tüüpi andmeid, sealhulgas sarjadesse jaotatud kahendarvu, ja kuigi te ei tohi kasutada tüüp otse, kasutate meetodid, mis sisaldavad `String` nimi. Kõige lihtsad andmetüüpide talletada ja tuua üksused vahemälu abil soovitud `StringSet` ja `StringGet` meetodid, kui talletate saidikogumid või muid Redis andmete vahemälu. 

`StringSet`ja `StringGet` on väga sarnane hallatavate vahemälu teenuse `Put` ja `Get` meetodid, üks põhi erinevus on see, et enne seadmine ja saada .NET objekti vahemälu peate selle esmalt serialiseerida. 

Kui helistate `StringGet`, kui objekt on olemas, tagastatakse ja seda ei ole, tagastatakse tühiväärtus. Sel juhul saate tuua väärtus soovitud andmeallika ja selle vahemälu edaspidiseks kasutamiseks salvestada. Seda nimetatakse vahemälu-kõrvale mustri.

Üksuse aegumise vahemälu, saate määrata selle `TimeSpan` parameeter `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure'i Redis vahemälu saate töötada .NET objektide kui ka lihtsad andmetüüpe, kuid enne .NET objekti saab kopeerida peab seeriasertide. See on rakenduse arendaja. See võimaldab paindlikult arendaja on serializer valik. Lisateavet ja proovi kood, lugege teemat [.net-i vahemälu objektidega töötamine](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>ASP.net-i seansi olek ja väljundi vahemällu Azure'i Redis vahemälu migreerimine

Azure'i Redis vahemälu on nii ASP.net-i seansi olek kui ja lehe väljundi vahemällu pakkujad. Teie rakendus, mis kasutab hallatavate vahemälu teenuse versioonide loetletud teenusepakkuja migreerimiseks esmalt eemaldamine teie web.config olemasoleva jaotised ja seejärel konfigureerige Azure'i Redis vahemälu versioonide pakkujatest. Azure'i Redis vahemälu ASP.net-i teenusepakkuja kasutamise kohta, leiate [Azure'i Redis vahemälu ASP.net-i seansi olek pakkuja](cache-aspnet-session-state-provider.md) ja [ASP.net-i väljundi vahemälu pakkuja Azure'i Redis vahemälu](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Järgmised sammud

Tutvuge [Azure'i Redis vahemälu dokumentatsiooni](https://azure.microsoft.com/documentation/services/cache/) õpetused, näidised, videoid ja muu jaoks.

