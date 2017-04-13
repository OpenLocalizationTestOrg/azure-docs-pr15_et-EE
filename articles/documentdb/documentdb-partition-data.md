<properties 
    pageTitle="Eraldamine ja mastaapimist rakenduses Azure'i DocumentDB | Microsoft Azure'i"      
    description="Siit saate teada, kuidas eraldamine töötab Azure'i DocumentDB, konfigureerimine eraldamine ja partition võtmed ja kuidas valida õige sektsiooni võti rakenduse kohta."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Eraldamine ja mastaapimist Azure'i DocumentDB sisse
[Microsoft Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) eesmärk on aidata teil saavutada kiire ja hõlpsalt prognoosida jõudlus ja skaala sujuvalt koos rakenduse, sest see kasvab. Selles artiklis antakse ülevaade sellest, kuidas eraldamine töötab DocumentDB ja kirjeldatakse DocumentDB saidikogumite tõhus mastaapimiseks rakenduste konfigureerimine.

Pärast selle artikli lugemist on võimalik vastata järgmistele küsimustele:   

- Kuidas Azure'i DocumentDB eraldamine töötab?
- Kuidas seadistada eraldamine sisse DocumentDB
- Mis on partition klahvid, ja kuidas valida õige sektsiooni võti minu rakenduse?

Kood alustada laadige projekti [DocumentDB jõudluse testimine draiver valimi](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark)põhjal. 

## <a name="partitioning-in-documentdb"></a>Klõpsake DocumentDB eraldamine

DocumentDB, saate talletada ja päringu skeemi-vähem JSON dokumentide järjestuses-millisekundilist vastuse ajad, mis tahes skaala. DocumentDB pakub ümbriste nimega **saidikogumid**andmete talletamiseks. Saidikogumite on loogilised ressursid ja võib hõlmata füüsiliste sektsioonid või serverites. Sektsioonid arvu määrab DocumentDB salvestusruumi suurus ja ettevalmistatud läbilaskevõime kogumi alusel. Iga sektsiooni DocumentDB on määratud hulk seotud SSD tagatud salvestusruumi ja on kopeeritud jaoks kõrge kättesaadavus. Sektsiooni haldus haldab täielikult Azure'i DocumentDB ja teil pole keerukate koodi kirjutamiseks ja hallata oma sektsioonid. DocumentDB saidikogumid on **praktiliselt piiramatu** salvestusruum ja läbilaskevõime. 

Eraldamine on täiesti läbipaistev rakenduse. DocumentDB toetab kiiresti loeb ja kirjutab, SQL-i ja LINQ päringute, JavaScript põhinev selgituseks loogika, järjepidevuse tasemed ja kohandatud juurdepääsu reguleerimine REST API kõned ühe saidikogumi ressursside kaudu. Teenuse tegeleb jaotavad andmete sektsioonid ja marsruutimise päringu taotluste õige sektsiooni. 

Kuidas see toimib? Kui loote DocumentDB kogumi, märkate, on **sektsiooni võtme atribuudi** konfiguratsiooni väärtus, mis saate määrata. See on teie dokumente, mida saate kasutada DocumentDB levitada teie andmete hulgas mitut serverit või sektsioonid sees JSON atribuut (või tee). DocumentDB kuvatakse räsi partition võtmeväärtuse ja räsitud tulemi abil saate määratleda sektsiooni JSON dokument salvestatakse. Kõigi dokumentide sama sektsiooni võti salvestatakse samasse sektsiooni. 

Näiteks kaaluge rakendus, mis talletab andmeid töötajate ja nende üksuste DocumentDB. Vaatame valige `"department"` partition klahv atribuudi, et mastaapimiseks andmed osakonna. Iga dokumendi DocumentDB peab sisaldama kohustuslik `"id"` atribuut, mis peab olema kordumatu iga dokumendi sektsiooni võtme väärtus, nt `"Marketing`". Iga dokumendi, mis on talletatud kogumi peab olema kordumatu kombinatsiooni sektsiooni võti ja ID-d, nt `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, ja `{ "Department": "Sales", "id": "0001" }`. Teisisõnu liitindeksi vara (sektsiooni võtme id) on teie saidikogumi primaarvõti.

### <a name="partition-keys"></a>Sektsiooni võtmed
Sektsiooni võti valik on oluline otsus, mida peate tegema koostamise ajal. Peate valima JSON atribuudi nimi, mis sisaldab mitmesuguseid väärtused ja tõenäoliselt olema ühtlaselt Accessi mustrite. Sektsiooni võti on määratud JSON tee, nt `/department` tähistab atribuudi osakonna poole. 

Järgmine tabel näitab näiteid sektsiooni võtme määratlused ja igale JSON väärtused.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Sektsiooni võtme tee</strong></p></td>
            <td valign="top"><p><strong>Kirjeldus</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Vastab doc.department, kus on dokumendis dokumendi JSON väärtus.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ atribuudid/nimi</p></td>
            <td valign="top"><p>Vastab JSON väärtus doc.properties.name, kus on dokumendis dokument (pesastatud atribuudi).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Vastab doc.id JSON väärtus (id "ja" sektsiooni võti on sama atribuudi).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "osakonna nimi"</p></td>
            <td valign="top"><p>Vastab dokumendi ["osakonna nimi"], kus on dokumendis dokumendi JSON väärtus.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Sektsiooni võtme tee süntaks on sarnane indekseerimise poliitika võrguaadressid võtme tee vastav atribuut väärtuse asemel vahe tee kirjeldus, st kaart ei ole wild lõpus. Näiteks määrate/osakonna /? registrisse osakonna jaotises väärtused, kuid määrata /department partition määratlus. Sektsiooni võtme tee on peidetult indekseeritud ja indekseerimise abil indekseerimise poliitika alistab välistada.

Vaatame vaadata, kuidas mõjutab partition klahvi valik rakenduse jõudlus.

### <a name="partitioning-and-provisioned-throughput"></a>Eraldamine ja ettevalmistatud läbilaskevõime
DocumentDB on mõeldud hõlpsalt prognoosida jõudlust. Kui loote kogumi, saate reserveerida läbilaskevõime ** [taotluse üksused](documentdb-request-units.md) (RU) sekundis**. Iga taotlus on määratud taotluse ühiku eest, mis on proportsionaalne süsteemiressursside nagu CPU ja tarbitud toimingu IO summa. Lugege 1 kB dokumendi seansi järjepidevus tarbib 1 taotluse üksus. Lugeda on 1 RU sõltumata talletatud üksuste arv või samaaegseid taotlusi, kus töötab samal arvu. Suuremate dokumentide jaoks on vaja kõrgema taotluse üksuste suurusest. Kui teate, suurus ja oma üksuste arv, peate rakenduse toetamiseks loeb, saate säte, täpse läbilaskevõime nõutav rakenduse kasutaja vajab lugeda. 

Kui DocumentDB talletab dokumente, seda jaotab need ühtlaselt sektsioonid põhjal partition võtmeväärtuse vahel. Funktsiooni läbilaskevõime ka ühtlaselt seas on saadaval piirded ehk läbilaskevõime sektsiooni kohta = (Kokku läbilaskevõime kogumise kohta) / (arv partitsioonid). 

>[AZURE.NOTE] Täielik läbilaskevõime kogumise saavutamiseks peate valima sektsiooni võtme, mis võimaldab teil ühtlaselt levitamiseks taotluste mitmeid eri sektsiooni väärtused.

## <a name="single-partition-and-partitioned-collections"></a>Ühe sektsiooni ja sektsioonitud saidikogumid
DocumentDB toetab nii ühe-sektsiooni ja sektsioonitud Saidikogumite loomine. 

- **Partitioned saidikogumite** saate ühendada mitu sektsiooni ja toetavad väga suurte andmemahtude salvestus- ja läbilaskevõime. Määrake sektsiooni võtme kogumi.
- **Ühe-sektsiooni saidikogumid** on väiksem hind suvandid ja päringu ja toiminguid saidikogumi kõik andmed üle. Neil ühe sektsiooni skaleeritavus ja salvestusruumi piirangud. Teil pole määratud sektsiooni võtme nende saidikogumite jaoks. 

![Klõpsake DocumentDB sektsioonitud saidikogumid][2] 

Stsenaariumid, mis pole vaja suurel hulgal salvestusruumi või läbilaskevõime ühe sektsiooni saidikogumid on hea sobib. Pange tähele, et ühe-sektsiooni saidikogumid skaleeritavus ja salvestuslimiidi ühe sektsiooni, st kuni 10 GB salvestusruumi ja kuni 10 000 taotluse üksuste sekundis. 

Sektsioonitud saidikogumid toetab väga suurte andmemahtude salvestus- ja läbilaskevõime. Vaikimisi pakutakse on konfigureeritud talletada kuni 250 GB salvestusruumi ja skaala kuni 250 000 taotluse üksuste sekundis. Kui teil on vaja suurema salvestusruumi või ühe saidikogumi, võtke ühendust [Azure tugi](documentdb-increase-limits.md) on need suurema teie konto jaoks.

Järgmine tabel loetleb ühe sektsiooni töötamise ja sektsioonitud saidikogumid erinevused.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Ühe sektsiooni saidikogumi</strong></p></td>
            <td valign="top"><p><strong>Sektsioonitud saidikogumi</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Sektsiooni võti</p></td>
            <td valign="top"><p>Ükski</p></td>
            <td valign="top"><p>Nõutav</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Dokumendi primaarvõti</p></td>
            <td valign="top"><p>"id"</p></td>
            <td valign="top"><p>liitindeksi klahvi &lt;sektsiooni võti&gt; ja "id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaalne salvestusruum</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Suurim lubatud salvestusruumi</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Piiramatu (vaikimisi 250 GB)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaalne läbilaskevõime</p></td>
            <td valign="top"><p>400 taotluse ühikut sekundis</p></td>
            <td valign="top"><p>10 000 taotluse üksuste sekundis</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maksimaalse läbilaskevõime</p></td>
            <td valign="top"><p>10 000 taotluse üksuste sekundis</p></td>
            <td valign="top"><p>Piiramatu (vaikimisi sekundis 250 000 taotluse ühikud)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API versioonid</p></td>
            <td valign="top"><p>Kõik</p></td>
            <td valign="top"><p>API 2015-12-16 ja uuem</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Töötamine on SDK-d

Azure'i DocumentDB lisatud tugi automaatne eraldamine [versioon 2015-12-16 REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)API-ga. Sektsioonitud saidikogumite loomiseks tuleb alla laadida SDK versioonid 1.6.0 või uuemat versiooni ühel SDK Toetatud platvormid (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Sektsioonitud Saidikogumite loomine

Järgmises näites kuvatakse .NET koodilõigu salvestada seadme koguda andmeid 20 000 taotluse üksuste sekundis läbilaskevõime kogumi loomiseks. SDK määrab OfferThroughput väärtust (mis omakorda seab selle `x-ms-offer-throughput` taotluse päise REST API). Siin saate selle `/deviceId` sektsiooni võti. Sektsiooni võti valik salvestatakse koos ülejäänud saidikogumi metaandmeid, nt nimi ja indekseerimise poliitika.

Selle proovi jaoks valitud `deviceId` Kuna me tean, et (a) Kuna seal on suur hulk seadmed, kirjutab saate olema ühtlaselt üle sektsioonid ja võimaldab meil mastaapimiseks andmebaasi neelata suuri andmemahtusid ja (b) paljud taotlused nagu tõmbamine uusima lugemine mõne seadme jaoks on rakendatud üks deviceId ja võib laadida ühe sektsiooni.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Sektsioonitud Saidikogumite loomine, peate määrama läbilaskevõime väärtus > 10 000 taotluse üksuste sekundis. Kuna läbilaskevõime on 100 hulgidiagrammides, see peab olema 10,100 või uuem versioon.

Selle meetodiga REST API-ga DocumentDB helistada ja teenus on ettevalmistamise arvu põhjal nõutud läbilaskevõime sektsioonid. Saate muuta kogumi läbilaskevõime jõudluse vajadused muutuda. Üksikasjalikumat teavet teemast [Jõudluse tasemed](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Lugemine ja kirjutamine dokumendid

Nüüd vaatame lisada andmete DocumentDB. Siin on valimi ainekursuse sisaldava seadme lugemine ja kõne CreateDocumentAsync lisada uue seadme koguda ühte lugemine.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Vaatame lugeda dokumendi sektsiooni võti ja id, värskendage seda ja seejärel Viimane toiming, kustutage see partition võti ja ID-d. Teate, et loeb sisaldavad PartitionKey väärtus (vastab olevat `x-ms-documentdb-partitionkey` taotluse päise REST API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Päringute sektsioonitud saidikogumid

Kui teete päringu andmete sektsioonitud saidikogumid, suunab DocumentDB päringu sektsioonid (kui on olemas) filtris määratud sektsioon väärtus vastav automaatselt. Näiteks see päring suunatakse lihtsalt sisaldava sektsiooni võti "XMS 0001" sektsiooni.

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Järgmine päring on filtri sektsiooni võti (DeviceId) ja on tuulutati, et kõik partitsioonid, kus käivitatakse vastu ka sektsiooni register. Teate, et peate määrama selle EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` REST API) on SDK päringu sektsioonid üle.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Paralleelne päringu käivitamine

DocumentDB SDK-d 1.9.0 ja tugiteenuste paralleelse päringu täitmise suvandid, mis võimaldavad teil teha madal latentsus päringute vastu sektsioonitud saidikogumid, isegi siis, kui need tuleb puudutada suure hulga sektsioonid kohal. Näiteks järgmine päring on konfigureeritud paralleelselt sektsioonid üle.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Paralleelne päringu täitmise haldamiseks järgmisi häälestamine.

- Seadmisega `MaxDegreeOfParallelism`, saate määrata, millises paralleelsuse st samaaegne võrguühenduste, et selle saidikogumi sektsioonid maksimumarv. Kui see -1, et paralleelsus astet haldab SDK. Kui soovitud `MaxDegreeOfParallelism` pole määratud või määramine, et 0, mis on vaikeväärtus, on selle saidikogumi sektsioonid ühe võrguühendus.
- Seadmisega `MaxBufferedItemCount`, vahetate väljalülitamine päringu latentsus ja kliendi pool mälu kasutamine. Kui argument see parameeter või see -1, et hallatakse SDK puhverdatud paralleelselt päringu käitamise ajal üksuste arvu.

Antud muutmata kogumi, paralleelse päring tagastab tulemused samas järjestuses nagu järjestikune täitmise. Rist-partition päring, mis sisaldab sortimise (JÄRJESTUSALUS või TOP) täites DocumentDB SDK probleemid päringu paralleelselt üle sektsioonid ja ühendab osaliselt sorditud tulemusi kliendipoolse globaalselt järjestatud tulemusi.

### <a name="executing-stored-procedures"></a>Salvestatud toimingute täitmine

Võite täita atomic tehingute dokumendid, millel on sama seadme ID, nt kui te olete säilitades agregaadid või seadme ühte dokumenti vastavalt. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

Järgmises jaotises vaatame, kuidas saate teisaldada sektsioonitud saidikogumid ühe-sektsiooni saidikogumid.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migreerimine versioonist ühe-sektsiooni sektsioonitud saidikogumid
Ühe-sektsiooni saidikogumi abil nõuab suurema läbilaskevõimega (> 10 000 RU/s) või suurem andmete talletamine (> 10GB), saate [DocumentDB andmete Migreerimistööriista](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) andmete migreerimiseks ühe-sektsiooni saidikogumi sektsioonitud saidikogumi. 

Ühe-sektsiooni saidikogumi sektsioonitud saidikogumi migreerimine

1. Ühe-sektsiooni saidikogumi JSON andmete eksportimine. Lisateabe saamiseks vt [JSON-faili eksportida](documentdb-import-data.md#export-to-json-file) .
2. Andmete importimine sektsioonitud kogumi loodud sektsiooni võtme määratluse ja üle 10 000 taotluse ühiku teise läbilaskevõime, nagu on näidatud järgmises näites. Lisateabe saamiseks vt [importimise DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) .

![Migreerimine andmete kogumise Partitioned DocumentDB][3]  

>[AZURE.TIP] Kiirem impordi korda, kaaluge suureneva arvu, paralleelselt taotluste 100 või suurem ära suurema läbilaskevõimega, mis on saadaval sektsioonitud saidikogumid. 

Nüüd, kui me lõpule viidud põhitõed, Vaatame mõned olulised kujundamine DocumentDB partition kiirklahvide (Pääsuklahvide) töötamisel.

## <a name="designing-for-partitioning"></a>Jagamine kujundamine
Sektsiooni võti valik on oluline otsus, mida peate tegema koostamise ajal. Selles jaotises kirjeldatakse mõningaid kompromisse, mis on seotud valida partition klahvi oma saidikogumi jaoks.

### <a name="partition-key-as-the-transaction-boundary"></a>Tehingu äärist Partition klahvi
Teie valitud sektsiooni võti peaks saldo vajadust tehinguid jagada oma üksuste üle mitme partition võtmed tagada skaleeritav lahendus selle nõude kasutamise lubamine. Ühes otsas, saate väärtuseks sama sektsiooni võti kõigi dokumentide jaoks, kuid see võib piirata teie lahenduse skaleeritavus. Skaala teises otsas, võite määrata kordumatu sektsiooni võti iga dokumendi, mis võib olla väga paindlik, kuid soovite takistada rist dokumendi tehingute salvestatud toimingute ja päästikute kaudu. Mõne optimaalne partition vajutamisega on üks võimaldab teil kasutada tõhusa päringud ja mis on piisavalt arvutite tagada teie lahendus on scalable. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Salvestusruum ja jõudluse kitsaskohtade vältimine 
Samuti on oluline valida atribuut, mis võimaldab kirjutab levitatakse kogu erinevate väärtuste arv. Sama sektsiooni võti taotluste ei tohi ületada ühe sektsiooni läbilaskevõime ja kuvatakse rakendus. Seega on oluline valida sektsiooni võti, mis ei põhjusta **"tulipunktidele"** oma rakenduses. Dokumentide sama sektsiooni võti kokku mälumaht saate ka kuni 10 GB salvestusruumi. 

### <a name="examples-of-good-partition-keys"></a>Hea partition klahvid näited
Siin on mõned näited selle kohta, kuidas valida rakenduse sektsiooni võti:

* Kui olete rakendamisel kasutaja profiili taustväärtus, kasutaja ID-d on hea valik sektsiooni võti.
* Kui talletate asjade andmete nt seadme olek, seadme ID on hea valik sektsiooni võti.
* Kui kasutate DocumentDB logimine aja andmesarja, hostname või protsessi ID on hea valik partition klahvi.
* Kui teil on mitme rentniku arhitektuur, rentniku ID on hea valik sektsiooni võti.

Pange tähele, et (nagu eespool kirjeldatud asjade ja kasutajale profiil) kasutamine mõnel sektsiooni võti võib sama, mis teie id (dokumendi võti). Teistes nt kellaaja sarja andmete, võib olla sektsiooni võti, mis on id erineda.

### <a name="partitioning-and-loggingtime-series-data"></a>Andmete eraldamine ja logimine/kellaaja-sarja
Üks kõige levinum kasutamine juhtudel DocumentDB on logimine ja telemeetria. See on oluline valida hea sektsiooni võtme, kuna peate lugemis-ja kirjutamisõigusega suur andmemahtusid. Valik on sõltuvad teie lugemine ja kirjutamine hinnad ja tüüpi päringute eeldate, et käivitada. Siit leiate näpunäiteid selle kohta, kuidas valida hea sektsiooni võti.

- Kui kasutada tegemist väike intressimäära, kirjutab acculumating pikka aega ja päringu abil lahtrivahemike ajatemplid vaja ja muud filtrid, siis kasutades rollup ajatempli, nt kuupäev sektsiooni võti on hea lähenemine. See võimaldab teil päringusse üle kõik andmed kuupäevale ühe sektsiooni kaudu. 
- Kui teie töökoormus on raske kirjutamine, mis on üldiselt levinud, kasutage sektsiooni võtme, mis ei põhine ajatempli, et DocumentDB saate levitada kirjutab ühtlaselt kogu sektsioonid arv. Siin hostinimi, protsessi ID, tegevuse ID või mõne muu atribuudi koos kõrge arvutite on hea valik. 
- Kolmas lähenemine on hübriid ühte kui teil on mitu saidikogumite, üks iga päev/kuu ja partition võti on Varundustöö atribuudi nagu hostname (hostinimi). See eelis, mida saate seada erinevatele tasemed põhjal aja aken, nt kogumise praeguse kuu on ette valmistatud suurema läbilaskevõime kuna see pakub loeb ja kirjutab, arvestades eelmise kuu vähendage läbilaskevõime, kuna neid kasutada ainult loeb.

### <a name="partitioning-and-multi-tenancy"></a>Eraldamine ja mitme rendiõigus
Kui te rakendavad mitme rentniku kasutav DocumentDB, on kaks põhilist mustrite rakendamiseks rendiõigus koos DocumentDB – üks sektsioon klahvi rentniku kohta ja ühe saidikogumi rentniku kohta. Siin on spetsialistid ja miinuste iga:

* Rentniku kohta ühte sektsiooni: selle mudeli rentnikud paikneb ühe saidikogumi sees. Kuid vastu ühe sektsiooni saab teha päringuid ja lisab dokumentide ühe rentniku jaoks. Saate rakendada ka selgituseks loogika üle kõik dokumendid rentniku. Kuna mitme rentniku kogumi ühiskasutusse anda, saate salvestada salvestusruumi ja läbilaskevõime ressursside rentnikud sees ühe saidikogumi jaoks asemel ettevalmistamise eest tulem iga rentniku jaoks. Tagastamise on teil jõudluse eraldamise rentniku kohta. Jõudlus ja läbilaskevõime suureneb rakendada kogu saidikogumi vs suunatud suureneb rentnikke.
* Ühe saidikogumi rentniku kohta: iga Rentnik on oma saidikogumi. Selle mudeli saate reserveerida jõudluse rentniku kohta. DocumentDB tema uue vastavalt hinnakirjad mudel, see mudel on mitme rentniku rakenduste väheste rentnikud tulusamaks.

Samuti saate kombinatsioon/astmelise lähenemisviisi, mis collocates small rentnikud ja migreerib suuremat rentnikud oma kogumi.

## <a name="next-steps"></a>Järgmised sammud
Selles artiklis kirjelduse Azure'i DocumentDB kuidas eraldamine töötab, kuidas saate luua sektsioonitud saidikogumite ja kuidas saate valida hea sektsiooni võtme rakenduse. 

-   Skaala ja jõudluse testimine DocumentDB teha. Vt [jõudlus ja katsetada Azure'i DocumentDB abil](documentdb-performance-testing.md) .
-   Töö alustamine kodeerimise [SDK-d](documentdb-sdk-dotnet.md) või [REST API -ga](https://msdn.microsoft.com/library/azure/dn781481.aspx)
-   Lisateavet [ettevalmistatud läbilaskevõime sisse DocumentDB](documentdb-performance-levels.md)
-   Kui soovite kohandada, kuidas rakenduse sooritab eraldamine, ühendage oma kliendipoolne eraldamine rakendamist. Vt [kliendipoolne eraldamine tugi](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
