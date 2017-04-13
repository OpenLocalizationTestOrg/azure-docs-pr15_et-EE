<properties
    pageTitle="DocumentDB Node.js API ja SDK | Microsoft Azure'i"
    description="Lisateavet kõigi Node.js API ja SDK, sh avaldamise kuupäevad, aegumine kuupäevade ja iga versiooni DocumentDB Node.js SDK vahel tehtud muudatused."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-d ja SDK-d

> [AZURE.SELECTOR]
- [.NET-I](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ÜLEJÄÄNUD](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL-I](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API ja SDK

<table>
<tr><td>**SDK allalaadimine**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API dokumentatsioon**</td><td>[Node.js API dokumentides](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK installimisjuhiseid.**</td><td>[Installimisjuhised](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Kaasa SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Näidised**</td><td>[Node.js koodinäiteid](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Hangi alustamise õpetus**</td><td>[Node.js SDK kasutamise alustamine](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web Appi õpetus**</td><td>[Node.js veebirakenduse abil DocumentDB koostamine](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Praeguse toetatud platvormi**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Väljalaskemärkmed

###<a name="1.10.0"/>1.10.0</a>

- Lisatud tugi risti partition paralleelselt päringuid.
- Lisatud tugi ülemine/ORDER BY päringute sektsioonitud saidikogumid.

###<a name="1.9.0"/>1.9.0</a>

- Lisatud uuesti poliitika tugi rakendus taotlused. (Rakendus taotlused saada taotlus määr liiga suur erand, tõrkekood 429.) Vaikimisi DocumentDB uued katsed üheksa korda iga taotlus tõrkekood 429 ilmnemisel austus vastuse päises retryAfter aeg. Fikseeritud uuesti intervalli aeg saate nüüd määrata atribuudi RetryOptions osana ConnectionPolicy objektil, kui soovite ignoreerida retryAfter aeg ning korduste vahel serveri poolt tagastatud. DocumentDB nüüd ootab iga 30 sekundi taotlemine, mis on on rakendus (sõltumata uuesti arv) ja tagastab tõrkekood 429 vastus. Praegu saab ka overriden RetryOptions atribuudi ConnectionPolicy objekti.

- DocumentDB nüüd tagastab x-ms-ahendamise-proovi uuesti-count ja x-ms-throttle-retry-wait-time-ms nagu vastus päised on ahendamise tähistamiseks iga taotluse proovige count ja cummulative aja taotluse oodanud ning korduste vahel.

- Lisati RetryOptions ainekursust, asetades atribuudi RetryOptions alistamiseks osa uuesti vaikesuvandite kasutatavate ConnectionPolicy ainekursust.

###<a name="1.8.0"/>1.8.0</a>

 - Lisatud tugi mitme piirkonna andmebaasi kontod.

###<a name="1.7.0"/>1.7.0</a>

- On aeg, et Live(TTL) funktsioon dokumentide tugi.

###<a name="1.6.0"/>1.6.0</a>

- Rakendatud [sektsioonitud saidikogumite](documentdb-partition-data.md) ja [kasutaja määratletud jõudlust](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- Fikseeritud RangePartitionResolver.resolveForRead vea, kui see oli tagasi lingid vigased concat tulemuste tõttu.

###<a name="1.5.5"/>1.5.5</a>

- Fikseeritud hashParitionResolver resolveForRead(): kui on esitatud sektsiooni võti korrutamine erandi kõik registreeritud linkide loend sisaldamise asemel.

###<a name="1.5.4"/>1.5.4</a>

- Paranduste probleemi [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - spetsiaalne HTTPS Agent: vältida muutmine globaalne agent DocumentDB eesmärgil. Kasutage sihtotstarbeline agent kõigi selle teegi taotlused.

###<a name="1.5.3"/>1.5.3</a>

- Paranduste probleemi [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - õigesti pideme kriipsud meediumi ID-d.

###<a name="1.5.2"/>1.5.2</a>

- Paranduste probleemi [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter kuulajale lekkima hoiatus.

###<a name="1.5.1"/>1.5.1</a>

- Paranduste probleemi [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - Muuda kausta räsi räsi tõstutundlik süsteemid.

### <a name="1.5.0"/>1.5.0</a>

- Rakendada sharding tugi räsi & vahemiku partition selsüünid lisamisega.

### <a name="1.4.0"/>1.4.0</a>

- Rakendada Upsert. Uue upsertXXX meetodite documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Tuua versioonide numbrid muud SDK-d vastavusseviimine vahele.

### <a name="1.2.2"/>1.2.2</a>

- Tükeldatud k lubab ümbris uue hoidlasse.
- Pakettfaili npm registri värskendamine

### <a name="1.2.1"/>1.2.1</a>

- Rakendab ID vastavalt marsruutimist.
- Parandab probleemi [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - praeguse atribuudi meetodi current() konflikte.

### <a name="1.2.0"/>1.2.0</a>

- Lisatud tugi ruumilisel register.
- Kinnitatakse id atribuudi kõik ressursid. Ressursside ID-d ei tohi sisaldada?, /, #, & #47; & #47; märkide või end-klahv tühik.
- Lisab uue päise "index teisendus edu" ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>

- Rakendab V2 indekseerimise.

### <a name="1.0.3"/>1.0.3</a>

- Probleemi [#40] (https://github.com/Azure/azure-documentdb-node/issues/40) – rakendatud eslint ja grunt konfiguratsioone core ja lubadus SDK.

### <a name="1.0.2"/>1.0.2</a>

- Probleem [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - lubadusi ümbris ei sisalda päise tõrkega.

### <a name="1.0.1"/>1.0.1</a>

- Rakendatud võimalus päringu konfliktide readConflicts, readConflictAsync ja queryConflicts lisamisega.
- Värskendatud API dokumentatsiooni.
- Probleemi [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync tõrge.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Väljalaske- ja aegumine kuupäevi
Microsoft annab teatis vähemalt **12 kuud** enne pensionile ka SDK, et sujuva ülemineku/toetatud uuema versiooni.

Uued funktsioonid ja funktsionaalsus ja Optimeerimised lisatakse ainult praegune SDK, nagu näiteks see on soovitame alati uuendada uusimale versioonile SDK juba võimalikult.

Mis tahes taotluse DocumentDB endine SDK abil on tagasi lükatud teenus.

> [AZURE.WARNING]
Kõik versioonid Azure DocumentDB SDK jaoks Node.js enne versioon **1.0.0** pensionile **29 veebruar**2016.

<br/>

| Versioon | Väljaandmisaeg | Aegumine kuupäev
| ---     | ---          | ---
| [1.10.0](#1.10.0) | Oktoober 03, 2016 |---
| [1.9.0](#1.9.0) | Juuli 07, 2016 |---
| [1.8.0](#1.8.0) | 14 juuni 2016 |---
| [1.7.0](#1.7.0) | 26 aprill 2016 |---
| [1.6.0](#1.6.0) | 29 märts 2016 |---
| [1.5.6](#1.5.6) | 08 märts 2016 |---
| [1.5.5](#1.5.5) | 02 veebruar 2016 |---
| [1.5.4](#1.5.4) | 01 veebruar 2016 |---
| [1.5.2](#1.5.2) | 26 jaanuar 2016 |---
| [1.5.2](#1.5.2) | 22 Jaanuar 2016 |---
| [1.5.1](#1.5.1) | 4 Jaanuar 2016 |---
| [1.5.0](#1.5.0) | 31 Detsember 2015 |---
| [1.4.0](#1.4.0) | 06 Oktoober 2015 |---
| [1.3.0](#1.3.0) | 06 Oktoober 2015 |---
| [1.2.2](#1.2.2) | 10 mai 2015 |---
| [1.2.1](#1.2.1) | 15 august 2015 |---
| [1.2.0](#1.2.0) | 05 August 2015 |---
| [1.1.0](#1.1.0) | Juuli 09, 2015 |---
| [1.0.3](#1.0.3) | 04 juuni 2015 |---
| [1.0.2](#1.0.2) | 23 mai 2015 |---
| [1.0.1](#1.0.1) | 15 mai 2015 |---
| [1.0.0](#1.0.0) | 08 aprill 2015 |---
| 0.9.4-prerelease | 06 aprill 2015 | 29 veebruar 2016
| 0.9.3-prerelease | 14 jaanuar 2015 | 29 veebruar 2016
| 0.9.2-prerelease | 18 detsembrini 2014 | 29 veebruar 2016
| 0.9.1-prerelease | 22 august 2014 | 29 veebruar 2016
| 0.9.0-prerelease | 21 August 2014 | 29 veebruar 2016


## <a name="faq"></a>KKK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vt ka

DocumentDB kohta leiate lisateavet teemast [Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) teenus.
