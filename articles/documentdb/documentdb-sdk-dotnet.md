<properties 
    pageTitle="DocumentDB .NET API ja SDK | Microsoft Azure'i" 
    description="Lisateavet kõigi .NET API ja SDK, sh avaldamise kuupäevad, aegumine kuupäevade ja iga versiooni DocumentDB .NET SDK vahel tehtud muudatused." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API-d ja SDK-d 

> [AZURE.SELECTOR]
- [.NET-I](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ÜLEJÄÄNUD](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL-I](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API ja SDK

<table>
<tr><td>**SDK allalaadimine**</td><td>[Nugeti](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API dokumentatsioon**</td><td>[.Net-i API dokumentides](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Näidised**</td><td>[.NET koodinäiteid](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Alustamine**</td><td>[DocumentDB .NET SDK kasutamise alustamine](documentdb-get-started.md)</td></tr>
<tr><td>**Web Appi õpetus**</td><td>[Veebirakenduste arendamine koos DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Praeguse toetatud raames**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Väljalaskemärkmed

> [AZURE.IMPORTANT] Alustades versiooni 1.9.2 väljaanne, võidakse kuvada System.NotSupportedException kui päringu sektsioonitud saidikogumid. Selle tõrke vältimiseks veenduge, et teie Majutaja protsess on 64-bitine. Executable projektide, saate seda teha eemaldades "Eelistate 32-bitine" suvand aknas Projekti atribuudid vahekaardil koostamine.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Lisatud otsese ühenduvuse tugi sektsioonitud saidikogumid.
  - Mis on piiratud Staleness järjepidevuse taseme paranenud jõudlus.
  - Lisatud Hulknurk ja täpsustades saidikogumi poliitika geo hoides ruumilise päringuid indekseerimine LineString andmetüübid.
  - Lisatud LINQ tugi StringEnumConverter, IsoDateTimeConverter ja UnixDateTimeConverter tõlkimise predicates ajal.
  - Erinevate SDK veaparandused.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Fikseeritud küsimus, mis põhjustas järgmised NotFoundException: vt seansi pole saadaval Sisestuskeel seansi luba. Seda erandit ilmnenud mõnel juhul kui päringu lugemis-regiooni geo jaotatud konto jaoks.
  - Avatud ResponseStream atribuut ResourceResponse tunni, mis võimaldab otsest juurdepääsu aluseks voo vastuse kaudu.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Muudetud ResourceResponse, FeedResponse, StoredProcedureResponse ja MediaResponse tunnid kasutusele vastava avaliku liidese nii, et nad saavad pilkasid katsetamiseks juhitud juurutamise (TDD).
  - Fikseeritud küsimus, mis põhjustas vigane sektsiooni võtme päise jaotamine andmete jaoks kohandatud JsonSerializerSettings objekti kasutamisel.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Kindla probleemi, mis põhjustas pikaajalise päringute nurjumise tõrge: autoriseerimine luba praegu ei sobi.
  - Kindla probleemi, mis eemaldatakse algse SqlParameterCollection kaudu rist partition ülemine /-Järjestusalus päringud.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Lisatud tugi paralleelselt päringuid sektsioonitud saidikogumid.
  - Lisatud tugi rist partition JÄRJESTUSALUS ja TOP päringute sektsioonitud saidikogumid.
  - Fikseeritud puuduvad viited DocumentDB.Spatial.Sql.dll ja Microsoft.Azure.Documents.ServiceInterop.dll vajalike tagastaks DocumentDB projekti DocumentDB Nugeti pakett viide.
  - Fikseeritud kasutamine eri tüüpi parameetreid LINQ kasutaja määratletud funktsioonide kasutamisel. 
  - Fikseeritud vea globaalselt tiražeeritud kontode jaoks, kus Upsert kõned olid suunatud lugeda asukohad asemel kirjutamine asukohad.
  - Lisatud meetodid IDocumentClient liides, mis olid puuduvad: 
      - UpsertAttachmentAsync meetod, mis võtab mediaStream ja parameetrite suvandid
      - CreateAttachmentAsync meetod, mis võtab parameetrina suvandid
      - CreateOfferQuery meetod, mis võtab querySpec parameetrina.
  - : Lahti pitseeritud avaliku tunnid, mis on esitatud IDocumentClient kasutajaliidese.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Lisatud tugi mitme piirkonna andmebaasi kontod.
  - Lisatud tugi taotluste rakendus uuesti.  Kasutaja saate kohandada korduskatsed ja max ajal arvu atribuudi ConnectionPolicy.RetryOptions konfigureerimisega.
  - Lisatud uus IDocumentClient kasutajaliides, mis määratleb kõigi DocumenClient atribuudid ja meetodid allkirjad.  Selle muudatuse osana ka muuta laiend viise, mida IQueryable ja IOrderedQueryable meetodite DocumentClient klassi ise luua.
  - Lisatud konfiguratsiooni valik on antud DocumentDB lõpp-punkti Uri ServicePoint.ConnectionLimit määramiseks.  Muutke vaikimisi väärtus, mis on seatud 50 ConnectionPolicy.MaxConnectionLimit abil.
  - Taunitud IPartitionResolver ja selle rakendamist.  IPartitionResolver tugi on nüüd aegunud. Soovitatav on kasutada liigendatud saidikogumid kõrgema salvestus- ja läbilaskevõime.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Lisatud ülekoormuse Uri-põhiste ExecuteStoredProcedureAsync meetod, mis võtab RequestOptions parameetrina.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Lisatud aeg live (TTL) tugiteenuste dokumendid.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Fikseeritud vea Nugeti pakendis .NET SDK pakendi Azure'i pilveteenuses lahenduse osana.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Rakendatud [sektsioonitud saidikogumite](documentdb-partition-data.md) ja [kasutaja määratletud jõudlust](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Fikseeritud]** Päringute DocumentDB lõpp-punkti viskab: "System.Net.Http.HttpRequestException: andmevoo sisu kopeerimisel ilmnes tõrge.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Laiendatud LINQ tugi, sh uus tehtemärgid kutsungi, tingimusvormingu avaldiste ja vahemiku võrdlus.
    - Tehtemärk lubamiseks valige ülemine käitumist LINQ tegemine
    - Tekstistring vahemiku võrdlemiseks CompareTo tehtemärk
    - Tingimuslik (?) ja liitumist tehtemärke (?)
  - **[Fikseeritud]** ArgumentOutOfRangeException mudeli projektsiooni koos kombineerimisel Where-In linq päringus.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Fikseeritud]** Kui valige pole viimase avaldise LINQ pakkuja eeldatakse, et erinevused ja valige toodeti * valesti.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Rakendatud Upsert lisatud UpsertXXXAsync meetodid
 - Kõik kutsed täiustatud jõudlus
 - Tingimuslik, LINQ pakkuja tugi liitumist ja CompareTo stringide meetodid
 - **[Fikseeritud]** LINQ pakkuja--> loendis nimega sama SQL IEnumerable ja massiivi loomiseks rakendada sisaldab meetod
 - **[Fikseeritud]** BackoffRetryUtility kasutab sama HttpRequestMessage uuesti asemel uuesti uue konto loomine
 - **[Aegunud]** UriFactory.CreateCollection--> peaks nüüd kasutada UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Fikseeritud]** Lokaliseerimine probleemid kasutamisel mitte en kultuur teave, nt nl-NL jne. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - ID, mis põhineb marsruutimine
    - Uue UriFactory helper abistamiseks ehitamise ID-ga vastavalt ressursside lingid
    - Uue ülekoormuse DocumentClient tegemiseks URI kohta
  - Lisatud IsValid() ja IsValidDetailed() LINQ ruumilisel jaoks
  - Täiustatud LINQ pakkuja tugi
    - **Matemaatika** - Abs Acos, Asin, Atan, Ceiling Cos Exp, Floor, Log, Log10, Pow, Round, märk, Sin, Sqrt, Tan, kärpige
    - **Stringi** - Concat, sisaldab, EndsWith, IndexOf, arv, ToLower, TrimStart, asendada, tühistada, TrimEnd, StartsWith, alamstringi, ToUpper
    - **Massiivi** - Concat, sisaldab loendamine
    - **Klõpsake** tehtemärk

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Indekseerimise poliitikaid muutmist lisatud tugi
    - Uue ReplaceDocumentCollectionAsync meetod DocumentClient
    - Uue IndexTransformationProgress atribuudi ResourceResponse<T> index kohta protsent edenemise jälgimine
    - DocumentCollection.IndexingPolicy on nüüd muutlik
  - Ruumilise indekseerimine ja päringu lisatud tugi
    - Uue Microsoft.Azure.Documents.Spatial nimeruumi jaotamine/deserializing ruumilise tüübid like punkti ja Hulknurk
    - Uue SpatialIndex klassi indekseerimise GeoJSON DocumentDB talletatavad andmed
  - **[Püsiv]** : vale SQL-päringu loodud linq avaldis [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Sõltuvus Newtonsoft.Json v5.0.7 
- Muudatuste Order By
  - OrderBy() või OrderByDescending() LINQ pakkuja tugi
  - IndexingPolicy toetama Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Uue HashPartitionResolver ja RangePartitionResolver tunnid ja selle IPartitionResolver abil andmete jagamine tugi
- DataContract sariväljaanne
- LINQ pakkuja GUID tugi
- LINQ UDF-i tugi

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Esines Nugeti paketi nimi eelvaate ja ga vahel Oleme liikunud **Microsoft.Azure.Documents.Client** **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Eelvaate SDK-d [aegunud]

## <a name="release--retirement-dates"></a>Väljalaske- ja aegumine kuupäevad
Microsoft annab teatis vähemalt **12 kuud** enne pensionile ka SDK, et sujuva ülemineku uuemate/toetatud versiooni.

Uued funktsioonid ja funktsionaalsus ja Optimeerimised lisatakse ainult praegune SDK, sellisena on soovitatav, et täiendate alati uusimale versioonile SDK niipea kui võimalik. 

Mis tahes taotluse DocumentDB endine SDK abil on tagasi lükatud teenus.

> [AZURE.WARNING]
Kõik versioonid Azure DocumentDB SDK .net-i versioon **1.0.0** enne pensionile **29 veebruar**2016. 
 
<br/>
 
| Versioon | Väljaandmisaeg | Aegumine kuupäev 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 mai 2016 |---
| [1.9.5](#1.9.5) | 01 mai 2016 |---
| [1.9.4](#1.9.4) | 24 August 2016 |---
| [1.9.3](#1.9.3) | 15 august 2016 |---
| [1.9.2](#1.9.2) | 23 juuli 2016 |---
| 1.9.1 | Aegunud |---
| 1.9.0 | Aegunud |---
| [1.8.0](#1.8.0) | 14 juuni 2016 |---
| [1.7.1](#1.7.1) | 06 mai 2016 |---
| [1.7.0](#1.7.0) | 26 aprill 2016 |---
| [1.6.3](#1.6.3) | 08 aprill 2016 |---
| [1.6.2](#1.6.2) | 29 märts 2016 |---
| [1.5.3](#1.5.3) | 19 veebruar 2016 |---
| [1.5.2](#1.5.2) | 14 Detsember 2015 |---
| [1.5.1](#1.5.1) | 23 november 2015 |---
| [1.5.0](#1.5.0) | 05 oktoober 2015 |---
| [1.4.1](#1.4.1) | 25 august 2015 |---
| [1.4.0](#1.4.0) | August 13, 2015 |---
| [1.3.0](#1.3.0) | 05 August 2015 |---
| [1.2.0](#1.2.0) | Juuli 06, 2015 |---
| [1.1.0](#1.1.0) | 30 aprill 2015 |---
| [1.0.0](#1.0.0) | 08 aprill 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12 märts 2015 | 29 veebruar 2016
| [0.9.2-prelease](#0.9.x-preview) | Jaanuar 2015 | 29 veebruar 2016
| [.9.1-prelease](#0.9.x-preview) | Oktoober 13, 2014 | 29 veebruar 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 August 2014 | 29 veebruar 2016

## <a name="faq"></a>KKK
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vt ka

DocumentDB kohta leiate lisateavet teemast [Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) teenus. 
