<properties
   pageTitle="Tõrge režiimi analüüsi | Microsoft Azure'i"
   description="Juhised põhinevate Azure pilveteenuse lahenduste nurjumise režiimi analüüsi tegemine."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure'i paindlikkust juhised: tõrke režiimi analüüs

Tõrge režiimi analüüsi (FMA) on hoone paindlikkust süsteemi, tuvastades võimalike punktide süsteemi protsess. Funktsiooni FMA peaksid kuuluma arhitektuuri- ja etapid, nii, et saate koostada tõrge taastamise süsteemi algusest.

Siin on üldine protsess on FMA läbiviimiseks: 

1. Leidke kõik komponendid süsteem. Lisada välise sõltuvusi, näiteks identiteedipakkujad ja muu tootja teenused jne.   

2. Leidke iga komponendi võimalikud tõrkeid, mis võivad tekkida. Ühe komponent võib olla rohkem kui üks tõrge. Näiteks peaks võiksite lugeda tõrkeid ja kirjutage tõrkeid eraldi, kuna mõju ja võimalike kergendamise teistsugused.

3. Hinnata iga tõrge režiimi vastavalt oma üldine risk. Võtke arvesse järgmisi tegureid.  

    - Mis on selle tõenäosuse. See on üsna levinud? Extrememly harv? Te ei pea täpse arvude; eesmärk on aidata järjestada prioriteet.

    - Milline on selle mõju rakenduse, kättesaadavus, andmete kaotsimineku, rahalise kulu ja business häirete? 

4. Määratleda iga tõrge režiimi, kuidas rakenduse vastata ja taastada. Kaaluge kompromissidega kulu ja rakenduste keerukuse.   

See artikkel sisaldab teie FMA protsessi alguspunktina võimalike tõrgete laad kataloogiga ja nende kergendamise. Kataloogi on korraldatud tehnoloogia või Azure'i teenus ning üldise kategooria Rakendusetaseme kujundada. Kataloogi pole täielik, kuid hõlmab paljude Azure'i core services. 


## <a name="app-service"></a>Rakenduse teenus

### <a name="app-service-app-shuts-down"></a>Rakenduse teenuse rakendus sulgub.

**Automaattuvastus**. Võimalikud põhjused:

- Oodatud sulgemine
    
    - Tehtemärgi sulgub rakenduse; näiteks kasutamine Azure portaali.

    - Rakenduse oli maha, sest see oli jõude. (Ainult siis, kui selle `Always On` säte on keelatud.)

- Ootamatu sulgemine

    - Rakendus jookseb kokku.

    - Rakenduse teenuse VM eksemplari kättesaamatu.


Application_End logimine on jaole juba rakenduse domeeni sulgemine (pehmed protsessi krahh) ja on ainus viis rakenduse domeeni sulgemiste jõuda.

**Taastamine**

- Kui sulgemist peaks kasutada rakenduse sulgumist sündmuse nõtkelt sulgeda. Näiteks ASP.net-i, kasutage funktsiooni `Application_End` meetod.

- Kui rakendus ei maha ajal jõude, on automaatselt uuesti järgmise taotluse. Siiski kaasneb maksumus "külmkäivitus". 

- Selleks, et rakendus alla laadimata ajal jõude, lubamine on `Always On` säte web Appis. Leiate [Azure'i rakendust Service konfigureerimine web apps][app-service-configure].

- Tehtemärgi takistada sulgemise rakendus, määrake ressursi Lukusta koos `ReadOnly` tase. Vt [Lock ressursse Azure'i ressursihaldur][rm-locks].

- Kui rakendus jookseb kokku või rakenduse teenuse VM muutub pole saadaval, rakenduse teenuse taaskäivitamist automaatselt rakendus. 


**Diagnostika**. Rakenduse logide ja web logid. Vt [lubamine diagnostika logimine veebirakenduste teenuses Azure rakenduse][app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Teatud kasutaja korduvalt teeb vigased taotlused või overloads süsteem. 

**Automaattuvastus**. Kasutajate autentimiseks ja lisage kasutaja ID logid.

**Taastamine**

- [Azure'i API halduse] funktsiooni abil[ api-management] ahendamise taotlustele kasutaja. [Azure'i API haldamise pidurdamise täpsemalt] taotlemine[api-management-throttling]

- Blokeeri kasutaja.

**Diagnostika**. Logige kõik autentimist taotlusi.


### <a name="a-bad-update-was-deployed"></a>Halb värskendus on juurutatud.

**Automaattuvastus**. Azure'i portaali kaudu rakenduse seisundi jälgimine (vt [kuvari Azure web appi jõudluse][app-insights-web-apps]) või rakendada [seisund lõpp-punkti jälgimisega seotud mustri][health-endpoint-monitoring-pattern].

**Taastamine**. Kasutage mitme [juurutamine] [ app-service-slots] ja viimase hästi juurutamise tagasi pöörata. Lisateavet leiate teemast [Basic web rakenduse viide arhitektuur][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID ühenduse (OIDC) autentimine nurjub.

**Automaattuvastus**. Võimalike tõrgete tüübid on järgmised.

1. Azure AD pole saadaval või ei pääse tõttu. Ümbersuunamise autentimise lõpp-punkti ei õnnestu ja OIDC vahevara põhjustab erandi.
2. Azure AD rentniku pole olemas. Ümbersuunamise autentimise lõpp-punkti tõrkekoodi HTTP ja OIDC vahevara põhjustab erandi.
3. Kasutaja ei saa autentida. Puudub tuvastamise strateegia on vajalik; Azure AD tegeleb sisselogimise ebaõnnestumist.

**Taastamine**

1. Jaole juba töötlemata erandid on vahevara.
2. Toime `AuthenticationFailed` sündmused.
3. Kõne ümbersuunamiseks tõrke lehe kasutaja.
4. Kasutaja korduskatsed.


## <a name="azure-search"></a>Azure'i otsing

### <a name="writing-data-to-azure-search-fails"></a>Kirjutamist andmete otsimiseks Azure'i nurjub.

**Automaattuvastus**. Jaole juba `Microsoft.Rest.Azure.CloudException` tõrkeid.

**Taastamine**

[Otsingu .NET SDK] [ search-sdk] automaatselt uued katsed pärast siirdamiseks ebaõnnestumist. Kliendi SDK visanud erandid tuleks käsitleda-mööduv tõrkeid.

Proovi uuesti vaikepoliitika kasutab eksponentsiaalse tagasi välja. Poliitika eri proovi uuesti kasutada, helistage `SetRetryPolicy` klõpsake soovitud `SearchIndexClient` või `SearchServiceClient` klassi. Lisateavet leiate teemast [Automaatse uued katsed][auto-rest-client-retry].

**Diagnostika**. Kasutage [Otsingu liikluse Analytics][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Andmete lugemine Azure'i otsingust nurjub.

**Automaattuvastus**. Jaole juba `Microsoft.Rest.Azure.CloudException` tõrkeid.

**Taastamine**

[Otsingu .NET SDK] [ search-sdk] automaatselt uued katsed pärast siirdamiseks ebaõnnestumist. Kliendi SDK visanud erandid tuleks käsitleda-mööduv tõrkeid.

Proovi uuesti vaikepoliitika kasutab eksponentsiaalse tagasi välja. Poliitika eri proovi uuesti kasutada, helistage `SetRetryPolicy` klõpsake soovitud `SearchIndexClient` või `SearchServiceClient` klassi. Lisateavet leiate teemast [Automaatse uued katsed][auto-rest-client-retry].

**Diagnostika**. Kasutage [Otsingu liikluse Analytics][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Lugemise või kirjutamise sõlme nurjub.

**Automaattuvastus**. Jaole juba erand. .Net-i kliendid, tavaliselt on see `System.Web.HttpException`. Muud klient võib-olla muud tüüpi erand.  Lisateabe saamiseks vt [Cassandra vigade käsitlemise valmis paremale](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Taastamine**

- Iga [Kliendi Cassandra](https://wiki.apache.org/cassandra/ClientOptions) on uuesti poliitikaid ja võimalusi. Lisateabe saamiseks lugege teemat [Cassandra vigade käsitlemise teha õige][cassandra-error-handling].
- Sektsioon arvestada juurutus, kasutamine andmete sõlmed jaotatud domeenides viga.
- Võtta kasutusele mitme piirkonna kohaliku kvoorumi järjepidevus. -Mööduv tõrke ilmnemisel ei üle teise piirkond.

**Diagnostika**. Logid


## <a name="cloud-service"></a>Pilveteenuses

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Veebis või töötaja rollid need ootamatult suletakse.

**Automaattuvastus**. [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] sündmuse.

**Taastamine**. Alistada [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] nõtkelt puhastamiseks meetod. Lisateabe saamiseks lugege teemat [Toime Azure'i OnStop sündmuste õige viis] [ onstop-events] (ajaveebi).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Andmete lugemine DocumentDB nurjub.

**Automaattuvastus**. Jaole juba `System.Net.Http.HttpRequestException` või `Microsoft.Azure.Documents.DocumentClientException`. 

**Taastamine**

- SDK automaatselt uued katsed nurjunud katsete. Korduskatsed ja ajal suurima arvu seadmiseks konfigureerimine `ConnectionPolicy.RetryOptions`. Kliendi tõstab erandid on kas lisaks uuesti poliitika või siirdamiseks vigu pole. 

- Kui DocumentDB ahendab klient, tagastab see vea HTTP 429. Sissemöllimine tähis on selle `DocumentClientException`. Kui teil on saada tõrke 429 pidevalt, kaaluge DocumentDB saidikogumi läbilaskevõime väärtusele.

- Kahe või enama piirkondade korrata DocumentDB andmebaas. Kõigi koopiate on loetav. Kasutades kliendi SDK-d, saate määrata selle `PreferredLocations` parameeter. See on Azure piirkondade järjestatud loend. Kõik loeb saadetakse loendi esimese saadaval alale. Kui kutse nurjus, proovib kliendi muude piirkondade loendis järjestuses. Lisateabe saamiseks lugege teemat [mitme piirkonna DocumentDB kontode arendamine][docdb-multi-region].

**Diagnostika**. Logi kõik kliendi poolel.


### <a name="writing-data-to-documentdb-fails"></a>Andmete kirjutamise DocumentDB nurjub.

**Automaattuvastus**. Jaole juba `System.Net.Http.HttpRequestException` või `Microsoft.Azure.Documents.DocumentClientException`. 

**Taastamine**

- SDK automaatselt uued katsed nurjunud katsete. Korduskatsed ja ajal suurima arvu seadmiseks konfigureerimine `ConnectionPolicy.RetryOptions`. Kliendi tõstab erandid on kas lisaks uuesti poliitika või siirdamiseks vigu pole. 

- Kui DocumentDB ahendab klient, tagastab see vea HTTP 429. Sissemöllimine tähis on selle `DocumentClientException`. Kui teil on saada tõrke 429 pidevalt, kaaluge DocumentDB saidikogumi läbilaskevõime väärtusele.

- Kahe või enama piirkondade korrata DocumentDB andmebaas. Esmane piirkond nurjumisel ülendatakse teises regioonis kirjutada. Samuti võite Tõrkesiirde käsitsi käivitada. SDK ei automaatse tuvastamise ja marsruutimine, nii rakenduse koodi jätkub pärast Tõrkesiirde töötada. Tõrkesiirde jooksul (tavaliselt minutit), kirjutage toimingud on suurem latentsuse SDK leiab uue kirjutamine piirkond. Lisateabe saamiseks lugege teemat [mitme piirkonna DocumentDB kontode arendamine][docdb-multi-region].

- Mõne Fall jää püsima dokumendi varukoopia järjekorda ja töötlemine järjekorda hiljem.

**Diagnostika**. Logi kõik kliendi poolel.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Andmete lugemine Elasticsearch nurjub.

**Automaattuvastus**. Jaole juba sobiv erand konkreetse [Elasticsearch kliendi] [ elasticsearch-client] kasutusel. 

**Taastamine**

- Kasutage uuesti süsteem. Iga kliendi on uuesti poliitika. 

- Mitme Elasticsearch sõlmed juurutada ja kasutada dispersioonanalüüs kõrge kättesaadavus.

Lisateabe saamiseks lugege teemat [Azure Elasticsearch töötab][elasticsearch-azure].

**Diagnostika**. Saate kasutada vahendid Elasticsearch või logige kõik vead kliendi poolel on last. [Töötab Elasticsearch Azure]jaotisest "Jälgimine"[elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Andmete kirjutamise Elasticsearch nurjub.

**Automaattuvastus**. Jaole juba sobiv erand konkreetse [Elasticsearch kliendi] [ elasticsearch-client] kasutusel.  

**Taastamine**

- Kasutage uuesti süsteem. Iga kliendi on uuesti poliitika. 
 
- Kui rakendus saab luba vähendatud järjepidevuse taset, kaaluge kirjaliku `write_consistency` määramine `quorum`.

Lisateabe saamiseks lugege teemat [Azure Elasticsearch töötab][elasticsearch-azure].

**Diagnostika**. Saate kasutada vahendid Elasticsearch või logige kõik vead kliendi poolel on last. [Töötab Elasticsearch Azure]jaotisest "Jälgimine"[elasticsearch-azure].


## <a name="queue-storage"></a>Järjekorda salvestusruum

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Kirjutamise sõnumi süsteemne Azure'i järjekorda salvestusruumi nurjub.

**Automaattuvastus**. Pärast *N* uuestiproovimise katsest, kirjutage toiming ikkagi nurjub.

**Taastamine**

- Kohaliku vahemälu andmed talletada ja edastab kirjutab salvestusruumi hiljem, kui teenus on saadaval.
- Teisene järjekorra loomiseks ja kirjutage selle järjekorda kui esmane järjekord on saadaval.

**Diagnostika**. Kasutage [talletusruumi mõõdikute][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Rakendus ei saa töödelda kindla sõnumi järjekorda. 

**Automaattuvastus**. Kindla rakenduse. Näiteks sõnum sisaldab sobimatuid andmeid või äriloogika nurjub mingil põhjusel. 

**Taastamine**

Sõnumi teisaldamiseks eraldi järjekorda. Käivitage uurida selle järjekorras sõnumid eraldi protsessi.

Kaaluge Azure'i teenus siini sõnumside järjekorrad, mis pakub [surnud täht järjekorda] [ sb-dead-letter-queue] selleks funktsioone.

Märkus: Kui kasutate salvestusruumi järjekorrad WebJobs, WebJobs SDK pakub sisseehitatud mürki sõnumite töötlemine. Vaadake, [Kuidas kasutada Azure järjekorda salvestusruumi WebJobs SDK][sb-poison-message].

**Diagnostika**. Kasutage rakenduse logimine.


## <a name="redis-cache"></a>Redis vahemälu

### <a name="reading-from-the-cache-fails"></a>Lugemispaanil vahemälust nurjub.

**Automaattuvastus**. Jaole juba `StackExchange.Redis.RedisConnectionException`.

**Taastamine**

1. Proovige uuesti sisse siirdamiseks ebaõnnestumist. Azure'i Redis vahemälu toetab sisseehitatud uuesti, lugege teemat [vahemälu Redis uuesti juhised]läbi[redis-retry].
2. -Mööduv tõrkeid käsitleda vahemälu vahele ja jäävad tagasi andmete allikaga.

**Diagnostika**. Kasutage [Redis vahemälu diagnostika][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Vahemällu kirjutamist nurjub.

**Automaattuvastus**. Jaole juba `StackExchange.Redis.RedisConnectionException`.

**Taastamine**

1. Proovige uuesti sisse siirdamiseks ebaõnnestumist. Azure'i Redis vahemälu toetab sisseehitatud uuesti, lugege teemat [vahemälu Redis uuesti juhised]läbi[redis-retry].
2. Kui tõrke on-mööduv, ignoreerida ja lasta muude toimingute kirjutada vahemällu hiljem.

**Diagnostika**. Kasutage [Redis vahemälu diagnostika][redis-monitor].


## <a name="sql-database"></a>SQL-andmebaas

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Andmebaasi esmane piirkonna ei saa ühendust luua.

**Automaattuvastus**. Nurjub.

**Taastamine**

Nõutav: Andmebaasi peab olema konfigureeritud aktiivse geo-kopeerimine. Vt [SQL-i andmebaasi aktiivse Geo kopeerimine][sql-db-replication].

- Päringute lugeda teise koopia. 
- Lisab ja värskendused, käsitsi ei õnnestu üle teise koopia. Leiate [Azure'i SQL-andmebaasi jaoks planeeritud või planeerimata Tõrkesiirde algatada][sql-db-failover].

Koopia kasutab eri ühendusstring, seega on vaja värskendada oma rakenduse ühendusstring.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Kliendi otsa ühenduse pargis ühendused.

**Automaattuvastus**. Jaole juba `System.InvalidOperationException` tõrkeid. 

**Taastamine**

- Proovige uuesti.
- Vähendamiseks lepingut, mis teostavad ühenduse kaustu iga puhul kasutada nii, et ühel kasutamise korral ei saa domineerivad kõik ühendused.
- Suurim lubatud ühenduse kaustu suurendamine

**Diagnostika**. Rakenduse logid.


### <a name="database-connection-limit-is-reached"></a>Andmebaasi ühenduse piirangu. 

**Automaattuvastus**. Azure'i SQL-andmebaasi samaaegseid töötajate, sisselogimise ja seansid arv on piiratud. Piiranguid sõltuvad teenuse kiht. Lisateavet leiate [Azure'i SQL-andmebaasi ressursi piirangud][sql-db-limits].

Nende vigade tuvastamiseks jaole juba `System.Data.SqlClient.SqlException` ja märkige ruut väärtus `SqlException.Number` SQL-i tõrkekood. Oluline tõrkekoodid loendi leiate teemast [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide][sql-db-errors].

**Taastamine**. Nende vigade käsitletakse siirdamiseks, et proovitakse uuesti võib probleemi lahendada. Kui vajutate korduvalt tõrkeid, kaaluge skaleerimist andmebaas.

**Diagnostika**. - [Sys.event_log] [ sys.event_log] päring tagastab tupikuid eduka andmebaasi ühendused ja ühenduse tõrkeid.

- [Reegli] loomine[ azure-alerts] jaoks nurjus ühendused.

- Luba [auditi SQL-andmebaasi] [ sql-db-audit] ja otsimine nurjunud sisselogimise.


## <a name="service-bus-messaging"></a>Teenuse siini sõnumside

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Sõnumi lugemine teenuse siini järjekorda nurjub.

**Automaattuvastus**. Jaole juba kliendi SDK erandid. Base klassi jaoks teenuse siini erandid on [MessagingException][sb-messagingexception-class]. Kui tõrke on ajutine, on `IsTransient` atribuut on täidetud. 

Lisateavet leiate teemast [teenuse siini sõnumside erandid][sb-messaging-exceptions].

**Taastamine**

1. Proovige uuesti sisse siirdamiseks ebaõnnestumist. [Teenuse siini proovige juhiseid]teemast[sb-retry].

2. Sõnumid, mis tahes vastuvõtja ei saa toimetada paigutatakse *surnud täht järjekorda*. Selle järjekorra abil saate vaadata, milliseid sõnumeid ei saanud vastu võtta. Ilma automaatse Kettapuhastus surnud täht järjekorra on. Sõnumite jäävad kuni konkreetselt taastada. Vt [Ülevaade teenuse siini surnud täht järjekorrad][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Sõnumi koostamine teenuse siini järjekord nurjub.

**Automaattuvastus**. Jaole juba kliendi SDK erandid. Base klassi jaoks teenuse siini erandid on [MessagingException][sb-messagingexception-class]. Kui tõrke on ajutine, on `IsTransient` atribuut on täidetud. 

Lisateavet leiate teemast [teenuse siini sõnumside erandid][sb-messaging-exceptions].

**Taastamine**

1. Kliendi teenuse siini automaatselt uued katsed pärast siirdamiseks vigu. Vaikimisi kasutab eksponentsiaalse tagasi välja. Pärast uuesti maksimaalne arv või maksimaalne aja, põhjustab kliendi erandi. Lisateabe saamiseks vt [Teenuse siini proovige juhiseid][sb-retry].

2. Kui järjekorda kvoodi ületamise korral kliendi põhjustab [QuotaExceededException][QuotaExceededException]. Erandi teade annab rohkem üksikasju. Tühjendada mõned sõnumid järjekorda proovimiseks ja kaaluge lüliti mustri vältida jätkumise korduskatsed, kui selle on ületatud. Lisaks veenduge, et [BrokeredMessage.TimeToLive] atribuut on seatud liiga suur. 

3. Piirkonna, saab parandada paindlikkust [sektsioonitud järjekorrad või teemade]abil[sb-partition]. Liigendatud järjekorda või teema on määratud üks sõnumside poe. Kui see sõnumside poe pole saadaval, ei õnnestu kõik toimingud, et järjekorda või teema. Sektsioonitud järjekorda või teema on liigendatud üle mitme kiirsõnumside poed. 

4. Täiendavad paindlikkust, luua kahte teenuse siini nimeruumi eri regioonide ja sõnumite korrata. Saate aktiivse kopeerimine või passiivne kopeerimine.

    - Aktiivse kopeerimine: kliendi saadab iga sõnumi nii järjekorrad. Vastuvõtja kuulab mõlemad järjekorrad. Sildistamine sõnumid, mille kordumatu, nii saate kliendi Hülga eksemplaris sõnumeid.

    - Passiivne dispersioonanalüüs: klient saadab sõnumi ühe järjekorda. Kui ilmneb tõrge, kliendi kuulub tagasi muude järjekorda. Vastuvõtja kuulab mõlemad järjekorrad. Seda moodust vähendab dubleeritud saadetavate sõnumite arv. Siiski vastuvõtja peate endiselt toime eksemplaris sõnumeid.

    Lisateavet leiate teemast [GeoReplication valimi] [ sb-georeplication-sample] [head]tavad isoleeriva rakenduste teenuse siini katkestuste eest katastroofide[sb-outages].


### <a name="duplicate-message"></a>Sõnumite.

**Automaattuvastus**. Uurida funktsiooni `MessageId` ja `DeliveryCount` sõnumi atribuute.

**Taastamine**

- Võimaluse korral kujundada töötleb sõnum olla idempotent. Muul juhul talletada sõnumid, mis on juba töödeldud ja märkige ruut ID enne sõnumi töötlemine sõnumi ID-d.

- Luba duplikaatide tuvastamine, luues järjekorda vastavalt `RequiresDuplicateDetection` väärtuseks true. Selle sätte teenuse siini automaatselt kustutab kõik sõnumid, mis saadetakse koos sama `MessageId` eelmise sõnumina.  Võtke arvesse järgmist.

    -  See säte takistab dubleeritud sõnumite pannakse järjekorda. See ei takista vastuvõtja sama sõnumi töötlemine rohkem kui üks kord.

    - Duplikaatide tuvastamine on aja aken. Kui duplikaat saadetakse Lisaks selles aknas, seda ei tuvastanud.

**Diagnostika**. Logige dubleeritakse sõnumeid.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Rakendus ei saa töödelda kindla sõnumi järjekorda. 

**Automaattuvastus**. Kindla rakenduse. Näiteks sõnum sisaldab sobimatuid andmeid või äriloogika nurjub mingil põhjusel. 

**Taastamine**

Millega tuleks arvestada on kaks tõrge. 

- Vastuvõtja tuvastab selle. Sel juhul sõnumi teisaldamiseks surnud täht järjekorda. Hiljem, käivitage uurida surnud täht kuhjuda sõnumid eraldi protsessi.

- Vastuvõtja nurjub keskel sõnumi töötlemine &mdash; näiteks töötlemata erandi tõttu. Sel juhul käsitlema kasutada `PeekLock` režiim. Selles režiimis kui lukustus aegub, sõnumi muutub kättesaadavaks muud vastuvõtjad. Kui sõnum ületab maksimaalne arv või aeg-to-live, automaatselt sõnum surnud täht järjekorda.

Lisateabe saamiseks vt [teenuse siini ülevaade surnud täht järjekorrad][sb-dead-letter-queue].

**Diagnostika**. Iga kord, kui rakenduse liigub sõnumi surnud täht järjekorda, kirjutage sündmuse rakenduse logid.


## <a name="service-fabric"></a>Teenuse struktuuri

### <a name="a-request-to-a-service-fails"></a>Taotluse loomine nurjus.

**Automaattuvastus**. Teenus tagastab tõrke.

**Taastamine**

- Leidke proxy uuesti (`ServiceProxy` või `ActorProxy`) ja teenuse/näitleja meetodit uuesti kutsuda. 

- **Stateful teenus**. Tehingu murdmiseks usaldusväärne saidikogumid toiminguid. Kui ilmneb tõrge, kuvatakse tehingu tagasi pöörata. Kutse, kui tõmmatakse järjekorda, töödeldakse uuesti. 
 
- **Kodakondsuseta teenus**. Kui teenus ei lahene välise poest andmed, kõik toimingud peavad olema idempotent.


**Diagnostika**. Rakenduse logi

### <a name="service-fabric-node-is-shut-down"></a>Teenuse struktuuri sõlm on välja lülitatud.

**Automaattuvastus**. Loobumise luba edastatakse teenuse `RunAsync` meetod. Teenuse struktuuri tühistab enne sulgemist sõlme tööülesanne.

**Taastamine**. Loobumise luba abil saate tuvastada sulgumist. Kui teenuse struktuuri taotleb tühistamine, valmis töö ja sulgege `RunAsync` nii kiiresti kui võimalik.

**Diagnostika**. Logid


## <a name="storage"></a>Salvestusruumi

### <a name="writing-data-to-azure-storage-fails"></a>Azure Storage kirjutamist andmed nurjub

**Automaattuvastus**. Tõrgete saab klient sisestamisel.

**Taastamine**

1. Proovige uuesti, taastada siirdamiseks ebaõnnestumist. [Proovige poliitika] [ Storage.RetryPolicies] klientrakenduses SDK tegeleb see automaatselt.
2. Lüliti mustri vältimiseks suur salvestusruumi rakendada.
3. Kui N uuesti katsed teha graatsiline Fall. Näiteks:

    - Kohaliku vahemälu andmed talletada ja edastab kirjutab salvestusruumi hiljem, kui teenus on saadaval.
    - Kui kirjutage toiming oli selgituseks ulatus, kompenseeri tehingu.

**Diagnostika**. Kasutage [talletusruumi mõõdikute][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Andmete lugemine Azure Storage nurjub.

**Automaattuvastus**. Lugemise ajal saab klient tõrkeid.

**Taastamine**

1. Proovige uuesti, taastada siirdamiseks ebaõnnestumist. [Proovige poliitika] [ Storage.RetryPolicies] klientrakenduses SDK tegeleb see automaatselt.
2. RA-GRS Storage, kui lugemine esmane lõpp-punkti nurjub, proovige lugemine teisene lõpp-punkti. Kliendi SDK võib käsitleda seda automaatselt. Teemast [Azure Storage dispersioonanalüüs][storage-replication].
3. Kui *N* uuesti katsed võtta taandepäringud toimingu nõtkelt näevad. Näiteks kui pilti ei saa taastada salvestusruumi, kuvada üldise kohatäitepilt.

**Diagnostika**. Kasutage [talletusruumi mõõdikute][storage-metrics].


## <a name="virtual-machine"></a>Virtuaalse masina

### <a name="connection-to-a-backend-vm-fails"></a>Mõne taustväärtus VM nurjub.

**Automaattuvastus**. Võrgu ühenduse tõrkeid.

**Taastamine**

- Vähemalt kaks taustväärtus VMs kättesaadavus komplekti, taha laadi koormusetasakaalustusteenuse juurutamine.

- Kui ühendus tõrke on ajutine, mõnikord TCP edukalt proovib saata sõnum. 

- Proovi uuesti poliitika rakendada rakenduse. 

- Püsivate või -mööduv tõrkeid, rakendada [lüliti] [ circuit-breaker] mustri.

- Kui helistaja VM ületab oma võrgu sealt, täidab väljamineva järjekorra. Kui väljamineva järjekorra on pidevalt täis, kaaluge skaleerimist välja. 

**Diagnostika**. Sündmuste logi teenuse piirmäärad juures.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>VM eksemplari muutub saadaval või on vigane.

**Automaattuvastus**. Laadi koormusetasakaalustusteenuse [seisundi juures] konfigureerimine[ lb-probe] mis annab märku, kas VM eksemplari on terve. Selle juures peab kontrollima, kas kriitilised funktsioonid vasta õigesti. 

**Taastamine**. Rakenduse iga taseme, pannakse kättesaadavus samu VM mitmes eksemplaris, ja paigutada laadi koormusetasakaalustusteenuse ette VMs. Kui seisundi juures nurjub, peatab laadimise koormusetasakaalustusteenuse saatmist uued ühendused vigane eksemplar.

**Diagnostika**. – Kasutage laadi koormusetasakaalustusteenuse [log analytics][lb-monitor].
- Häälestada jälgimine süsteemi jälgida kõiki seisundi jälgimine lõpp-punktid.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Tehtemärk kogemata sulgub VM.

**Automaattuvastus**. N/A

**Taastamine**. Ressursi Lukusta abil määrata `ReadOnly` tase. Vt [Lock ressursse Azure'i ressursihaldur][rm-locks].

**Diagnostika**. Kasutage [Azure tegevuse logib][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Pidev töö seiskub jõude SCM host korral.

**Automaattuvastus**. Funktsiooni WebJob loobumise luba edastada. Lisateavet leiate teemast [graatsiline sulgumist][web-jobs-shutdown]. 

**Taastamine**. Luba selle `Always On` säte web Appis. Lisateabe saamiseks lugege teemat [taustal käitada ülesannete WebJobs][web-jobs].


## <a name="application-design"></a>Rakenduse kujundus

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Rakendus ei oska on kühvli sissetulevad taotlused sisse.

**Automaattuvastus**. Sõltub rakendusest. Tüüpilised sümptomid:

- Veebisaidi käivitatakse HTTP 5xx tõrkekoodid esitus.
- Sõltuvad teenused, nt andmebaasi või salvestusruumi, alustage throttle taotlused. Vaadake, kas HTTP tõrkeid, nagu HTTP 429 (liiga palju taotlusi), sõltuvalt teenuse.
- HTTP järjekorra pikkus kasvab.

**Taastamine**

- Koormuse käsitlema skaala välja. 

- Leevendada tõrkeid, et vältida olukorda, kus kaskaadlaadistiku häirida kogu rakenduse tõrked. Järgmised vähendamiseks strateegiad.

    - Rakendada [Mustreid ahendamise] [ throttling-pattern] vältimiseks suur taustväärtus süsteemid.
    - Kasutada [järjekorda vastavalt laadi ühtlustamise] [ queue-based-load-leveling] puhvri taotlusi ja töödelda õiges tempos.
    - Tähtsuse järjekorda seada teatud kliendid. Näiteks kui rakendus on tasuta ja tasulisi astme, throttle tasuta taseme klientidele, kuid pole tasulist kliendid. Lugege teemat [prioriteet järjekorda mustri][priority-queue-pattern].

**Diagnostika**. Kasutage [rakendust Service diagnostika logimine][app-service-logging]. Kasutada teenust, näiteks [Azure'i Log Analytics][azure-log-analytics], [Rakenduse ülevaated][app-insights], või [Uue reliikvia] [ new-relic] mõistmiseks diagnostika logid.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Üks töövoog jagatud toimingu või toimingute nurjub.

**Automaattuvastus**. Pärast *N* uuestiproovimise katsest, seda ei saa ikka.

**Taastamine**

- Vähendamiseks plaan, rakendada [Scheduler Agent halduri] [ scheduler-agent-supervisor] mustri kogu töövoo haldamiseks. 
- Ärge uuesti sisse ajalõpud. On madal edukust selle tõrke. 
- Järjekorda töös, proovige hiljem uuesti.

**Diagnostika**. Logige kõik toimingud (õnnestunud ja nurjunud), sh kompensatsioonitoodete toimingud. Kasutada korrelatsiooni ID-d, nii et saate jälgida kõiki toiminguid sama tehingust.


### <a name="a-call-to-a-remote-service-fails"></a>Kõne kaugjuhtimise teenuse nurjub.

**Automaattuvastus**. HTTP tõrkekoodi.

**Taastamine**

1. Proovige uuesti sisse siirdamiseks ebaõnnestumist. 
2. Kui kõne nurjub pärast *N* avaldab, taandepäringud toimingu tegemine. (Rakenduse teatud.)
3. [Lüliti mustri] rakendada[ circuit-breaker] kaskaadlaadistiku tõrgete vältimiseks. 

**Diagnostika**. Logige sisse kõigi kõnede kaugjuhtimine ebaõnnestumist.


## <a name="next-steps"></a>Järgmised sammud

FMA protsessi kohta leiate lisateavet teemast [paindlikkuse põhimõttel puhul pilveteenuste] [ resilience-by-design-pdf] (PDF-faili alla laadida).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

