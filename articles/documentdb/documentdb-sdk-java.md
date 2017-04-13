
<properties
    pageTitle="DocumentDB Java API ja SDK | Microsoft Azure'i"
    description="Lisateavet kõigi Java API ja SDK, sh avaldamise kuupäevad, aegumine kuupäevade ja iga versiooni DocumentDB Java SDK vahel tehtud muudatused."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-d ja SDK-d

> [AZURE.SELECTOR]
- [.NET-I](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ÜLEJÄÄNUD](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL-I](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API ja SDK

<table>
<tr><td>**SDK allalaadimine**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API dokumentatsioon**</td><td>[Java API dokumentides](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Kaasa SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Alustamine**</td><td>[Java SDK kasutamise alustamine](documentdb-java-application.md)</td></tr>
<tr><td>**Praeguse toetatud käitusaja**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Väljalaskemärkmed

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Lisatud tugi BoundedStaleness järjepidevuse tase.
  - Lisatud tugi CRUD toimingute jaoks sektsioonitud saidikogumite otsese Ühenduvus.
  - Fikseeritud vea päringu SQL-i andmebaasi.
  - Fikseeritud vea, kus seansi luba võib olla valesti määratud seansi vahemälu.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Lisatud tugi rist partition paralleelselt päringuid.
  - Lisatud tugi ülemine/ORDER BY päringute sektsioonitud saidikogumid.
  - Lisatud tugi tugev järjepidevuse.
  - Lisatud tugi nime vastavalt taotlusi otsese ühenduvuse kasutamisel.
  - Veenduge, et püsida ühtsete üle kõik taotluse korduskatsed ActivityId kinnitatud.
  - Fikseeritud vea seotud seansi vahemälu kui taasloomine kogumi sama nimega.
  - Lisatud Hulknurk ja täpsustades saidikogumi poliitika geo hoides ruumilise päringuid indekseerimine LineString andmetüübid.
  - Java dokumendi Java 1.8 fikseeritud probleeme.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Fikseeritud vea PartitionKeyDefinitionMap vahemälu ühe sektsiooni saidikogumite ja tee eest toomise partition võtme taotlused.
  - Fikseeritud vea pole uuesti, kui väärtus on vale sektsiooni.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Lisatud tugi mitme piirkonna andmebaasi kontod.
  - Lisatud tugi automaatse võimalusi kohandada max proovi uuesti üritab ja max uuesti ootamine aeg taotluste rakendus uuesti.  Vt RetryOptions ja ConnectionPolicy.getRetryOptions().
  - Taunitud IPartitionResolver vastavalt kohandatud eraldamine koodi. Kasutage sektsioonitud saidikogumid suuremad salvestusruumi ja läbilaskevõime.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Lisatud uuesti poliitika tugi pidurdamise.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Lisatud aeg live (TTL) tugiteenuste dokumendid.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Rakendatud [sektsioonitud saidikogumite](documentdb-partition-data.md) ja [kasutaja määratletud jõudlust](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Fikseeritud vea HashPartitionResolver räsi väärtused vähe ees oleks kooskõlas muude SDK-sid luua.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Lisa räsi & vahemiku partition selsüünid abistamiseks üle mitme sektsioonid sharding rakendustega.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Rakendada Upsert. Uute upsertXXX meetodite lisatud Upsert funktsiooni toetamiseks.
- Rakendada ID, mis põhineb marsruutimist. Avaliku API muudatusi, kõik muudatused sisemine.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Väljalaske vahele tuua versiooninumber vastavusseviimine muude SDK-d

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Toetab ruumilisel Index
- Kinnitatakse id atribuudi kõik ressursid. Ressursside ID-d ei tohi sisaldada ?, /, #, \, märki või end-klahv tühik.
- Lisab uue päise "index teisendus edu" ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Rakendab V2 indekseerimine

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Väljalaske- ja aegumine kuupäevad
Microsoft annab teatis vähemalt **12 kuud** enne pensionile ka SDK, et sujuva ülemineku/toetatud uuema versiooni.

Uued funktsioonid ja funktsionaalsus ja Optimeerimised lisatakse ainult praegune SDK, nagu näiteks see on soovitame alati uuendada uusimale versioonile SDK juba võimalikult.

Mis tahes taotluse DocumentDB endine SDK abil on tagasi lükatud teenus.

> [AZURE.WARNING]
Kõik versioonid Azure DocumentDB SDK Java versioon **1.0.0** enne pensionile **29 veebruar**2016.

<br/>

| Versioon | Väljaandmisaeg | Aegumine kuupäev
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 oktoober 2016 |---
| [1.9.0](#1.9.0) | Oktoober 03, 2016 |---
| [1.8.1](#1.8.1) | 30 juuni 2016 |---
| [1.8.0](#1.8.0) | 14 juuni 2016 |---
| [1.7.1](#1.7.1) | 30 aprill 2016 |---
| [1.7.0](#1.7.0) | 27 aprill 2016 |---
| [1.6.0](#1.6.0) | 29 märts 2016 |---
| [1.5.1](#1.5.1) | 31 Detsember 2015 |---
| [1.5.0](#1.5.0) | 04 detsember 2015 |---
| [1.4.0](#1.4.0) | 05 oktoober 2015 |---
| [1.3.0](#1.3.0) | 05 oktoober 2015 |---
| [1.2.0](#1.2.0) | 05 August 2015 |---
| [1.1.0](#1.1.0) | Juuli 09, 2015 |---
| [1.0.1](#1.0.1) | 12 mai 2015 |---
| [1.0.0](#1.0.0) | 07 aprillis 2015 |---
| 0.9.5-prelease | 09 märts 2015 | 29 veebruar 2016
| 0.9.4-prelease | 17 veebruar 2015 | 29 veebruar 2016
| 0.9.3-prelease | Jaanuar 13, 2015 | 29 veebruar 2016
| 0.9.2-prelease | 19 detsembrini 2014 | 29 veebruar 2016
| 0.9.1-prelease | 19 detsembrini 2014 | 29 veebruar 2016
| 0.9.0-prelease | 10 detsembrini 2014 | 29 veebruar 2016

## <a name="faq"></a>KKK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vt ka

DocumentDB kohta leiate lisateavet teemast [Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) teenus.
