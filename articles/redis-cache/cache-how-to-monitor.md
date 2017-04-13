<properties 
    pageTitle="Kuidas jälgida Azure'i Redis vahemälu | Microsoft Azure'i" 
    description="Saate teada, kuidas Azure'i Redis vahemälu eksemplaride seisundi ja jõudluse jälgimine" 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Kuidas jälgida Azure'i Redis vahemälu

Azure'i Redis vahemälu pakub mitmeid võimalusi oma vahemälu eksemplari jälgimiseks. Saate vaadata mõõdikute, kinnitamine mõõdikute diagrammide soovitud Startboard, kohandada kuupäeva ja kellaaja vahemik jälgida diagrammide, lisamine ja mõõdikute eemaldamine on diagrammid ja teatiste seadmine kui teatud tingimused on täidetud. Nende tööriistade võimaldavad teil oma Azure'i Redis vahemälu eksemplaride seisundi jälgimine ja haldamine vahemällu rakenduste abil.

Kui vahemälu diagnostika on lubatud, mõõdikute Azure'i Redis vahemälu eksemplaride kogutakse ligikaudu iga 30 sekundi järel ja talletatud, et nad saaksid kuvatakse mõõdikute diagrammide ja teatiste reeglite poolt.

Vahemälu mõõdikute kogutakse, kasutades funktsiooni Redis käsku [teave](http://redis.io/commands/info) . Lisateavet erinevate teave iga kasutatavaid väärtusi vahemälu meetermõõdustik leiate teemast [saadaval mõõdikute ja aruandlus intervalle](#available-metrics-and-reporting-intervals).

Vaadata vahemälu mõõdikute, [otsige](cache-configure.md#configure-redis-cache-settings) oma vahemälu eksemplari [Azure portaali](https://portal.azure.com). Azure'i Redis vahemälu eksemplaride mõõdikute pääseb **Redis mõõdikute** enne.

![Redis mõõdikud][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Kui **Redis mõõdikute** tera kuvatakse järgmine teade, järgige juhiseid jaotises [Luba vahemälu diagnostika](#enable-cache-diagnostics) lubamiseks vahemälu diagnostika.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

**Redis mõõdikute** tera on **jälgimis** diagrammid, mis kuvavad vahemälu mõõdikute. Igale diagrammile saab kohandada, lisamise või eemaldamise mõõdikute ja muutes aruandlusteenuste intervalli. Kuvamis-ja konfigureerimise toimingud ja teatised, **Vahemälu Redis** tera on **toimingute** jaotis, kuvatakse vahemälu **sündmused** ja **teatiste reegleid**.

## <a name="enable-cache-diagnostics"></a>Vahemälu diagnostika lubamine

Azure'i Redis vahemälu pakub teile võimalus on salvestatud salvestusruumi konto abil saate mis tahes tööriistad, mida soovite juurdepääsu ja andmed otse töötlemine diagnostika andmeid. Selleks tuleb koguda vahemälu diagnostika, talletatud ja Azure portaali, kuvatakse salvestusruumi konto peab olema konfigureeritud. Vahemälu sama piirkonna-ja tellimuse jagamine sama diagnostika salvestusruumi konto ja konfiguratsiooni muutmisel kehtib kõik vahemälu tellimus, mis on selle piirkonna.

Lubamiseks ja konfigureerimiseks vahemälu diagnostika, liikuge **Redis vahemälu** tera vahemälu eksemplari puhul. Kui diagnostika pole veel lubatud, kuvatakse sõnumi asemel diagnostika diagrammi.

![Vahemälu diagnostika lubamine][redis-cache-enable-diagnostics]

Klõpsake sõnumi kuvamiseks **meetermõõdustik** tera ja **diagnostikasätete** lubamiseks ja konfigureerimiseks diagnostika sätete vahemälu eksemplari.

![Diagnostika sätete][redis-cache-diagnostic-settings]

![Diagnostika konfigureerimine][redis-cache-configure-diagnostics]

Klõpsake vahemälu diagnostika lubamine ja konfigureerimine diagnostika kuvamiseks **klõpsake** nuppu.

Klõpsake noolt paremas servas **Salvestusruumi konto** salvestusruumi konto ootelepanekuks Diagnostikaandmete valimiseks. Parima jõudluse tagamiseks valige salvestusruumi konto sama piirkonna vahemälu.

Kui diagnostika sätted on konfigureeritud, klõpsake nuppu **Salvesta** konfiguratsiooni salvestamiseks. Pange tähele, et see võib võtta mõne hetke muudatuste jõustumiseks.

>[AZURE.IMPORTANT] Vahemälu sama piirkonna-ja tellimuse jagamine sama diagnostika talletusmahu ja kui konfiguratsioon on muudetud (diagnostika lubatud või keelatud või salvestusruumi konto muutmine) kehtib kõigi vahemälu tellimus, mis on selle piirkonna.

Salvestatud mõõdikute vaatamiseks läbi vaadata tabelite salvestusruumi konto nimed algavate `WADMetrics`. Salvestatud mõõdikute väljaspool Azure portaali pääsemise kohta leiate lisateavet teemast [Accessi Redis vahemälu jälgimise andmete](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) näidis.

>[AZURE.NOTE] Azure'i portaalis kuvatakse ainult mõõdikute, mis on talletatud valitud salvestusruumi konto. Kui muudate salvestusruumi kontod, eelnevalt konfigureeritud salvestusruumi konto andmed jäävad allalaadimiseks saadaval, kuid seda ei kuvata Azure'i portaalis.  

## <a name="available-metrics-and-reporting-intervals"></a>Saadaval mõõdikute ja intervallide teatamine

Vahemälu mõõdikute esitatakse mitu aruandlusteenuste intervalle, sh **tunnis**, **eile**, **möödunud nädalal**ja **kohandatud**. Iga mõõdikute diagrammi **meetermõõdustik** tera kuvatakse selle Keskmine, miinimum- ja maksimumväärtused väärtused iga mõõdiku diagrammi ja teatud mõõdikute kuvada kokku aruandlusteenuste intervalli. 

Iga mõõdiku sisaldab kaks versiooni. Ühe meetermõõdustik meetmed jõudluse kogu vahemälu ja vahemälu kasutavate, [rühmitamise](cache-how-to-premium-clustering.md), mõne teise versiooni meetermõõdustik, mis sisaldab `(Shard 0-9)` jaoks ühe Kildu vahemälus täitmisel nimi mõõdud. Näiteks kui vahemälu on 4 shards, `Cache Hits` on kogu vahemälu kokkulangevuste kogusumma ja `Cache Hits (Shard 3)` on lihtsalt kokkulangevuste selle Kildu vahemälu.

>[AZURE.NOTE] Isegi kui vahemälu on ühendatud aktiivse klientrakendustes koos ootel, võidakse kuvada mõni vahemälu tegevuse, nt omavahel seotud klientide, mälukasutust ja tehtud toimingute. See on tavaline Azure'i Redis vahemälu eksemplari selle toimingu käigus.

| Meetermõõdustik            | Kirjeldus                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vahemälutabamusi        | Eduka võtme otsingud määratud perioodil arv. See kaardid `keyspace_hits` kaudu Redis [teave](http://redis.io/commands/info) käsk.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Vahemälu jätab      | Nurjunud võtme otsingud perioodil määratud arv. See kaartide `keyspace_misses` käsu Redis teave. Vahemälu jätab ei tähenda tingimata on vahemälu küsimus. Näiteks vahemälu-kõrvale programmeerimise mustri kasutamisel rakenduse otsib esimese üksuse vahemälu. Kui üksus on (vahemälu nurjumine), üksuse ja andmebaasist ja edaspidiseks kasutamiseks vahemällu lisatud. Vahemälu jätab on tavaline käitumine vahemälu-kõrvale programmeerimise muster. Kui vahemälu jätab arv on kõrgem kui, kontrollige rakenduse loogika, mis kuvab ja loeb vahemälu. Kui üksused häätää vahemälu mälu tõttu, siis võib olla mõni vahemälu jätab, kuid oleks parem mõõdiku jälgida mälu `Used Memory` või `Evicted Keys`. |
| Ühendatud kliendid | Kliendi ühendusi vahemälu määratud perioodil arv. See kaardid `connected_clients` käsu Redis teave. Kui [ühenduse piirang](cache-configure.md#default-redis-server-configuration) on jõudnud nurjuvad edaspidised ühenduse katsete vahemällu. Pange tähele, et ka siis, kui on pole aktiivne klientrakendusega võib siiski olla mõni eksemplarid ühendatud klientide sisemiste protsesside ja ühendused.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Väljatõstetud kiirklahvid      | Vahemälu perioodil määratud tähtaja, et eemaldada üksuste arv on `maxmemory` piiratud. See kaardid `evicted_keys` käsu Redis teave.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Aegunud kiirklahvid      | Üksuste arv aegunud vahemälu määratud perioodil. See väärtus kaardid `expired_keys` käsu Redis teave.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Saab              | Hangi toimingute vahemälust perioodil määratud arv. See väärtus on väärtuste summa järgmist Redis teave käsku Kõik: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, ja `cmdstat_getrange`, ja võrdub summa Vahemälutabamusi ja jätab perioodil.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis serveri laadimine | Protsent, kus Redis server on hõivatud töötlemine ja ei oodanud jõude sõnumite tsüklit. Kui see counter jõuab 100, tähendab see Redis server on tulemus jõudluse ceiling ja CPU ei saa töödelda mis tahes kiiremini töötada. Kui te ei näe serveri suure koormuse Redis seejärel kuvatakse ajalõpp erandid klientrakenduses. Sel juhul võiksite kaaluda ülespoole või eraldamine andmete mitme vahemälu sisse.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Komplektid              | Arvu määramine toimingute vahemällu määratud perioodil. See väärtus on Redis teave väärtuste summa järgmine käsk kõik: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, ja `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Tegevus kokku  | Käskude määratud perioodil vahemälu server töötleb koguarv. See väärtus kaardid `total_commands_processed` käsu Redis teave. Pange tähele, et kui Azure Redis vahemälu kasutatakse üksnes pub/sub pole mõõdikud `Cache Hits`, `Cache Misses`, `Gets`, või `Sets`, kuid on `Total Operations` mõõdikud kajastamiseks vahemälu kasutus pub/sub-analüüsitoiminguid.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Mällu       | Vahemälu kasutatakse /-väärtuse paarideks MB vahemälu määratud perioodil summa. See väärtus kaardid `used_memory` käsu Redis teave. See ei sisalda metaandmete või killustatus.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Mällu RSS-i   | Vahemälu kasutatakse perioodil määratud, sh killustatus ja metaandmete MB hulk. See väärtus kaardid `used_memory_rss` käsu Redis teave.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PROTSESSOR               | CPU kasutamine server Azure'i Redis vahemälu protsendina määratud perioodil. See väärtus kaartide operatsioonisüsteemi `\Processor(_Total)\% Processor Time` Jõudluseloenduri.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Vahemälu lugemine        | Suurt hulka andmeid lugeda vahemälu megabaitides määratud perioodil sekundis (MB/s). See väärtus on tuletatud virtuaalse masina, mis majutab vahemälu ja pole Redis teatud toetavad võrgu kasutajaliidese kaardid. **See väärtus vastab võrgu läbilaskevõime, mis kasutavad vahemälu. Kui soovite häälestada teatised serveri pool võrgu läbilaskevõime piirangute, looge see abil `Cache Read` counter. Leiate [selle tabeli](cache-faq.md#cache-performance) jaoks eri vahemälu hinnad astme ja suurused täheldatud läbilaskevõime limiidid.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Vahemälu kirjutamine       | Teatamise andmehulga kirjutada vahemällu megabaitides ajal määratud sekundis (MB/s). See väärtus on tuletatud virtuaalse masina, mis majutab vahemälu ja pole Redis teatud toetavad võrgu kasutajaliidese kaardid. See väärtus vastab andmete vahemällu saadetakse kliendi võrgu läbilaskevõime.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Kuidas vaadata mõõdikute ja diagrammide kohandamine

Saate vaadata ülevaate mõõdikud vahemälu **Redis mõõdikute** enne. Juurdepääs **Redis mõõdikute** blade valige **Kõik sätted** > **Redis mõõdikute**.

![Redis mõõdikud][redis-cache-redis-metrics]


**Redis mõõdikute** tera sisaldab järgmisi diagramme.

| Redis mõõdikute diagramm | Kuvatud mõõdikud     |
|------------------|-------------------|
| Vahemälu lugemis- ja vahemälu kirjutamine | Vahemälu lugemine |
|                            | Vahemälu kirjutamine |
| Ühendatud kliendid      | Ühendatud kliendid |
| Tabamust ja jätab  | Vahemälutabamusi        |
|                  | Vahemälu jätab      |
| Kokku käsud   | Tegevus kokku  |
| Saab ja komplektid    | Saab              |
|                  | Komplektid              |
| CPU hõivatus         | PROTSESSOR           |
| Mälu kasutamine      | Mällu   |
|                   | Mällu RSS-i |
| Redis serveri laadimine | Serveri laadimine   |
| Klahv loendamine | Kokku võtmed |
|           | Väljatõstetud võtmed |
|           | Aegunud võtmed |


Üksikasjalikuma vaate mõõdikud teatud diagrammi ja kohandada diagrammi, klõpsake soovitud diagrammi keelest **Redis mõõdikute** **meetermõõdustik** tera, et diagrammi kuvamiseks.

![Argumendil tera.][redis-cache-metric-blade]

**Meetermõõdustik** tera, et diagrammi allservas on loetletud kõik teatised, kuvatakse diagrammi mõõdikud määratud.

Lisamine või eemaldamine mõõdikute või aruandlusteenuste intervalli muutmiseks, valige käsk **Redigeeri diagrammi**.

Lisada või mõõdikute diagrammilt eemaldada, klõpsake mõõdiku nime kõrval olev ruut. Aruandlusteenuste intervalli muutmiseks klõpsake soovitud aeg. **Diagrammitüübi**muutmiseks klõpsake soovitud laadi. Kui soovitud muudatused on tehtud, klõpsake nuppu **Salvesta**. 

![Diagrammi redigeerimine][redis-cache-edit-chart]

Kui klõpsate käsku **Salvesta** muudatused püsida kuni **väärtuseks meetermõõdustik** tera lahkute. Kui te hiljem, kuvatakse algse meetermõõdustik ja ajavahemiku uuesti. Diagrammide kohandamise kohta leiate lisateavet teemast [kuvari teenuse mõõdikute](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Kuvatakse selle intervalli mõõdikute ja teatud aja jooksul mõõdikud vaatamiseks diagrammi Hõljutage kursorit üle ühe kindla lindid või punktide diagrammi, mis vastab soovitud aeg.

![Diagrammi üksikasjade kuvamine][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Kuidas jälgida premium vahemälu abil rühmitamise

Premium vahemälu, mis on [rühmitamise](cache-how-to-premium-clustering.md) lubatud võib olla kuni 10 shards. Iga Kildu on oma mõõdikute ja nende mõõdikute liidetakse vahemälu kogu mõõdikute ette. Iga mõõdiku sisaldab kaks versiooni. Ühe meetermõõdustik meetmed kogu vahemälu ja teise versiooni meetermõõdustik, mis sisaldab `(Shard 0-9)` jaoks ühe Kildu vahemälus täitmisel nimi mõõdud. Näiteks kui vahemälu on 3 shards, `Cache Hits` on kogu vahemälu kokkulangevuste kogusumma ja `Cache Hits (Shard 2)` on lihtsalt kokkulangevuste selle Kildu vahemälu.

Iga jälgimise diagramm kuvatakse ülataseme mõõdikute vahemälu koos mõõdikute iga vahemälu Kildu jaoks.

![Jälgimine][redis-cache-premium-monitor]

Aja libistamisel hiirega üle andmepunktide kuvab selle punkti üksikasjad. 

![Jälgimine][redis-cache-premium-point-summary]

Suuremate väärtuste on tavaliselt agregaatväärtused vahemälu väiksemad väärtused on üksikute mõõdikute jaoks on Kildu. Pange tähele, et selles näites on kolm shards ja selle Vahemälutabamusi on ühtlaselt üle shards.

![Jälgimine][redis-cache-premium-point-shard]

Kui soovite vaadata rohkem üksikasju, klõpsake diagrammi kuvamiseks laiendatud vaade **meetermõõdustik** enne.

![Jälgimine][redis-cache-premium-chart-detail]

Vaikimisi sisaldab iga diagrammi ülataseme vahemälu jõudluse näidiku kui ka üksikute shards Jõudluseloendurite. Saate kohandada need enne **Diagrammi redigeerida** .

![Jälgimine][redis-cache-premium-edit]

Hinnale saadaval jõudluse kohta leiate lisateavet teemast [saadaval mõõdikute ja aruandlus intervalle](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Toimingud ja teatised

Jaotises **toimingud** enne **Redis vahemälu** on jaotised **sündmused** ja **teatiste reeglid** .

![Oeprations][redis-cache-operations-events]

Vahemälu tehtud toimingute loendi vaatamiseks klõpsake **sündmuste** diagrammi **sündmuste** tera kuvamiseks. Tegevuste näited toomine ja regenereeriv kiirklahvide, ja aktiveerimise ja eraldusvõime teatiste reeglid. Iga sündmuse kohta lisateabe saamiseks klõpsake väärtust **sündmuste** tera.

Sündmuste kohta leiate lisateavet teemast [vaate sündmuste ja auditilogid](../monitoring-and-diagnostics/insights-debugging-with-events.md).

**Teatiste reeglite** jaotises kuvatakse vahemälu eksemplari teatiste arv. Reegli võimaldab teil jälgida oma vahemälu eksemplar ja saada e-posti, kui argumendil väärtus jõuab määratletud reegli piirmäära. 

Teatiste reeglid hinnatakse iga viie minuti ja reegli aktiveerimisel saadetakse mis tahes konfigureeritud teatised. Reegli sisselülitamise ja teatised ei töödelda hetkega; võib kuluda mitu minutit enne reegli on aktiveeritud ja saadetud teatised.

Teatiste reegleid saab vaadata ja määrata teatud jälgimisega seotud diagrammi keelest **väärtuseks meetermõõdustik** või keelest **teatiste reeglid** .

Teatiste reeglid on järgmised atribuudid.

| Reegli atribuut                 | Kirjeldus                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ressurss                            | Ressursi hinnatud reegli järgi. Reegli loomisel Redis vahemälust vahemälu on ressurss.                                                                                                                                                                                                                                                                                                                                                  |
| Nimi                                | Nimi, mis tuvastab kordumatult reegli sees vahemälu praeguse eksemplari.                                                                                                                                                                                                                                                                                                                                                                                       |
| Kirjeldus                         | Reegli valikuline kirjeldus.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Meetermõõdustik                              | Mõõdiku reegli järgi tuleb kontrollida. Vahemälu mõõdikute loendi leiate teemast saadaolevad mõõdikute ja aruandlus intervallide.                                                                                                                                                                                                                                                                                                                                             |
| Tingimus                           | Tingimuse tehtemärk reegli jaoks. Võimalikud valikud on: suurem, suurem või võrdne, vähem kui väiksem kui või võrdne                                                                                                                                                                                                                                                                                                                             |
| Lävi                           | Tehtemärgi määratud tingimusele atribuudi abil mõõdiku võrrelda kasutatava väärtuse. Sõltuvalt meetermõõdustik, see väärtus võib olla baiti sekundis baiti, % või loendus.                                                                                                                                                                                                                                                                                     |
| Perioodi kohta.                              | Saate määrata ajavahemiku üle, kus kasutatakse mõõdiku keskmise väärtuse reegli võrdlus. Näiteks kui on viimase tunni jooksul, kasutatakse keskmise väärtuse meetermõõdustik intervalli eelmise tunni võrdlus. Kui soovite teada, kui piirmäär on täidetud tegevuse tõttu on kühvli, on asjakohane lühem. Kohaletoimetamisest on pidev inimeste läve, kasutage pikemaks ajaks. |
| E-posti teenus ja koostöö administraatorid | Kui meilitsi true, teenuse administraator ja koostöö administraator teatise aktiveerimisel.                                                                                                                                                                                                                                                                                                                                                                    |
| Täiendavad administraatori e-posti      | Täiendavad administraator teavitama teatise aktiveerimisel valikuline meiliaadress.                                                                                                                                                                                                                                                                                                                                                                    |

Ainult üks saadetakse selle reegli aktiveerimise kohta. Pärast reegli ületati ja saadetakse teatis, reeglit pole uuesti hinnatakse kuni mõõdiku langeb piirmäärast. Kui mõõdiku hiljem ületab läve, teatise on uuesti aktiveerida ja Uus teatis saadetakse.

Teatiste reegleid vahemälu eksemplari puhul kuvamiseks klõpsake nuppu **teatis reeglite** osa **Redis vahemälu** tera. Ainult teatis reeglid, mis kasutavad teatud mõõdiku vaatamiseks liikuge **meetermõõdustik** tera diagrammi, mis sisaldab selle väärtuseks meetermõõdustik.

![Teatiste reeglid][redis-cache-alert-rules]

Reegli lisamiseks klõpsake tera **väärtuseks meetermõõdustik** või **teatiste reeglid** tera **teatise lisamine** . 

Sisestage soovitud reegli kriteeriumide keelde **teatise lisa** reegel ja klõpsake nuppu **OK**. 

![Reegli lisamine][redis-cache-add-alert]

>[AZURE.NOTE] Reegli loomisel, klõpsates nuppu **Lisa teatise** **meetermõõdustik** keelest kuvatakse ainult mõõdikute, kuvatakse selle tera diagrammi rippmenüüst **mõõt** . Reegli loomisel, klõpsates nuppu **Lisa teatise** keelest **teatiste reeglid** kõik vahemälu mõõdikud on saadaval rippmenüüst **mõõt** .

Pärast reegli salvestatakse see kuvatakse **teatis reeglid** enne kui ka **meetermõõdustik** enne mis tahes reeglis kasutada mõõdiku kuvatavate diagrammide jaoks. Reegli redigeerimiseks klõpsake **Reegli redigeerimine** tera kuvamiseks reegli nimi. **Reegli redigeerimine** keelest saate reegli atribuutide redigeerimine, kustutamine või keelata reeglit või uuesti lubamiseks reegel, kui see on varem keelatud.

>[AZURE.NOTE] Reegli atribuute muudatused võib mõne hetke enne **teatiste reeglid** tera või **meetermõõdustik** tera kajastuma.

Reegli aktiveerimisel saadetakse meilisõnum reegli konfiguratsioonist ja **teatiste reeglite** osa **Redis vahemälu** enne kuvatakse hoiatusikoon.

Reegli peetakse lahendada teatiste tingimus pole enam väärtuseks true. Pärast reegli tingimus on lahendatud, asendatakse hoiatuse ikooni märge. Teatiste sisselülitamise ja lahendused kohta täpsema teabe saamiseks klõpsake **sündmuste** osa **Redis vahemälu** enne **sündmuste** enne sündmuste kuvamiseks.

Azure teatiste kohta leiate lisateavet teemast [teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Enne Redis vahemälu mõõdikud

**Redis vahemälu** tera kuvab mõõdikute järgmisi.

-   [Diagrammide jälgimine](#monitoring-charts)
-   [Kasutus-diagrammid](#usage-charts)

### <a name="monitoring-charts"></a>Diagrammide jälgimine

Jaotise **jälgimis** on **tabab ja jätab**, **saab ja määrab**, **ühendused**ja **Kokku käsud** diagrammid.

![Diagrammide jälgimine][redis-cache-monitoring-part]

Kaartidel **järelevalve** kuvada järgmised mõõdikud.

| Diagrammi jälgimine | Vahemälu mõõdikud     |
|------------------|-------------------|
| Tabamust ja jätab  | Vahemälutabamusi        |
|                  | Vahemälu jätab      |
| Saab ja komplektid    | Saab              |
|                  | Komplektid              |
| Ühendused      | Ühendatud kliendid |
| Kokku käsud   | Tegevus kokku  |

Mõõdikud vaatamine ja kohandamise üksikute diagrammide selle jaotise kohta leiate teavet jaotisest järgmised [mõõdikute vaatamine ja kohandamine mõõdikute diagrammid](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Kasutus-diagrammid

**Kasutus** on **Redis serveri koormus**, **Mälukasutust**, **Võrgu ribalaius**ja **CPU hõivatus** diagrammide ja kuvab ka **hinnakirjad taseme** vahemälu eksemplari.

![Kasutus-diagrammid][redis-cache-usage-part]

Kuvatakse **hinnakirjad teise** taseme vahemälu hinnad ja saab kasutada [skaala](cache-how-to-scale.md) abil muu hinnakirjad taseme vahemälu.

**Kasutus** radiaaldiagrammil kuvatakse järgmised mõõdikud.

| Kasutus diagramm       | Vahemälu mõõdikud |
|-------------------|---------------|
| Redis serveri laadimine | Serveri laadimine   |
| Mälukasutuse      | Mällu   |
| Võrgu läbilaskevõime | Vahemälu kirjutamine   |
| CPU hõivatus         | CPU           |

Mõõdikud vaatamine ja kohandamise üksikute diagrammide selle jaotise kohta leiate teavet jaotisest järgmised [mõõdikute vaatamine ja kohandamine mõõdikute diagrammid](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



