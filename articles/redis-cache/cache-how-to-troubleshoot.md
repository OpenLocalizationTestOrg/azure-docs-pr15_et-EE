<properties 
    pageTitle="Azure'i Redis vahemälu tõrkeotsing | Microsoft Azure'i" 
    description="Saate teada, kuidas Azure'i Redis vahemälu seotud levinud probleemid lahendada." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Azure'i Redis vahemälu tõrkeotsing

Sellest artiklist leiate juhised järgmiste kategooriate Azure'i Redis vahemälu probleemide tõrkeotsing.

-   [Kliendi küljel tõrkeotsing](#client-side-troubleshooting) – see jaotis leiate juhised, määratlemine ja lahendamine põhjustatud Azure'i Redis vahemälu ühenduse loomise rakendus.
-   [Serveri pool tõrkeotsing](#server-side-troubleshooting) – see jaotis leiate juhised määratlemine ja lahendamine Azure'i Redis vahemälu serveripoolse põhjustada probleeme.
-   [StackExchange.Redis ajalõpp erandid](#stackexchangeredis-timeout-exceptions) – see jaotis sisaldab teavet tõrkeotsingu StackExchange.Redis kliendi kasutamisel.


>[AZURE.NOTE] Mitut tõrkeotsingu juhiseid teemas sellest juhendist sisaldavad juhiseid Redis käsud ja jälgida erinevate jõudluse mõõdikute. Lisateavet ja juhiseid, kohta leiate [Lisateavet](#additional-information) jaotise artiklitest.

## <a name="client-side-troubleshooting"></a>Kliendi pool tõrkeotsing


Selles jaotises käsitletakse tõrkeotsingu probleeme, mis ilmnevad tingimus klientrakenduse kohta.

-   [Mälu klient](#memory-pressure-on-the-client)
-   [Purunemiskatse liikluse](#burst-of-traffic)
-   [Kõrge Kliendi CPU hõivatus](#high-client-cpu-usage)
-   [Kliendi küljel läbilaskevõime on ületatud](#client-side-bandwidth-exceeded)
-   [Suure taotluse/vastuse suurus](#large-requestresponse-size)
-   [Mis juhtus minu andmed Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Mälu klient

#### <a name="problem"></a>Probleem

Kliendi seadme mälu viib igasuguseid jõudlusprobleemide, mida saab viivitada Redis astme ilma viivituseta saadetud andmete töötlemiseks. Kui mälu tabab, süsteemi tavaliselt on lehe andmete füüsilise mälu virtuaalmälu, mis on ketas. Selle *lehe faulting* põhjustab süsteemi märkimisväärselt aeglustada.

#### <a name="measurement"></a>Mõõtühikute 

1.  Jälgida mälukasutuse arvutisse, veenduge, et see ei ületa mälu. 
2.  Kuvari soovitud `Page Faults/Sec` Jõudluseloenduri. Enamik süsteemid on mõned lehe vead isegi tavalise töötamise ajal, et vaadata selle lehe vead jõudluse vastupäeva diagrammi, mis vastavad ajalõpud.

#### <a name="resolution"></a>Eraldusvõime

Oma kliendi versioonile suuremat kliendi VM suurus rohkem mälu või tõrkeid uurida oma mälu mustreid mälu kütusekulu vähendada.


### <a name="burst-of-traffic"></a>Purunemiskatse liiklus

#### <a name="problem"></a>Probleem

Liikluse puruneb koos halb `ThreadPool` sätted võib põhjustada viivitust andmete Redis serveri juba saadetud, kuid pole veel tarbitud kliendis.

#### <a name="measurement"></a>Mõõtühikute 

Kuvari kuidas oma `ThreadPool` statistika muuta aja jooksul, kasutades koodi [järgmiselt](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Saate ka vaadata selle `TimeoutException` StackExchange.Redis sõnum. Siin on näide:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Eespool sõnum on mitmed probleemid, mis on huvitav.

 1. Pange tähele, et selle `IOCP` jaotis ja `WORKER` jaotist teil on `Busy` väärtus, mis on suurem kui on `Min` väärtus. See tähendab, et teie `ThreadPool` sätted vaja.
 2. Saate vaadata ka `in: 64221`. See näitab, et 64211 baiti on saanud tuum turvasoklite kiht, kuid see pole veel loetud rakendus (nt StackExchange.Redis). See tähendab tavaliselt, et teie rakendus pole andmete lugemine võrgust nii kiiresti kui server on teile selle saatmise.

#### <a name="resolution"></a>Eraldusvõime

Konfigureerida veenduge, et teie jutulõnga pool kuvatakse skaalal kiiresti jaotises lõhkemist stsenaariumid [ThreadPool sätted](https://gist.github.com/JonCole/e65411214030f0d823cb) .


### <a name="high-client-cpu-usage"></a>Kõrge Kliendi CPU hõivatus

#### <a name="problem"></a>Probleem

Kõrge CPU klient on kinnitab, et süsteemi ei saa kursis tööd, mis see on palunud teha. See tähendab, et klient ei pruugi töötlemine õigel Redis vastust, ehkki Redis saadetud vastuse väga kiiresti.

#### <a name="measurement"></a>Mõõtühikute

Jälgimine süsteemi lai CPU kasutus Azure portaali kaudu või seotud jõudluse näidiku. Olla ettevaatlik, et jälgida *protsessi* CPU, sest ühe protsess võib olla madala CPU üldise süsteemi CPU saab kõrge samal ajal. Olge diagrammi CPU hõivatus, mis vastavad ajalõpud. Tulemusena kõrge CPU, võidakse teile kuvada ka kõrge `in: XXX` väärtust `TimeoutException` veateateid, nagu on kirjeldatud jaotises [liikluse plahvatuse](#burst-of-traffic) .

>[AZURE.NOTE] StackExchange.Redis 1.1.603 ja hiljem sisaldab selle `local-cpu` argumendil sisse `TimeoutException` tõrketeated. Tagada te kasutate [StackExchange.Redis Nugeti pakett](https://www.nuget.org/packages/StackExchange.Redis/)uusim versioon. Leiduvad vead pidevalt on fikseeritud koodi teha seda senisest käepärasemad ajalõpud, millel on uusim versioon on oluline.

#### <a name="resolution"></a>Eraldusvõime

Täiendamine suurema CPU jõudlusega VM suuremaks või uurida, mis võib põhjustada CPU diagrammi. 



### <a name="client-side-bandwidth-exceeded"></a>Kliendi pool läbilaskevõime on ületatud

#### <a name="problem"></a>Probleem

Erinevate suurusega klientarvutite kehtida võrgu läbilaskevõime palju neil on saadaval. Kui kliendi ületab läbilaskevõime, siis andmeid ei töödelda kliendi poolel nii kiiresti kui server on selle saatmise. See võib põhjustada ajalõpud.

#### <a name="measurement"></a>Mõõt

Jälgida, kuidas teie läbilaskevõime kasutuse muuta aja jooksul, kasutades koodi [järgmiselt](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Pange tähele, et see kood ei pruugi töötada edukalt mõned keskkonnas piiratud õigustega (nt Azure veebisaite).

#### <a name="resolution"></a>Eraldusvõime 

Kliendi VM suurendamine või vähendamine võrgu läbilaskevõime kulu.


### <a name="large-requestresponse-size"></a>Suure taotluse/vastuse suurus

#### <a name="problem"></a>Probleem

Suure taotluse/vastuse, võivad põhjustada ajalõpud. Näiteks Oletame, et teie konfigureeritud oma kliendi ajalõpp väärtus on 1 teise. Rakenduse taotleb kahe klahvi (nt "A" ja "B"), samal ajal (kasutades sama füüsilise võrguühendust). Enamik kliente tuge "Torujuhtmete" taotlused, nii, et nii taotluste "A" ja "B" saatmise kohta kaabel server, üks üksteise järel ootamata vastuseid. Server ei saada vastuseid tagasi samas järjestuses. Kui vastus on "A" on suur piisavalt see hilisemaks taotlused ajalõpp enamik sööma. 

Järgmises näites näitab seda stsenaariumi. Selle stsenaariumi korral saadetakse kutse "A" ja "B" kiiresti, server hakkab vastuste "A" ja "B" saatmine kiiresti, kuid andmete edastamine korda, kuna "B" ei jõua taga taotlus ja ajalõpp isegi juhul, kui server vastas kiiresti.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Mõõt

See on raske mõõta. Teil on põhiliselt dokumendi jälgimiseks suurte taotlusi ja koosolekutaotluste oma kliendi kood. 

#### <a name="resolution"></a>Eraldusvõime

1.  Redis on optimeeritud suure hulga väiksed väärtused, mitte suurte väärtuste. Eelistatud lahendus on Liigendage oma andmeid seotud väiksemad väärtused. Vaadake selle [mis on väärtus optimaalne suurus vahemiku jaoks redis? 100KB on liiga suur?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Lisateavet ümber miks väiksemad väärtused on soovitatav postitada.
2.  Suurendamine oma VM (klient ja Redis vahemälu Server) jaoks saada suurema läbilaskevõime võimalusi, andmete edastamine korda suurema vastuste vähendamine. Pange tähele, et saada rohkem ribalaiust ainult server või lihtsalt klient ei pruugi olla piisavalt. Mõõtke oma läbilaskevõime kasutuse ja võrrelda seda võimaluste VM, mis teil praegu on suurust.
3.  Suurendage `ConnectionMultiplexer` objektide saate kasutada ja round-jaan taotluste üle erinevate ühendused.


### <a name="what-happened-to-my-data-in-redis"></a>Mis juhtus minu andmed Redis?

#### <a name="problem"></a>Probleem

Ma oodata teatud andmete minu Azure'i Redis vahemälu eksemplari, kuid see ei tundu seal.

##### <a name="resolution"></a>Eraldusvõime

Vt [Redis mu andmed on kadunud?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) võimalikud põhjused ja lahendused.


## <a name="server-side-troubleshooting"></a>Serveri pool tõrkeotsing

Selles jaotises käsitletakse tõrkeotsingu probleeme, mis ilmnevad tingimus vahemälu serveris.

-   [Mälu serveris](#memory-pressure-on-the-server)
-   [Kõrge CPU hõivatus / serveri laadimine](#high-cpu-usage-server-load)
-   [Server Side läbilaskevõime on ületatud](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Mälu serveris

#### <a name="problem"></a>Probleem

Mälu serveripoolse viib igasuguseid jõudlusega probleeme, mis saab edasi lükata taotluste töötlemist. Kui mälu tabab, süsteemi tavaliselt on lehe andmete füüsilise mälu virtuaalmälu, mis on ketas. Selle *lehe faulting* põhjustab süsteemi märkimisväärselt aeglustada. On mitu võimalikku põhjust seda mälu. 

1.  Olete täitnud vahemälu täielik mahu andmetega. 
2.  Redis on näha mälu killustatus - enamasti põhjustatud talletamise suured objektid (väike objektid on optimeeritud Redis - näha selle [, mis on väärtus optimaalne suurus vahemiku jaoks redis? 100KB on liiga suur?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Postitage üksikasjad). 

#### <a name="measurement"></a>Mõõtühikute

Redis seab mõõdikud, mis aitavad teil probleemi tuvastada. Esimene on `used_memory` ja teine on `used_memory_rss`. [Need mõõdikud](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) on saadaval Azure portaali või käsk [Redis teave](http://redis.io/commands/info) läbi.

#### <a name="resolution"></a>Eraldusvõime

On mitu võimalikku aitab hoida terve mälukasutuse tehtud muutused.

1. [Konfigureerimine mälu poliitika](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ja määramine aegumise korda oma võtmed. Pange tähele, et see ei pruugi olla piisavalt kui teil on killustatus.
2. [Konfigureerimine maxmemory reserveeritud väärtus](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , mis on piisavalt suur, et kompenseeri mälu killustumise.
3. Liigendage oma suurte vahemällu talletatud objektide väiksemate seotud objektid.
4. [Skaala](cache-how-to-scale.md) vahemälu suuremaks.
5. Kui kasutate [premium vahemälu Redis kobar lubatud abil](cache-how-to-premium-clustering.md) saate [suurendada shards arvu](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Kõrge CPU hõivatus / serveri laadimine

#### <a name="problem"></a>Probleem

Kõrge CPU hõivatus saate tähendab, et kliendipoolse ei pruugi töötlemine õigel Redis vastust, ehkki Redis saadetud vastuse väga kiiresti.

#### <a name="measurement"></a>Mõõtühikute

Jälgimine süsteemi lai CPU kasutus Azure portaali kaudu või seotud jõudluse näidiku. Olla ettevaatlik, et jälgida *protsessi* CPU, sest ühe protsess võib olla madala CPU üldise süsteemi CPU saab kõrge samal ajal. Olge diagrammi CPU hõivatus, mis vastavad ajalõpud.

#### <a name="resolution"></a>Eraldusvõime

Suurema vahemälu [skaala](cache-how-to-scale.md) esimese taseme suurema CPU jõudlusega või uurida, mis võib põhjustada CPU diagrammi. 

### <a name="server-side-bandwidth-exceeded"></a>Server Side läbilaskevõime on ületatud

#### <a name="problem"></a>Probleem

Erinevate suurusega vahemälu eksemplarid on piirangud võrgu läbilaskevõime palju neil on saadaval. Kui server ületab läbilaskevõime, siis andmeid ei saadetakse kliendi kiiresti. See võib põhjustada ajalõpud.

#### <a name="measurement"></a>Mõõtühikute

Saate jälgida funktsiooni `Cache Read` mõõdiku, mis on suurt hulka andmeid lugeda vahemälust megabaitides määratud perioodil sekundis (MB/s). See väärtus vastab võrgu läbilaskevõime, mis kasutavad vahemälu. Kui soovite häälestada teatised serveri pool võrgu läbilaskevõime piirangute, saate luua neid kasutades seda `Cache Read` counter. Võrrelge oma näite [selle tabeli](cache-faq.md#cache-performance) jaoks eri vahemälu hinnad astme ja suurused täheldatud läbilaskevõime limiidid väärtustega.

#### <a name="resolution"></a>Eraldusvõime

Kui olete pidevalt lähedal täheldatud maksimaalse läbilaskevõime hinnakirjad taseme- ja vahemälu maht, kaaluge [skaleerimist](cache-how-to-scale.md) hinnakirjad taseme või suurust, mis on suurem võrgu läbilaskevõime, kasutades juhend [selles](cache-faq.md#cache-performance) tabelis olevad väärtused.


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis ajalõpp erandid

StackExchange.Redis kasutab konfiguratsiooni säte nimega `synctimeout` sünkroonse toiminguid, mis on vaikeväärtus on 1000 ms jaoks. Kui sünkroonse kõne lõpetamiseks ei ettenähtud aja StackExchange.Redis kliendi põhjustab ajalõpp tõrketeade, mis sarnaneb järgmises näites.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


See tõrketeade kuvatakse sisaldab mõõdikud, mis aitavad osutage probleemi põhjuse ja võimalik eraldusvõime. Järgmine tabel sisaldab viga sõnumi mõõdikute üksikasjad.

| Tõrge sõnumi meetermõõdustik | Üksikasjad                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inst       | Klõpsake viimase aja sektorit: 0 käsud on välja antud                                                                                                                                                                                              |
| MGR        | Turvasoklite haldur täidab `socket.select` mis tähendab, et küsib OS tähistamiseks turvasoklite, mis sisaldab midagi teha; sellise tõrke ilmnemisel: lugeja ei ole aktiivselt lugemine võrgust kuna see ei arvate, et pole midagi teha |
| järjekord      | On pooleli tegevus 73 kokku                                                                                                                                                                                                        |
| qu         | 6 pooleli toimingud on saatmata järjekorras ja pole veel võrku väljaminevate kirjutatud                                                                                                                                                           |
| Qs         | 67 he pooleli toimingute on saadetud server, kuid vastust pole veel saadaval. Vastus võib olla `Not yet sent by the server` või`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 pooleli toimingute näinud vastused, kuid pole veel tõttu ootel lõpetamise tsükkel lõpetatuks märgitud                                                                                                                                      |
| WR         | Aktiivse kirjutaja (st 6 saatmata taotlused ei ignoreeritakse) baiti/activewriters on                                                                                                                                                   |
| rakenduses         | On pole aktiivne lugeja ja null baiti on saadaval lugemiseks NIC baiti või activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Juhised uurimine

1. Hea tava veenduge, et kasutate järgmise mustriga StackExchange.Redis kliendi kasutamisel ühendamiseks.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Lisateavet leiate teemast [ühenduse loomine abil StackExchange.Redis vahemällu](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Veenduge, et Azure'i Redis vahemälu ja klientrakenduse oleks sama piirkonna Azure. Näiteks pruugi te saada ajalõpud vahemälu on Ida-USA, kuid klient on Lääne USA ja taotluse ei jooksul lõpule viia ning `synctimeout` intervall või võib saada ajalõpud kui teil on silumine teie kohaliku arengu arvutist. 

    See on soovitatav on vahemälu ja Azure piirkonna klientrakenduses. Kui teil on stsenaarium, mis sisaldab rist piirkond kõnesid, peate seadma selle `synctimeout` intervalli väärtuse, mis on suurem kui 1000 ms vaikimisi intervalli, sh on `synctimeout` ühendusstringi atribuudi. Järgmises näites on kujutatud StackExchange.Redis vahemälu ühenduse stringi koodilõigu koos mõne `synctimeout` 2000 MS.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Tagada te kasutate [StackExchange.Redis Nugeti pakett](https://www.nuget.org/packages/StackExchange.Redis/)uusim versioon. Leiduvad vead pidevalt on fikseeritud koodi teha seda senisest käepärasemad ajalõpud, millel on uusim versioon on oluline.

4. Kui taotlusi, mis on seotud saada läbilaskevõime piirangute serveri või kliendi, on see kauem neid lõpuleviimine ja seega ajalõpud. Kui teie ajalõpp on põhjustatud võrgu läbilaskevõime serveris, vaadake [serveri pool läbilaskevõime on ületatud](#server-side-bandwidth-exceeded). Kui teie ajalõpp on põhjustatud kliendi võrgu läbilaskevõime, vaadake [Kliendi küljel läbilaskevõime on ületatud](#client-side-bandwidth-exceeded).

6. On teil saada CPU seotud serveris või klient?
    -   Märkige ruut, kui teil on saada seotud CPU oma klienti, mis võivad põhjustada taotluse ei töödelda sees on `synctimeout` intervall, põhjustades ajalõpp. Kliendi suuremaks teisaldamine või levitamise laadi aitab selle kontrollimiseks. 
    -   Kontrollige, kas teil on saada CPU seotud serveris jälgides on `CPU` [vahemälu jõudluse meetermõõdustik](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Taotlused tulevad Redis on CPU seotud, võivad põhjustada nende taotluste ajalõpp. Selle saate levitada laadi üle mitme shards premium vahemälus või suuremaks või hinnakirjad taseme versioonile. Lisateabe saamiseks vt [Server Side läbilaskevõime on ületatud](#server-side-bandwidth-exceeded).

7. Kas on olemas serveris töötlemine võtab kaua aega käskude? Kaua töötavad käsud, mis võtab kaua aega, et töödelda redis serveris, võivad põhjustada ajalõpud. Mõned näited pikaajalise käsud `mget` klahvid, suure arvu `keys *` või halvasti kirjutatud lua skripte. Ühenduse loomine oma Azure'i Redis vahemälu eksemplari redis-cli kliendi või [Redis konsooli](cache-configure.md#redis-console) ja kas on taotlusi oodatust kauem käsu [SlowLog](http://redis.io/commands/slowlog) . Redis Server ja StackExchange.Redis on optimeeritud palju väike taotlusi asemel vähem suur taotlused. Oma andmete tükeldamist väiksem tükkideks võib parandada asju siin. 

    Azure'i Redis vahemälu SSL-i lõpp-punkti redis-cli ja stunnel abil ühenduse loomise kohta leiate teavet leiate ajaveebipostitusest [Redis eelvaateversiooniga teatavaks ASP.net-i seansi olek pakkuja](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) . Lisateavet leiate teemast [SlowLog](http://redis.io/commands/slowlog).

8. Kõrge Redis serveri koormus võib põhjustada ajalõpud. Saate jälgida serveri koormus jälgimine selle `Redis Server Load` [vahemälu jõudluse meetermõõdustik](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Serveri koormus 100 (suurim väärtus) tähendab, et redis server on hõivatud aja jooksul jõudeolekus töötlemise. Kui võtate teatud taotlusi kuni kõik serveri võimalus kuvamiseks käivitage käsk SlowLog, nagu on kirjeldatud eelmises punktis. Lisateavet leiate teemast [suure CPU / serveri koormus](#high-cpu-usage-server-load).

9. Oli mõne muu sündmuse põhjuseks võib olla võrgu laik kliendi poolel? Märkige ruut (web, töötaja rolli või mõne Iaas VM) klient kui sündmuse nagu skaleerimist kliendi eksemplaride arv üles või alla või juurutamine kliendi või mastaabi automaatselt uus versioon on lubatud? Meie katsetamine oleme leidnud, võivad põhjustada autoscale või skaleerimist üles/alla väljaminev võrguühendus võib olla kaotsi mitu sekundit. StackExchange.Redis kood on olles selliste sündmuste ja parooli. Sel ajal, uuesti ühendust taotlused järjekorda saate ajalõpuni.

10. Oli suur taotluse eelneva mitu small päringuid Redis aegus vahemälu? Parameetri `qs` viga sõnumiks on mitu taotlusi saadeti serveriga klient, kuid ei ole veel töödeldud vastuse. See väärtus saab hoida kasvab, kuna StackExchange.Redis kasutab ühe TCP-ühenduse ja lugeda ainult üks vastus korraga. Isegi juhul, kui esimene toiming aegus, see ei lõpe server ja sealt saadetavate andmete ja muud taotlused on blokeeritud seni, kuni see on valmis, põhjustades aja üksikasjad. Üks lahendus on märgiks ajalõpud vahemälu on piisavalt suur, et teie töökoormus ja suurte väärtuste mitmeks väiksemaks tükkideks. Teine võimalik lahendus on kasutada kogumi `ConnectionMultiplexer` objektide oma kliendi ja valige vähemalt laaditud `ConnectionMultiplexer` Uus koosolekukutse saatmiseks. See peaks ühe ajalõpp takistada põhjustab muude taotluste ka ajalõpp.

11. Kui kasutate `RedisSessionStateprovider`, proovige uuesti ajalõpp on õigesti häälestatud. `retrytimeoutInMilliseconds`peaks olema suurem kui `operationTimeoutinMilliseonds`, muidu ei korduskatsed juhtub. Järgmises näites `retrytimeoutInMilliseconds` on seatud 3000. Lisateavet leiate teemast [ASP.net-i seansi olek pakkuja Azure'i Redis vahemälu](cache-aspnet-session-state-provider.md) ja [Kuidas kasutada konfiguratsiooni parameetrid seansi oleku pakkuja ja väljundi vahemälu pakkuja](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Märkige ruut server Azure'i Redis vahemälu mälukasutuse [jälgides](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` ja `Used Memory`. Kui ka väljatõstmine poliitika on juba olemas, käivitab Redis evicting nooleklahvid, millal `Used_Memory` jõuab vahemälu maht. Ideaalvariandis mitte `Used Memory RSS` peaks olema ainult veidi suurem kui `Used memory`. Suur erinevus tähendab on mälu killustumise (sise- või. Kui `Used Memory RSS` on väiksem kui `Used Memory`, tähendab see osa vahemälu on vahetatud operatsioonisüsteem. Sellisel juhul võib juhtuda, et mõned olulised latentsused. Kuna Redis ei saa juhtida, kuidas oma jaotus on vastendatud mälu lehtede, kõrge `Used Memory RSS` on sageli tulemus on kühvli mälukasutuse sisse. Kui Redis vabastab mäluga, mälu on tagasi pöörata selle jaoturi ja selle jaoturi võib või ei või anda mälu tagasi süsteem. Vahel võib selle `Used Memory` väärtus ja mälu tarbimine, nagu on näidatud operatsioonisüsteem. See võib olla tingitud asjaolust mälu on kasutatud ja välja Redis, kuid mitte antud süsteemi tagasi. Mälumahuga seotud probleemid leevendada abil saate teha järgmist.
    -   Võtke kasutusele vahemälu suuremaks nii, et te ei kasuta vastu mälu piirangud süsteemi.
    -   Seada aegumise korda klahve nii, et vanemate väärtused välja tõsta aktiivselt.
    -   Kuvari soovitud funktsiooni `used_memory_rss` vahemälu meetermõõdustik. Kui see väärtus lähenedes oma vahemälu suurust, tõenäoliselt alustada näha jõudlusprobleeme. Jagada andmed üle mitme shards, kui kasutate premium vahemälu, või vahemälu suuremaks täiendamine.

    Lisateavet leiate teemast [Mälu serveris](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Lisateave

-   [Kuidas Redis vahemälu pakkumise ja suurus kasutada?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Kuidas võrrelda ja testida minu vahemälu?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Kuidas ma saan Redis käsud?](cache-faq.md#how-can-i-run-redis-commands)
-   [Kuidas jälgida Azure'i Redis vahemälu](cache-how-to-monitor.md)