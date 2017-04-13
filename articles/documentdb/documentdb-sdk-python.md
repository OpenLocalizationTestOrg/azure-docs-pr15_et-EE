<properties 
    pageTitle="DocumentDB Python API ja SDK | Microsoft Azure'i" 
    description="Lisateavet kõigi Python API ja SDK, sh avaldamise kuupäevad, aegumine kuupäevade ja iga versiooni DocumentDB Python SDK vahel tehtud muudatused." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-d ja SDK-d

> [AZURE.SELECTOR]
- [.NET-I](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ÜLEJÄÄNUD](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL-I](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python API ja SDK

<table>
<tr><td>**SDK allalaadimine**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API dokumentatsioon**</td><td>[Python API dokumentides](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**SDK installimisjuhiseid.**</td><td>[Python SDK installimisjuhiseid.](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Kaasa SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Alustamine**</td><td>[Python SDK kasutamise alustamine](documentdb-python-application.md)</td></tr>
<tr><td>**Praeguse toetatud platvormi**</td><td>[Python 2.7](https://www.python.org/downloads/) ja [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Väljalaskemärkmed

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Lisatud tugi Python 3.5.
- Lisatud ühenduse ühiskasutus taotlusi mooduli kasutamise tugi.
- Seansi järjepidevuse lisatud tugi.
- Lisatud tugi ülemine/ORDERBY päringute sektsioonitud saidikogumid.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Lisatud uuesti poliitika tugi rakendus taotlused. (Rakendus taotlused saada taotlus määr liiga suur erand, tõrkekood 429.) Vaikimisi DocumentDB uued katsed üheksa korda iga taotlus tõrkekood 429 ilmnemisel austus vastuse päises retryAfter aeg. Fikseeritud uuesti intervalli aeg saate nüüd määrata atribuudi RetryOptions osana ConnectionPolicy objektil, kui soovite ignoreerida retryAfter aeg ning korduste vahel serveri poolt tagastatud. DocumentDB nüüd ootab iga 30 sekundi taotlemine, mis on on rakendus (sõltumata uuesti arv) ja tagastab tõrkekood 429 vastus. Praegu saab ka overriden RetryOptions atribuudi ConnectionPolicy objekti.

- DocumentDB nüüd tagastab x-ms-ahendamise-proovi uuesti-count ja x-ms-throttle-retry-wait-time-ms nagu vastus päised on ahendamise tähistamiseks iga taotluse proovige count ja cummulative aja taotluse oodanud ning korduste vahel.

- Eemaldatud RetryPolicy klassi ja esitatud document_client klassi vastavate atribuutide (retry_policy) ja selle asemel kasutusele RetryOptions ainekursuse asetades ConnectionPolicy klassi alistada osa uuesti vaikesuvandite kasutatavate atribuudi RetryOptions.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Lisatud tugi mitme piirkonna andmebaasi kontod.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- On aeg, et Live(TTL) funktsioon dokumentide tugi.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Seotud serveri pool eraldamine lubamiseks erimärgid partitionkey Path veaparandused.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Rakendatud [sektsioonitud saidikogumite](documentdb-partition-data.md) ja [kasutaja määratletud jõudlust](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Lisa räsi & vahemiku partition selsüünid abistamiseks üle mitme sektsioonid sharding rakendustega.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Rakendada Upsert. Uute UpsertXXX meetodite lisatud Upsert funktsiooni toetamiseks.
- Rakendada ID, mis põhineb marsruutimist. Avaliku API muudatusi, kõik muudatused sisemine.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Toetab ruumilisel register.
- Kinnitatakse id atribuudi kõik ressursid. Ressursside ID-d ei tohi sisaldada ?, /, #, \, märki või end-klahv tühik.
- Lisab uue päise "index teisendus edu" ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Rakendab V2 indekseerimise.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Toetab proxy ühendus.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Kuupäevade väljaanne ja aegumine
Microsoft annab teatis vähemalt **12 kuud** enne pensionile ka SDK, et sujuva ülemineku/toetatud uuema versiooni.

Uued funktsioonid ja funktsionaalsus ja Optimeerimised lisatakse ainult praegune SDK, nagu näiteks see on soovitame alati uuendada uusimale versioonile SDK juba võimalikult. 

Mis tahes taotluse DocumentDB endine SDK abil on tagasi lükatud teenus.

> [AZURE.WARNING]
Kõik versioonid Azure DocumentDB SDK jaoks Python enne versioon **1.0.0** pensionile **29 veebruar**2016. 

<br/>

| Versioon | Väljaandmisaeg | Aegumine kuupäev 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 mai 2016 |---
| [1.9.0](#1.9.0) | Juuli 07, 2016 |---
| [1.8.0](#1.8.0) | 14 juuni 2016 |---
| [1.7.0](#1.7.0) | 26 aprill 2016 |---
| [1.6.1](#1.6.1) | 08 aprill 2016 |---
| [1.6.0](#1.6.0) | 29 märts 2016 |---
| [1.5.0](#1.5.0) | Jaanuar 03, 2016 |---
| [1.4.2](#1.4.2) | 06 Oktoober 2015 |---
| [1.4.1](#1.4.1) | 06 Oktoober 2015 |---
| [1.2.0](#1.2.0) | 06 august 2015 |---
| [1.1.0](#1.1.0) | Juuli 09, 2015 |---
| [1.0.1](#1.0.1) | 25 mai 2015 |---
| [1.0.0](#1.0.0) | 07 aprillis 2015 |---
| 0.9.4-prelease | 14 jaanuar 2015 | 29 veebruar 2016
| 0.9.3-prelease | 09 detsembrini 2014 | 29 veebruar 2016
| 0.9.2-prelease | 25 November 2014 | 29 veebruar 2016
| 0.9.1-prelease | 23 mai 2014 | 29 veebruar 2016
| 0.9.0-prelease | 21 August 2014 | 29 veebruar 2016

## <a name="faq"></a>KKK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vt ka

DocumentDB kohta leiate lisateavet teemast [Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) teenus. 
