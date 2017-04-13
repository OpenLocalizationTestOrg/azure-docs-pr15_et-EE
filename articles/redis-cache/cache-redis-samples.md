<properties 
    pageTitle="Azure'i Redis vahemälu näidised | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure Redis vahemälu" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure'i Redis vahemälu näidised 

Selles teemas antakse Azure'i Redis vahemälu näidised, mis hõlmab stsenaariumid, nt vahemälu ühenduse, lugemine ja kirjutamine ja sealt vahemälu andmete ja ASP.net-i vahemälu Redis pakkujate loendit. Mõned näidised on allalaaditav projektide ja mõned üksikasjalikke suuniseid ja kaasata Koodilõigud, kuid ärge link allalaaditavad projekti.

## <a name="hello-world-samples"></a>Tere, maailm näidised

Selles jaotises näidised kuvatakse ühenduse näiteks Azure'i Redis vahemälu ja lugemise ja kirjutamise andmete vahemällu erinevate keelte kasutamise põhialused ja Redis kliendid.

[Tere, maailm](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) valimi näitab, kuidas sooritada erinevaid toiminguid vahemälu, kasutades [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .net-i klient.

See näide kirjeldab, kuidas:

-   Ühenduse mitmesuguste suvandite kasutamine
-   Lugeda ja kirjutada objektid ja kasutamise toimingute sünkroonse ja asünkroonse vahemälust
-   Tagastab väärtuste määratud klahvid Redis MGET/MSET käskude abil
-   Redis selgituseks toiminguid
-   Töötamine Redis loendid ja sorditud väärtuse.
-   .Net-i objektide abil JsonConvert sarjadesse jaotaja talletamine
-   Kasutage Redis määrab rakendada sildistamine
-   Töötamine Redis kobar

Lisateabe saamiseks [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentatsiooni github ja lugege rohkem kasutamise stsenaariumid [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) ühiku testide.

[Azure'i Redis vahemälu Python kasutamine](cache-python-get-started.md) näitab, kuidas alustada Azure'i Redis vahemälu Python ja [redis-py](https://github.com/andymccurdy/redis-py) kliendi abil.

[.Net-i vahemälu objektidega töötamine](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) kuvatakse üks võimalus serialiseerida .NET objektid nii, et saate neid kirjutada ja lugeda nende Azure'i Redis vahemälu eksemplar. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>ASP.net-i SignalR kasutada välja Backplane skaala Redis vahemälu

[Kasutage Redis vahemälu skaala Backplane ASP.net-i SignalR jaoks välja nagu](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) valimi näitab, kuidas saate kasutada Azure Redis vahemälu SignalR backplane. Backplane kohta leiate lisateavet teemast [SignalR Scaleout koos Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis vahemälu kliendi päringu näidis

See näide näitab võrdleb jõudluse juurdepääsu andmeid vahemälu ja juurdepääsu andmeid püsimine salvestusruumi vahel. See näide on kaks projekti.

-   [Kuidas Redis vahemälu saate jõudluse parandamiseks andmete vahemällu demo](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Demo seemne andmebaas ja vahemälu](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.net-i seansi olek ja väljundi vahemällu talletamine

[Kasutada Azure Redis vahemälu talletamiseks ASP.net-i SessionState ja OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) valimi näitab, kuidas kasutada Azure Redis vahemälu talletamiseks ASP.net-i seansi ja väljundi vahemälu Redis SessionState ja OutputCache pakkujate kasutamise.

## <a name="manage-azure-redis-cache-with-maml"></a>Azure'i Redis vahemälu MAML ja haldamine

[Azure'i haldamine teekide haldamine Azure'i Redis vahemälu kasutamine](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) valimi näitab, kuidas saate kasutada Azure haldamine teekide haldamine – (loomine ja värskendamine ja kustutamine) vahemälu. 

## <a name="custom-monitoring-sample"></a>Kohandatud valimi jälgimine

[Accessi Redis vahemälu jälgimise andmete](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) näidis näitab, kuidas pääsete andmeid Azure'i Redis vahemälu väljaspool Azure'i portaal.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Twitteri laadis klooni, kirjutada, kasutades PHP ja Redis

[Retwis](https://github.com/SyntaxC4-MSFT/retwis) valim on selle Redis Tere, maailm. See on minimaalne Twitteri laadis suhtlusvõrgu klooni kirjutada, kasutades Redis ja PHP, kasutades [Predis](https://github.com/nrk/predis) klient. Lähtekoodi eesmärk on väga lihtne ja samal ajal näidata erinevate Redis andmestruktuurid.

## <a name="bandwidth-monitor"></a>Läbilaskevõime jälgimine

Valimi [läbilaskevõime jälgimine](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) võimaldab teil jälgida kliendi kasutatavat ribalaiust. Mõõtke läbilaskevõime, käivitada valimi vahemälu klientarvutis, helistada vahemälu ja jälgida läbilaskevõime, mis on esitatud läbilaskevõime kuvari valimi.
