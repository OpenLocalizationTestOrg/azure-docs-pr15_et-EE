<properties 
    pageTitle="Azure'i Redis vahemälu KKK | Microsoft Azure'i" 
    description="Siin on vastused levinud küsimustele, mustrite ja heade tavade Azure'i Redis vahemälu" 
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
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure'i Redis vahemälu KKK

Siin on vastused levinud küsimustele, mustrite ja heade tavade Azure'i Redis vahemälu. 


## <a name="what-if-my-question-isnt-answered-here"></a>Mida teha, kui mu küsimusele pole siin vastata?

Kui küsimus pole siin loetletud, andke meile teada ja aitame teil leida vastuseid.

-   Saate Postitage küsimus [Disqus teemat](#comments) korduma Kippuvate lõpus ja meeskonnatöö Azure'i vahemälu ja muude ühenduse liikmete kohta käesoleva artikli.
-   Laiemalt saavutamiseks saate Postitage küsimus [Azure'i vahemälu MSDN-i Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) ja Azure vahemälu meeskonnatöö ja teiste ühenduse suhelda.
-   Kui soovite teha funktsiooni taotluse, saate esitada oma taotlusi ja [Azure Redis vahemälu kasutaja kõneposti](https://feedback.azure.com/forums/169382-cache)ideid.
-   Lisaks saate e-posti saata veebisaidil [Azure'i vahemälu välise tagasiside](mailto:azurecache@microsoft.com)meile.

## <a name="azure-redis-cache-basics"></a>Azure'i Redis vahemälu põhialused

Selles jaotises KKK katta osa põhitõdesid Azure'i Redis vahemälu.

-    [Mis on Azure Redis vahemälu?](#what-is-azure-redis-cache)
-    [Kuidas saab alustada Azure'i Redis vahemälu?](#how-can-i-get-started-with-azure-redis-cache)

Järgmised KKK põhimõtted ja Azure Redis vahemälu küsimused ja vastused on üks korduma kippuvaid.

-   [Kuidas Redis vahemälu pakkumise ja suurus kasutada?](#what-redis-cache-offering-and-size-should-i-use)
-   [Millist Redis vahemälu kliendid saavad kasutada?](#what-redis-cache-clients-can-i-use)
-   [Kas kohalik emulaator Azure'i Redis vahemälu jaoks?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Kuidas jälgida seisundi ja minu vahemälu jõudlust?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Kavandamise KKK

-   [Kuidas Redis vahemälu pakkumise ja suurus kasutada?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure'i Redis vahemälu jõudlus](#azure-redis-cache-performance)
-   [Piirkond, mis peaks minu vahemälu leida?](#in-what-region-should-i-locate-my-cache)
-   [Kuidas maksmine Azure'i Redis vahemälu jaoks?](#how-am-i-billed-for-azure-redis-cache)
-   [Saate kasutada Azure Redis vahemälu Azure'i Government Cloud või Azure Hiina pilvepõhises?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Arengu KKK

-   [Mida teha StackExchange.Redis konfiguratsiooni suvandid?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Millist Redis vahemälu kliendid saavad kasutada?](#what-redis-cache-clients-can-i-use)
-   [Kas kohalik emulaator Azure'i Redis vahemälu jaoks?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Kuidas ma saan Redis käsud?](#how-can-i-run-redis-commands)
-   [Miks pole Azure'i Redis vahemälu MSDN-i klassi teegi teatmematerjalid nagu mõned Azure teenused?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Azure'i Redis vahemälu saab kasutada PHP seansi vahemälu?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Turvalisuse KKK

-   [Millal tuleks lubada pordi-SSL-ühenduse Redis?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Tootmise KKK

-   [Mis on tootmise parimaid tavasid?](#what-are-some-production-best-practices)
-   [Mis on mõned kaalutlused levinud Redis käskude kasutamisel?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Kuidas võrrelda ja testida minu vahemälu?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Olulisi üksikasju ThreadPool growth](#important-details-about-threadpool-growth)
-   [Luba server GC saada lisateavet läbilaskevõime kliendi StackExchange.Redis kasutamisel](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Jälgimine ja tõrkeotsingu KKK

Selles jaotises KKK katta levinud jälgimine ja tõrkeotsingu küsimused. Jälgimine ja Azure Redis vahemälu eksemplaride tõrkeotsingu kohta leiate lisateavet teemast [Kuidas jälgida Azure'i Redis vahemälu](cache-how-to-monitor.md) ja [Azure Redis vahemälu tõrkeotsing](cache-how-to-troubleshoot.md).

-   [Kuidas jälgida seisundi ja minu vahemälu jõudlust?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Oma vahemälu diagnostika salvestusruumi sätteid muutnud, mis on juhtunud?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Miks on mõned uue vahemälu, kuid mitte teiste jaoks lubatud diagnostika?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Miks ma näen ajalõpud?](#why-am-i-seeing-timeouts)
-   [Miks minu klient katkestati vahemälu?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Eelnevate vahemälu pakkumise KKK

-   [Millised Azure'i vahemälu pakkumine on mulle sobivaim?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Mis on Azure Redis vahemälu?

Azure'i Redis vahemälu põhineb populaarsed avatud lähtekoodi [Redis vahemälu](http://redis.io). See annab teile juurdepääsu turvaline, sihtotstarbeline Redis vahemälu, Microsoft hallatavate ja mis tahes rakenduses Azure'is kaudu juurdepääsetavad. Üksikasjalikuma ülevaate leiate [Azure'i Redis vahemälu](https://azure.microsoft.com/services/cache/) toote lehe näidata Azure.com.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Kuidas saab alustada Azure'i Redis vahemälu?

On mitu võimalust, saate alustada Azure'i Redis vahemälu.

-    Saate vaadata ühte meie õpetused saadaval [.net-i](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.net-i](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)ja [Python](cache-python-get-started.md). 
-    [Kuidas koostada kõrge jõudluse rakenduste abil Microsoft Azure'i Redis vahemälu](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/)saab vaadata.
-    Saate vaadata projekti näha, kuidas kasutada Redis keelele arengu klientides kliendi dokumentatsioonist. On palju Redis kliendid, mida saab kasutada Azure Redis vahemälu. Redis klientide loendi leiate teemast [http://redis.io/clients](http://redis.io/clients).


Kui te pole veel Azure'i konto, saate teha järgmist.

-    [Avage Azure'i konto tasuta](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Saate proovida makstud Azure'i teenuste kasutatavate krediidi summa liitmisel. Isegi juhul, kui tegu on kasutanud, saate selle konto ja tasuta Azure teenuste ja funktsioonide kasutamine.
-    [Aktiveerige Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN-i tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Kuidas Redis vahemälu pakkumise ja suurus kasutada?
Iga Azure'i Redis vahemälu pakub erineva **suuruse**, **läbilaskevõime**, **kõrge-saadavus**ja **SLA** suvandid.

Kaalutlused valimise vahemälu pakkumise on järgmised.

-   **Mälu**: The põhi- ja Standard astme pakkuda 250 MB – 53 GB. Premium taseme pakub veel saadaval [taotluse](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)koos 530 GB. Lisateavet leiate teemast [Azure Redis vahemälu hinnad](https://azure.microsoft.com/pricing/details/cache/).
-   **Võrgu jõudluse**: kui teil on töökoormus, mis nõuab kõrge läbilaskevõime Premium taseme pakub rohkem ribalaiust võrreldes Standard- või Basic. Iga taseme sees on suurem suurused vahemälu ka rohkem läbilaskevõime tõttu aluseks oleva VM, mis hostib vahemälu. Lugege lisateavet [järgmises tabelis](#cache-performance) .
-   **Läbilaskevõime**: The Premium taseme pakub maksimaalne saadaval läbilaskevõime. Kui vahemälu server või kliendi jõuab läbilaskevõime piirangute, saate ajalõpud kliendis. Lisateavet leiate teemast järgmises tabelis.
-   **Kõrge kättesaadavus/SLA**: Azure'i Redis vahemälu tagab Standard/Premium vahemälu saadaval vähemalt 99,9% ajast. Meie SLA kohta leiate lisateavet teemast [Azure Redis vahemälu hinnad](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). SLA katab ainult Ühenduvus vahemälu lõpp-punktid. SLA ei hõlma kaitse andmekao. Soovitame kasutada funktsiooni Redis andmete püsimine Premium astme suurendamiseks paindlikkust andmekao vastu.
-   **Redis andmete püsimine**: The Premium taseme võimaldab Azure Storage konto vahemälu andmed säilivad. Basic/Standard vahemälust kõik andmed on salvestatud ainult mälu. Aluseks oleva taristu korral võib probleeme seal andmekadu. Soovitame kasutada funktsiooni Redis andmete püsimine Premium astme suurendamiseks paindlikkust andmete kaotsimineku vastu. Azure'i Redis vahemälu pakub RDB ja Ofensiva # (tulekul) suvandid Redis püsimine. Lisateabe saamiseks vaadake, [Kuidas konfigureerida püsimine Premium Azure'i Redis vahemälu](cache-how-to-premium-persistence.md).
-   **Redis kobar**: loomiseks vahemälu suurem kui 53 GB või Kildu andmed üle mitme Redis sõlmed, võite kasutada Redis rühmitamise, mis on saadaval Premium astme. Iga sõlme koosneb paari primaarne/koopia vahemälu kõrge kättesaadavus. Lisateabe saamiseks vaadake, [Kuidas konfigureerida rühmitamise Premium Azure'i Redis vahemälu](cache-how-to-premium-clustering.md).
-   **Täiustatud turvalisus ja võrgu eraldamise**: Azure'i virtuaalse võrgu (VNET) juurutamise pakub tõhustatud turvalisus ja Azure Redis vahemälu, samuti alamvõrku, juhtelemendi juurdepääsupoliitikaid eraldamise ja muude funktsioonide veelgi piirata juurdepääsu. Lisateabe saamiseks vaadake, [Kuidas konfigureerida virtuaalse võrgu tugi Premium Azure'i Redis vahemälu](cache-how-to-premium-vnet.md).
-   **Konfigureerimine Redis**: nii Standard- ja Premium astme, saate konfigureerida Redis Keyspace teatiste saamiseks.
-   **Suurim arv kliendi ühendused**: The Premium taseme pakub kliendid, mida saate luua ühenduse Redis suurema arvu ühenduste jaoks suurema suurusega vahemälu maksimaalne arv. [Palun vaadake hinnakirjad lehelt](https://azure.microsoft.com/pricing/details/cache/).
-   **Sihtotstarbeline Core Redis server**: Premium astme kõik vahemälu suurused on spetsiaalne core Redis. Basic/Standard astme C1 suuruse ja on spetsiaalne core Redis server.
-   **Redis on ühelõimelised** nii on rohkem kui kaks tuuma ei paku täiendavat üle võttes vaid kahe tuuma, kuid suuremad VM suurused on tavaliselt väiksemad kui rohkem ribalaiust. Kui vahemälu server või kliendi jõuab läbilaskevõime piirangute, siis kuvatakse ajalõpud kliendis.
-   **Jõudlustäiustused**: riistvara, mis on kiirem protsessorit on juurutatud Premium astme vahemälu ja annab parema jõudluse võrreldes Basic või Standard taseme. Premium taseme vahemälu on suurem läbilaskevõime ja alumise latentsused.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure'i Redis vahemälu jõudlus

Järgmine tabel näitab samas katsetamine erineva suurusega standardi maksimaalse läbilaskevõime väärtused ja Premium salvestab, kasutades `redis-benchmark.exe` on Iaas VM Azure Redis vahemälu lõpp-punkti suhtes. Pange tähele, et need väärtused ei ole tagatud on puudub SLA nende arvud, kuid mul peaks olema tüüpilised. Peaks teil laadida testige õige vahemälu suuruse rakenduse rakendust.

Selle tabeli saame teha järgmised järeldusi.

-   Läbilaskevõime jaoks vahemälu, mis on sama suurus on suurem Premium taseme võrreldes Standard taseme. Näiteks 6 GB vahemälu, on läbilaskevõime P1 140K RPS võrreldes 49 K C3.
-   Redis rühmitamise, mis suurendab läbilaskevõime lineaarses shards (sõlmed) klaster arvu suurendamisel. Näiteks kui loote P4 kobar, 10 shards, siis saadaval läbilaskevõime on 250K * 10 = 2,5 miljonit RPS.
-   Läbilaskevõime suurem võti suurused on suurem Premium taseme võrreldes Standard taseme.

| Esimese taseme hinnad             | Suurus   | Protsessorituuma | Saadaval läbilaskevõime.                                    | 1 KB võti suurus                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Standardse vahemälu suurus** |        |           | **Sec (Mb/s) megabitti / megabaiti (MB/s) sec kohta** | **Taotlusi sekundis (RPS)**            |
| C0                       | 250 MB | Ühiskasutusse antud    | 5 / 0,625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2,5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Premium vahemälu suurus**  |        | **Protsessorituuma Kildu kohta.**  |                                         | **Taotlusi kohta Kildu sekundis (RPS)** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Juhised allalaadimine Redis tööriistu, nagu `redis-benchmark.exe`, leiate selle [Kuidas saab käitada Redis käsud?](#cache-commands) jaotis.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>Piirkond, mis peaks minu vahemälu leida?

Parima jõudluse ja kõige väiksemad latentsus, otsige üles Azure Redis vahemälu vahemälu klientrakenduse sama piirkonna.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Kuidas maksmine Azure'i Redis vahemälu jaoks?

Azure'i Redis vahemälu hinnad on [siin](https://azure.microsoft.com/pricing/details/cache/). Hinnakirjad lehel kuvatakse, kui tunni hinnad. Vahemälu eest tuleb tasuda minutis alusel loodud vahemälu kuni vahemälu kustutatud aeg. Puudub võimalus peatamine või pausi vahemälu arveldamine.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Saate kasutada Azure Redis vahemälu Azure'i Government Cloud või Azure Hiina pilvepõhises?

Jah, Azure'i Redis vahemälu on saadaval nii Azure'i Government Cloud ja Azure Hiina pilve. Pange tähele, et juurdepääs ja haldamise Azure'i Redis vahemälu URL-ide erinevad Azure Governmenti pilveteenuses Azure'i Hiina Cloud võrreldes Azure'i avaliku pilve. Azure'i Government Cloud ja Azure Hiina pilve Azure Redis vahemälu kasutamisel peaksite arvesse võtma kohta leiate lisateavet teemast [Azure Governmenti andmebaasid - Azure'i Redis vahemälu](../azure-government/documentation-government-services-database.md#azure-redis-cache) ja [Azure Hiina pilve - Azure'i Redis vahemälu](https://www.azure.cn/documentation/services/redis-cache/).

PowerShelliga Azure'i Government Cloud ja Azure Hiina pilve Azure Redis vahemälu kasutamise kohta leiate teavet teemast [Azure Government Cloud või Azure Hiina pilvepõhises ühendamiseks](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Mida teha StackExchange.Redis konfiguratsiooni suvandid?

StackExchange.Redis on palju võimalusi. Selles jaotises räägib mõned levinud sätted. Üksikasjalikumat teavet StackExchange.Redis suvandite kohta leiate teemast [StackExchange.Redis konfigureerimine](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Kirjeldus|Soovitused
---|---|---
AbortOnConnectFail|Kui on seatud tõene, ühendust ei taastada pärast võrgu tõrke.|Väärtuseks false ja andke StackExchange.Redis automaatselt.
ConnectRetry|Ühendage korduste ajal algne ühenduse katsete arv.| Vt järgmised juhised. |
ConnectTimeout|Ajalõpp MS jaoks luua ühenduse toimingud.| Vt järgmised juhised. |

Enamikul juhtudel kliendi vaikeväärtused on piisavalt. Saate häälestada suvandid vastavalt oma töökoormus.

-   **Korduskatsed**
    -   Üldised juhised on ConnectRetry ja ConnectTimeout ei suuda kiirelt ja proovige uuesti. See põhineb oma töökoormus ja kui palju aega Keskmine see võtab oma kliendi anda Redis käsku ja saate vastuse.
    -   Lubage StackExchange.Redis asemel ühenduse oleku kontrollimine ja taastamine ennast automaatselt uuesti. **Vältige ConnectionMultiplexer.IsConnected atribuuti**.
    -   Snowballing - mõnikord võivad tekkida probleemi, kus teil on proovitakse uuesti ja see lumepalle ja kunagi taastab. Sel juhul kaaluge abil algoritmi eksponentsiaalse backoff uuesti, nagu on kirjeldatud [üldised juhised uuesti](best-practices-retry-general.md) avaldatud jaotises Microsoft Patterns ja tavade.
-   **Ajalõpp väärtused**
    -   Kaaluge oma töökoormus ja seada vastavalt väärtused. Kui talletate suured väärtused, määrake aeg, mille suurem väärtus.
    -   Määrake `AbortOnConnectFail` false ja lasta StackExchange.Redis ühendus teie eest.
    -   Kasutage rakenduse ConnectionMultiplexer ühekordsest. Mõne LazyConnection abil saate luua ühe eksemplari, mis tagastatakse ühenduse atribuudi, nagu on näidatud [ühenduse abil ConnectionMultiplexer klassi vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Määrake soovitud `ConnectionMultiplexer.ClientName` rakenduse eksemplari kordumatu nimi ajutised atribuut.
    -   Kasutage mitme `ConnectionMultiplexer` eksemplari kohandatud töökoormus.
    -   Kui teil on rakenduse erineva laadi tehke see mudel. Näiteks:
    -   Saate määrata ühe suure klahvid tegelemiseks multiplekseri.
    -   Saate määrata ühte multiplekseri small klahvid tegelemiseks.
    -   Saate määrata erinevaid väärtusi ühenduse ajalõpud ja uuestiproovimise loogikale jaoks iga ConnectionMultiplexer, mida kasutate.
    -   Määrake soovitud `ClientName` atribuudi iga multiplekseri diagnostika abi.
    -   Lisateavet viia sujuvam latentsus kohta `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Millist Redis vahemälu kliendid saavad kasutada?

Üks hea asju Redis on paljud kliendid toetavad paljude erinevate arengu keeled. Praeguse klientide loendi leiate teemast [Redis kliendid](http://redis.io/clients). Õpetused, mis hõlmavad mitmeid erinevaid keeli ja kliendid, vaadake, [Kuidas kasutada Azure Redis vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md) ja klõpsake selle artikli ülaosas Keelevahetaja loendist soovitud keel.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Kas kohalik emulaator Azure'i Redis vahemälu jaoks?

Ei ole kohaliku emulaator Azure'i Redis vahemälu, kuid saate redis-server.exe MSOpenTech versiooni käitamiseks [Redis käsurea](https://github.com/MSOpenTech/redis/releases/) kaudu oma kohalikus arvutis ja ühendage see sarnaseid probleeme põhjustada avamiseks kohaliku vahemälu emulaator, nagu on näidatud järgmises näites.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Soovi korral saate konfigureerida [redis.conf](http://redis.io/topics/config) vastavaks täpsemalt [vaikesätted ja vahemälu](cache-configure.md#default-redis-server-configuration) teie võrgus Azure'i Redis vahemälu kui soovitud faili.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Kuidas ma saan Redis käsud?

Saate kasutada käske, välja arvatud [Redis ei toeta Azure Redis vahemälu käskude](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)loendis käskude [Redis käskude](http://redis.io/commands#) loendis. Redis käskude on teil mitu võimalust.

-   Kui teil on Standard või Premium vahemälu, saate käivitada Redis käsud [Redis konsooli](cache-configure.md#redis-console)abil. See pakub turvaliselt Redis käskude käivitamiseks Azure'i portaalis.
-   Saate kasutada ka Redis käsurea tööriistu. Neid kasutada, tehke järgmist.
-   Laadige [Redis käsurea tööriistu](https://github.com/MSOpenTech/redis/releases/).
-   Ühenduse loomine vahemälu abil `redis-cli.exe`. Liigu vahemälu lõpp-punkti abil soovitud -h aktiveerimine ja kasutamine-, nagu on näidatud järgmises näites võti.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Teate, et Redis käsurea tööriistad ei tööta SSL-i pordiga, kuid saate kasutada näiteks kasuliku `stunnel` turvaliselt ühendada tööriistade SSL-i pordi [teatavaks ASP.net-i seansi olek pakkuja Redis eelvaateversiooniga](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) ajaveebipostituse juhiseid järgides.

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Miks pole Azure'i Redis vahemälu MSDN-i klassi teegi teatmematerjalid nagu mõned Azure teenused?

Microsoft Azure'i Redis vahemälu põhineb populaarsed avatud allika Redis vahemälu ja pääseb erinevaid [Redis kliendid](http://redis.io/clients) , mis on saadaval palju programmeerimise keelte jaoks. Iga kliendi on oma API, mis toodab kõned Redis vahemälu eksemplari [Redis käskude](http://redis.io/commands)abil.

Kuna iga kliendi erineb, on pole ühe keskse klassi viite MSDN-i; selle asemel iga kliendi säilitab oma dokumentides. Viide dokumentatsiooni kohta, Lisaks on mitu õpetused näitab, kuidas alustada Azure'i Redis vahemälu kasutamine erinevates keeltes ja vahemälu kliendid. Juurde pääseda olümpiaandmed, vaadake, [Kuidas kasutada Azure Redis vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md) ja klõpsake selle artikli ülaosas Keelevahetaja loendist soovitud keel.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Azure'i Redis vahemälu saab kasutada PHP seansi vahemälu?

Jah, kasutamiseks PHP seansi vahemälu Azure'i Redis vahemälu määrata ühendusstring Azure'i Redis vahemälu eksemplariga `session.save_path`.

>[AZURE.IMPORTANT] Azure'i Redis vahemälu PHP seansi vahemälu kasutamisel tuleb teil URL-i kodeerida turvakood ühendamiseks vahemälufailid, nagu on näidatud järgmises näites.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Kui võti pole URL-ina kodeeritav, võidakse kuvada järgmine erand:`Failed to parse session.save_path`

PHP seansi vahemälu PhpRedis kliendiga Redis vahemälu kasutamise kohta leiate lisateavet teemast [PHP seansi sündmuseohjuri](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Millal tuleks lubada pordi-SSL-ühenduse Redis?

Redis server ei toeta SSL-i välja kasti, kuid ei Azure'i Redis vahemälu. Kui loote Azure'i Redis vahemälu ja teie klient toetab SSL-i, nt StackExchange.Redis, peaksite kasutama SSL-i.

Võtke arvesse, et pordi SSL keelatakse vaikimisi Azure'i Redis vahemälu uued eksemplarid. Kui teie klient ei toeta SSL-i, siis tuleb lubada pordi-SSL- [Accessi pordid](cache-configure.md#access-ports) jaotises [konfigureerimine Azure'i Redis vahemälu vahemälu](cache-configure.md) artikli juhiste järgi.

Redis tööriistad nagu `redis-cli` SSL Port ei tööta, kuid saate kasutada näiteks kasuliku `stunnel` turvaliselt ühendada tööriistade SSL-i pordi [teatavaks ASP.net-i seansi olek pakkuja Redis eelvaateversiooniga](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) ajaveebipostituse juhiseid järgides.

Juhised allalaadimine Redis tööriistu, vt selle [Kuidas saab käitada Redis käsud?](#cache-commands) jaotis.



### <a name="what-are-some-production-best-practices"></a>Mis on tootmise parimaid tavasid?

-   [StackExchange.Redis head tavad](#stackexchangeredis-best-practices)
-   [Konfiguratsiooni ja mõisted](#configuration-and-concepts)
-   [Jõudluse testimine](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis head tavad

-   Määrake `AbortConnect` FALSE, siis lasta ConnectionMultiplexer, automaatselt. [Üksikasju vt siit](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Uuesti kasutada funktsiooni ConnectionMultiplexer - ei saa luua uus iga taotlus. Funktsiooni `Lazy<ConnectionMultiplexer>` mustri [joonisel](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) on soovitatav.
-   Redis toimib kõige paremini väiksemate väärtustega, seega kaalu tükeldamine üles suurem andmeid mitme võtmed. [See Redis arutelu](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), käsitletakse "suur" 100 kb. Lugege [artiklit](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) näide probleem, mis võib olla põhjustatud suured väärtused.
-   Konfigureerige [ThreadPool sätteid](#important-details-about-threadpool-growth) nii, et ajalõppe vältida.
-   Kasutage vähemalt vaikimisi connectTimeout 5 sekundit. See ei annaks StackExchange.Redis piisavalt aega uuesti luua ühendus, laik võrgu korral.
-   Arvestage erinevaid toiminguid, mida teil on seotud jõudlusega kulud. Näiteks kuvatakse `KEYS` käsk on toiming O(n) ja vältida. [Redis.io sait](http://redis.io/commands/) on ümber aja keerukuse iga toimingu, mis toetab üksikasjad. Klõpsake iga keerukus iga toimingu kuvamiseks.

#### <a name="configuration-and-concepts"></a>Konfiguratsiooni ja mõisted

-   Standard- või Premium taseme kasutamiseks tootmissüsteemide. Tavaline astme on ühe sõlm süsteem pole andmete kopeerimine ja puudub SLA. Kasutada ka vähemalt C1 vahemälu. C0 vahemälu on mõeldud väga lihtne arendaja katse stsenaariumid.
-   Pidage meeles, et Redis on hoida **- Mälu** Date. Lugege [seda artiklit](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , et olete teadlikud stsenaariumid, kus Andmekao võivad tekkida.
-   Välja teie süsteemi nii, et seda võib käsitleda ühendus blips [lappimine ja Tõrkesiirde tõttu](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Jõudluse testimine

-   Käivitage `redis-benchmark.exe` teada saamiseks võimalik läbilaskevõime enne oma täiuslik testinud. Pange tähele, et redis-võrdlusalus ei toeta SSL-i, nii on [mitte-SSL-pordi Azure portaali kaudu](cache-configure.md#access-ports) enne käivitate test. Näited [kuidas saate võrrelda ja testida minu vahemälu?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   Kliendi VM testimiseks peaks olema sama piirkonna oma Redis vahemälu eksemplari.
-   Soovitame kasutada Dv2 VM sarja oma kliendi, nagu need on parem riistvara ja parima tulemuse annab.
-   Veenduge, et teie kliendi VM valite vähemalt palju arvutuste ja läbilaskevõime võimalus nimega vahemälu on testimine. 
-   Kui teil on Windows lubamiseks VRSS klientarvutis. [Üksikasju vt siit](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium taseme Redis eksemplarid on on parem võrgu latentsus ja läbilaskevõime kuna need töötavad parem riistvara nii CPU ja võrgu jaoks.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Mis on mõned kaalutlused levinud Redis käskude kasutamisel?

-   Ei tohi käivitada käske Redis, mis pikka aega lõpuleviimiseks ilma väljaloendisse need käsud.
    -   Näiteks ei tööta [klahvid](http://redis.io/commands/keys) käsk valmistamisel nagu see võib võtta kaua aega, et tagastada sõltuvalt klahvide arv. Redis on ühelõimelised server ja töötleb käsud ühe korraga. Kui teil on muid käske välja pärast klahvid, neid ei töödelda kuni Redis töötleb käsk võtmed. [Redis.io sait](http://redis.io/commands/) on ümber aja keerukuse iga toimingu, mis toetab üksikasjad. Klõpsake iga keerukus iga toimingu kuvamiseks.
-   Võtme suurused - kasutada väike võti/väärtuste või suur võti/väärtused? Üldiselt sõltub seda stsenaariumi. Kui stsenaariumist nõuab suuremat klahvid seejärel saate reguleerida soovitud ConnectionTimeout ja proovige väärtused ja kohandada oma proovi uuesti loogikat. Redis serveri seisukohalt järgimine parema jõudluse olema väiksemad väärtused.
-   See ei tähenda, et te ei saa salvestada suuremate väärtuste Redis; teil tuleb arvestada järgmisega. Latentsused on suurem. Kui teil on andmed, mis on suurem kui üks, mis on väiksem, saate kasutada ConnectionMultiplexer mitmes eksemplaris, iga konfigureeritud teistsuguseid ajalõpp ja proovige uuesti väärtused, [mida StackExchange.Redis konfiguratsiooni võimalusi teha](#cache-configuration) eelmises jaotises kirjeldatud.



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Kuidas võrrelda ja testida minu vahemälu?

-   [Luba vahemälu diagnostika](cache-how-to-monitor.md#enable-cache-diagnostics) saate vahemälu seisundi [jälgimine](cache-how-to-monitor.md) . Saate vaadata mõõdikud Azure'i portaalis ja võite ka [alla laadida ja vaadata](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) neid teie valitud tööriista abil.
-   Saate redis-benchmark.exe laadimiseks testi Redis serverisse.
-   Veenduge, et kliendi ja Redis vahemälu laadi oleks sama piirkonna.
-   Kasutage redis-cli.exe ja jälgida vahemälu, kasutades käsku teave.
-   Kui teie laadi põhjustab kõrge mälu killustumise siis tuleks skaalal vahemälu suuremaks.
-   Juhised allalaadimine Redis tööriistu, vt selle [Kuidas saab käitada Redis käsud?](#cache-commands) jaotis.

Järgmises näites kasutamise redis-benchmark.exe. Täpne tulemuste saamiseks käivitage see käsk vahemälu sama piirkonna VM.

-   Test Pipelined määramine nõuab 1 k last abil

    redis-benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t seadmine -n 1000000 - d 1024 - P 50
    
-   Test Pipelined saada taotleb 1 k last abil. 
    Märkus: Käivitage määramine testida esmalt vahemälu asustamiseks eespool näidatud
    
    redis-benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t saada -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Olulisi üksikasju ThreadPool growth

CLR-i ThreadPool on kahte tüüpi Teemad - "Töötaja" ja "I/o lõpetamise Port" (ehk IOCP) Teemad. 

-   Töötaja teemad kasutatakse kui töötlemine, näiteks `Task.Run(…)` või `ThreadPool.QueueUserWorkItem(…)` viise. Järgmiste teemade kasutatakse ka erinevad komponendid on CLR-i, kui töö vajab juhtuda tausta teemat.
-   IOCP Teemad kasutatakse asünkroonne IO juhtub (nt lugemine võrgust). 

Jutulõnga pool pakub uue töötaja teemad või I/O lõpetamise Teemad nõudmisel (ilma mis tahes pidurdamise) seni, kuni see jõuab igat tüüpi jutulõnga sätet "Miinimum". Vaikimisi on seatud väikseima arvu Teemad süsteemi protsessorite arvu. 

Pärast olemasoleva (hõivatud) arvu Teemad tabamust teemad, on ThreadPool "miinimum" arv on serveripoolse ahendamise määra, mis on uut teemad, et ühe teema kohta 500 millisekundit. See tähendab, et teie süsteemi saab automaatselt mõne IOCP teemat töö plahvatuse, töödelda töötavad väga kiiresti. Aga kui töö purunemiskatse on rohkem kui konfigureeritud "Miinimum" säte, seal on mõned viivitus töötlemist osa tööd on ThreadPool ootab ühte kahest järgmisest juhtub.

1. Mõne olemasoleva jutulõnga muutub tasuta töö töötlemine.
1. Pole olemasoleva jutulõnga muutub tasuta 500 ms, luuakse uus teema.

Põhimõtteliselt, tähendab see, et kui hõivatud Teemad arv on suurem kui Min teemad, mida tõenäoliselt maksavad 500 ms viivituse enne võrguliiklust töötleb rakendus. Samuti on tähtis, et kui mõne olemasoleva jutulõnga jääb rohkem kui 15 minutit (mida ma mäleta põhjal) jõude, seda puhastada ja see tsükkel kasv ja kahanemine saate korrata.

Kui vaatame näites kuvatakse tõrketeade, alates StackExchange.Redis (koostada 1.0.450 või uuem versioon), kuvatakse see nüüd prindib ThreadPool statistika (IOCP ja töötaja üksikasju vt allpool).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Eeltoodud näites näete, et IOCP teemat on hõivatud 6 teemad ja süsteem on konfigureeritud lubama 4 minimaalne teemad. Sel juhul kliendi oleks tõenäoliselt näinud kaks 500 ms viivitused kuna 6 > 4.

Pange tähele, et StackExchange.Redis tabab ajalõpud kui kasvu IOCP või töötaja teemad saab rakendus.

### <a name="recommendation"></a>Soovitused

Antud seda teavet, me soovitame, et kliendid määratud IOCP ja töötaja teemad minimaalne konfiguratsioon väärtus suurem vaikeväärtus. Me ei saa anda üks suurus sobib kõigile juhised, mis see väärtus peab olema, sest ühe rakenduse jaoks õige väärtus on liiga tähtsateks/vähetähtsateks mõnda muusse rakendusse. See säte ka võib mõjutada jõudlust teiste osade keeruline rakendused, nii, et iga kliendi tuleb täpsustada selle sätte konkreetsetele vajadustele. Hea koht käivitamine on 200 või 300, testige ja kohandada efektisuvandite vastavalt vajadusele.

Kuidas seadistada seda sätet:

-   ASP.net-i, kasutage jaotises ["minIoThreads" Otsingukonfiguratsiooni säte][] on `<processModel>` konfiguratsiooni web.config element. Kui teil on Azure veebisaitide sees, see säte on avatud konfiguratsiooni suvandid kaudu. Siiski saate peaksid olema ikka saate määrata selle programmiliselt (vt allpool) kaudu oma global.asax.cs Application_Start meetod.

> **Olulise teabe märge:** – see element konfiguratsiooni määratud väärtus on *kohta core* säte. Näiteks, kui teil on 4 core arvuti ja soovite oma minIOThreads säte olema 200 käitusajal, kasutaksite `<processModel minIoThreads="50"/>`.

-   ASP.net-i, väljaspool kasutada [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API-GA.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Luba server GC saada lisateavet läbilaskevõime kliendi StackExchange.Redis kasutamise korral

Server GC lubamine saab optimeerida klient ja sisestage parema jõudluse ja läbilaskevõime StackExchange.Redis kasutamisel. Server GC ja selle lubamise kohta lisateabe saamiseks vt järgmisi artikleid.

-   [Server GC lubamiseks](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Prügikoristus alused](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Prügikoristus ja jõudlus](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Kuidas jälgida seisundi ja minu vahemälu jõudlust?

Microsoft Azure'i Redis vahemälu eksemplari saate jälgida [Azure portaali](https://portal.azure.com). Saate vaadata mõõdikute, mõõdikute diagrammide soovitud Startboard kinnitamine, kohandada kuupäeva ja kellaaja vahemik jälgida diagrammide, lisamine ja mõõdikute eemaldamine on diagrammid ja teatiste seadmine kui teatud tingimused on täidetud. Lisateavet leiate teemast [Kuvari Azure'i Redis vahemälu](cache-how-to-monitor.md).

Redis objektivahemälu **sätete** tera **tugi + tõrkeotsingu** jaotise sisaldab mitut tööriista jälgimine ja tõrkeotsingu oma vahemälu. 

-   **Tõrkeotsing:** sisaldab teavet levinud probleemide ja strateegiad lahendamisel.
-   **Auditilogi** sisaldab teavet vahemälu tehtavad toimingud. Samuti saate filtreerimise laiendamiseks selle vaate lisada muud ressursid.
-   **Ressursi seisundi** jälgimine oma ressursside ja ütleb teile, kas see töötab ootuspäraselt. Azure'i ressursi seisund teenuse kohta leiate lisateavet teemast [Azure ressursside seisundi ülevaade](../resource-health/resource-health-overview.md).
-   **Uus tugiteenuste taotlus** moodi avamiseks esitada tugiteenuse taotluse vahemälu jaoks.

Nende tööriistade võimaldavad teil oma Azure'i Redis vahemälu eksemplaride seisundi jälgimine ja aitavad teil oma vahemällu rakenduste haldamine. Lisateavet leiate teemast [tugi ja tõrkeotsingu sätted](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Oma vahemälu diagnostika salvestusruumi sätteid muutnud, mis on juhtunud?

Vahemälu sama piirkonna-ja tellimuse jagamine sama diagnostika talletusmahu ja kui konfiguratsioon on muudetud (diagnostika lubatud või keelatud või salvestusruumi konto muutmine) kehtib kõigi vahemälu tellimus, mis on selle piirkonna. Kui vahemälu diagnostika sätteid muutnud, siis kontrollige, kui diagnostikasätete teise vahemälu samas tellimus ja regiooni jaoks on muutunud. Üheks võimaluseks on teie jaoks vahemälu auditilogide vaatamiseks on `Write DiagnosticSettings` sündmus. Auditilogide töötamise kohta leiate lisateavet teemast [sündmuste vaatamine ja audit logid](../monitoring-and-diagnostics/insights-debugging-with-events.md) ja [auditi toimingud koos ressursihaldur](../resource-group-audit.md). Azure'i Redis vahemälu sündmuste jälgimise kohta leiate lisateavet teemast [toimingud ja teatised](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Miks on mõned uue vahemälu, kuid mitte teiste jaoks lubatud diagnostika?

Vahemälu sama piirkonna ja tellimuse ühiskasutusse anda sama diagnostika salvestusruumi sätted. Kui loote uue vahemälu sama piirkonna ja muu vahemälu, mis on lubatud diagnostika tellimuse, diagnostika on lubatud uue vahemälu sama sätete abil.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Miks ma näen ajalõpud?

Ajalõpud ilmneda rääkida Redis kasutatav klientrakenduses. Enamasti Redis server ei ole ajalõpuni. Käsu saatmisel Redis server käsk on ootele ja Redis serveri lõpuks käsk huvitavat ja käivitab selle. Kuid kliendi saate ajalõpuni käigus ja kui see erandi tõstetakse helistaja küljel. Ajalõpp probleemide tõrkeotsingu kohta leiate lisateavet teemast [Kliendi küljel tõrkeotsing](cache-how-to-troubleshoot.md#client-side-troubleshooting) ja [StackExchange.Redis ajalõpp erandid](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Miks minu klient katkestati vahemälu?

Järgnevalt on toodud mõned levinud põhjused vahemälu Katkesta.

-   Kliendipoolne põhjused
    -   Klientrakenduse oli ümberpaigutatud.
    -   Klientrakenduse läbi skaleerimise toiming.
        -   Puhul pilveteenuste või veebirakenduste, võib see olla tingitud automaatne skaleerimist.
    -   Kliendipoolse võrgu kiht muuta.
    -   Siirdamiseks ilmnes klientrakenduses või võrgu sõlmed kliendi ja serveri vahel.
    -   Funktsiooni läbilaskevõime piirmäärad ei jõudnud.
    -   CPU seotud toiminguid võttis liiga kaua aega.
-   Serveripoolne põhjused
    -   Standardse vahemälu pakub, klõpsake algatatud Azure'i Redis vahemälu teenuse lisamine ei lõppenud esmane sõlme teisene sõlm.
    -   Azure'i oli lappimine eksemplari, kus on juurutatud vahemälu
        -   See võib olla Redis serveri värskendusi või üldine VM hooldustööd.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Millised Azure'i vahemälu pakkumine on mulle sobivaim?

>[AZURE.IMPORTANT]Eelmine aasta [teadaanne](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), ühe Azure'i hallatavate vahemälu teenuse ja Azure-rolli vahemälu teenuse pensionile 30 November 2016. Soovitatav on seda kasutada [Azure Redis vahemälu](https://azure.microsoft.com/services/cache/). Migreerimise kohta leiate teavet teemast [hallatavate vahemälu teenuse Azure Redis vahemälu migreerimine](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure'i Redis vahemälu
Azure'i Redis vahemälu on üldiselt saadaval 53 GB suurused ja saadavus SLA 99,9%. Uue [premium taseme](cache-premium-tier-intro.md) pakub suuruses kuni 530 GB ja tugiteenuste rühmitamiseks VNET ja kestus, kus 99,9% SLA.

Azure'i Redis vahemälu annab klientidele võimalus kasutada turvalist, pühendunud Redis vahemälu, haldab Microsoft. Selle pakkumise saate ära kasutada rikkalike funktsioonikomplekt ja ökosüsteemi Redis, ja usaldusväärne hosting ja Microsofti jälgimine.

Erinevalt traditsioonilise vahemälu, mis käsitlevad ainult võti ja väärtuse paarideks, Redis on populaarsed selle väga kiire andmete tüüpi. Redis ka toetab atomic toiminguid järgmist tüüpi, nt lisamine stringi; suurendades väärtuse räsi; lükkamine loendisse; arvutuste määramine ühisosa, union ja vahe; või saada kõrgeim sorditud kogumi liige. Muud funktsioonid toetavad tehingud, pub/sub, Lua skriptimine klahvid, kellel on piiratud aja-to-live, ja teha veel käitub traditsiooniline vahemälu Redis Otsingukonfiguratsiooni sätetest.

Teise võtme Redis edu on ehitatud ümber terve, elav avatud allika ökosüsteemi. See kajastub erinevaid komplekti Redis klientide saadaval mitmes keeles. See võimaldab neid kasutada peaaegu iga saate luua sees Azure'i töökoormus. 

Alustamine: Azure'i Redis vahemälu kohta leiate lisateavet teemast [Kuidas kasutada Azure Redis vahemälu](cache-dotnet-how-to-use-azure-redis-cache.md) ja [Azure Redis vahemälu dokumentatsiooni](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="managed-cache-service"></a>Hallatavate vahemälu teenuse
[Hallatav teenus on seatud pensionile 30 November 2016 vahemälu.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>Rolli vahemälu
[Rolli vahemälu on seatud pensionile 30 November 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





["minIoThreads" Otsingukonfiguratsiooni säte]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
