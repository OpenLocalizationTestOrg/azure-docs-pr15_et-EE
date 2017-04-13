<properties 
    pageTitle="Azure'i DocumentDB ruumilisel andmetega töötamine | Microsoft Azure'i" 
    description="Mõista, kuidas luua, indekseerida ja päringu Azure'i DocumentDB ruumilise objektid." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Azure'i DocumentDB Ruumilisel andmetega töötamine

Selles artiklis on sissejuhatus [Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/)ruumilisel funktsioone. Pärast seda lugeda, siis saab järgmised küsimused:

- Kuidas Azure'i DocumentDB ruumilise andmete talletamiseks?
- Kuidas saan päringu ruumilisel andmete LINQ ja SQL Azure'i DocumentDB?
- Kuidas lubada või keelata ruumilise indekseerimine sisse DocumentDB?

Lugege selle koodinäiteid [Github projekti](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) .

## <a name="introduction-to-spatial-data"></a>Ruumiliste andmete tutvustus

Ruumiliste andmete kirjeldatakse asukoha ja objektide ruumi kuju. Enamik rakendustes need vastavad maa, st ruumilisel andmete objekte. Ruumiliste andmete saab tähistada isik, koht huvi või linna või järv asukoht. Kasutage levinud olukorda hõlmavad sageli lähedus päringuid, näiteks "leida kõik kohvi poed lähedal minu praegune asukoht". 

### <a name="geojson"></a>GeoJSON
DocumentDB toetab indekseerimine ja päringud ruumilisel punkti andmeid, mis on esindatud kasutamine [GeoJSON määratlus](http://geojson.org/geojson-spec.html). GeoJSON andmestruktuurid on alati kehtiv JSON objektide nii, et ta saab salvestada ja esitatakse selle kohta päring DocumentDB ilma spetsiaalseid tööriistu või teekide abil. DocumentDB SDK-d pakuvad helper tunnid ja viise, mida oleks hõlpsam ruumilise andmetega töötada. 

### <a name="points-linestrings-and-polygons"></a>Punktide, linestrings ja hulknurga
**Punkti** tähistab ühe positsiooni ruumi. Ruumilisel andmed, tähistab punkti täpse asukoha, mis võiks olla ostude poe, kiosk, auto või linn füüsilise asukoha aadressi.  Punkti on esindatud GeoJSON (ja DocumentDB), kasutades oma koordinaatide paari või pikkus- ja laiuskraadide. Siin on näide JSON punkti jaoks.

**Punktide DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] GeoJSON spetsifikatsioon määrab laiuskraadide ees- ja laiuskraadide teise. Nagu vastendamise muid rakendusi, pikkus- ja laiuskraadide on nurgad ja esindatud kraadi osas. Laiuskraadide väärtused on mõõdetuna algarvu geograafiline ja-180 180.0 kraadi ja vahel laiuskraad on väärtused on mõõdetud ekvaatoril ja on vahel-90.0 ja 90,0 kraadi. 
>
> DocumentDB tõlgendab koordinaate WGS 84 viide süsteemi kujutatud. Koordinaatide süsteemi kohta lisateabe saamiseks vt allpool.

See võib olla manustatud DocumentDB dokumendis, nagu on näidatud järgmises näites asukoha andmeid sisaldav kasutajaprofiili:

**Profiili kasutamine talletatud DocumentDB asukoht**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Lisaks punktid GeoJSON toetab LineStrings ja hulknurga. **LineStrings** tähistavad kahe või enama punkti ruumi ja ühendada need rea segmente sarja. Ruumilisel andmete linestrings kasutatakse tavaliselt kujutab kiirteed või jõed. **Hulknurga** on ühendatud punktide äärist, mis moodustab suletud LineString. Hulknurga levinud loomulikus formatsioonides nagu järved või poliitika jurisdiktsiooni nagu linna ja olekus. Siin on näide hulknurga DocumentDB. 

**Hulknurga sisse DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] GeoJSON määratlus nõuab, et jaoks lubatud hulknurga, viimase koordinaatide paari esitatud olema sama mis esimene, luua suletud kujund.
>
>Hulknurga sees peab olema määratud vastupäeva järjestuses. Hulknurga määratud päripäeva järjestuses tähistab piirkond kogumahu pöördfunktsiooni väärtuse.

Lisaks punkti, LineString ja Hulknurk GeoJSON määrab esituse kohta rühmitada mitme ruumilisel asukohad, samuti suvalise atribuudid seostamist **funktsiooni**geograafiline asukoht. Kuna need objektid on lubatud, JSON, ta saab kõik talletatud ja töödelda DocumentDB. Kuid DocumentDB toetab ainult, automaatse indekseerimise punkte.

### <a name="coordinate-reference-systems"></a>Koordinaatide süsteemi

Kuna Maa kuju korrapäratu, koordinaadid ruumilisel andmed on esindatud paljud koordinaatide süsteemid (KMS) iga oma paneelide arv viide ja mõõtühikud. Näiteks on "liikmesriikide ruudustiku Suurbritannia" on viide on väga täpne Ühendkuningriik, kuid mitte seda väljapoole. 

Kõige populaarsemate KMS kasutusel on juba täna maailma geodeetiline süsteem [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS seadmed ja palju vastenduse teenuseid, sealhulgas Google kaardid ja Bingi kaartide API-de kasutamine WGS 84. DocumentDB toetab indekseerimine ja päringud ruumilisel andmete WGS 84 KMS ainult abil. 

## <a name="creating-documents-with-spatial-data"></a>Ruumilise andmetega dokumentide loomine
Dokumendid, mis sisaldavad väärtusi GeoJSON loomisel nad on automaatselt indekseeritud ruumilise indeks vastavalt indekseerimise poliitika kogumi. Kui töötate DocumentDB SDK nagu Python või Node.js dünaamiliselt tipitud keeles, peate looma kehtiv GeoJSON.

**Dokumendi loomine Node.js ruumilisel andmetega**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Kui töötate .net-i (või Java) SDK-d, saate punkt- ja Hulknurk uutel sees Microsoft.Azure.Documents.Spatial nimeruumi manustamiseks asukohateavet sees oma rakenduse objektid. Need tunnid aitab lihtsustada sariväljaanne ja vahemäluasukohaga ruumiliste andmete GeoJSON sisse.

**.NET ruumilisel andmetega dokumendi loomine**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Kui te pole ja teavet, kuid on füüsilist aadressi või asukoha nime, nt linn või riik, saate otsida tegelik koordinaate geokodeerimise teenuse nagu Bingi kaartide ülejäänud teenuste abil. Lisateavet Bingi kaartide geokodeerimise [siin](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Päringute ruumilise tüübid

Nüüd kus oleme võtnud vaadata, kuidas lisada ruumilisel andmeid, vaatame vaadata, kuidas päringu abil SQL ja LINQ abil DocumentDB andmed.

### <a name="spatial-sql-built-in-functions"></a>Ruumilise SQL-i sisseehitatud funktsioonid
DocumentDB toetab ruumilisel päringute avatud ruumilisel Consortium (OGC) sisseehitatud järgmisi funktsioone. Lisateavet valmisfunktsioonide SQL keeles täielik kogum, vaadake [Päringu DocumentDB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Kasutus</strong></td>
  <td><strong>Kirjeldus</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Tagastab kahe GeoJSON punkti avaldiste vahel kaugus.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Tagastab loogikaavaldis, mis näitab, kas esimese argumendi määratud punkti GeoJSON jääb GeoJSON Hulknurk teises argumendis.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Tagastab kahendväärtuse, mis näitab, kas määratud GeoJSON punkt või Hulknurk avaldis on lubatud.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Tagastab sisaldava on kahendväärtus JSON väärtus väärtus, kui määratud GeoJSON punkt või Hulknurk avaldis on lubatud ja kui see on kehtetu, lisaks põhjus stringi väärtus.</td>
</tr>
</table>

Ruumilise funktsioone saab kasutada lähedus päringute ruumilise andmetega täita. Siin on näiteks päring, mis tagastab kõik pere dokumendid, mis on 30 km, kasutades funktsiooni ST_DISTANCE sisseehitatud määratud asukohta. 

**Päringu**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Tulemused**

    [{
      "id": "WakefieldFamily"
    }]

Kui te oma indekseerimise poliitika ruumilise indekseerimine, siis "kauguse päringute" pakutakse tõhus registri kaudu. Ruumilise indekseerimine rohkem üksikasju, leiate allpool olevat jaotist. Kui teil pole ruumilise indeks jaoks määratud teed, saate siiski teha ruumilise päringud, määrates `x-ms-documentdb-query-enable-scan` taotluse päis on seatud väärtus "true". .Net-i, saate seda teha saates **FeedOptions** valikulist argumenti päringute [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) on seatud tõene. 

ST_WITHIN saab kontrollida, kui punkti asub hulknurga. Hulknurga on levinud tähistada piirmäärad, nt sihtnumbrite, state piirmäärad või loomulikus koosseise. Uuesti kui kaasate ruumilise indekseerimine oma indekseerimise poliitika, siis "sees" päringute pakutakse tõhus registri kaudu. 

Hulknurga ST_WITHIN argumendid võivad sisaldada ainult ühe ring, st ei tohi sisaldada soovitud hulknurga auke neid. 

**Päringu**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Tulemused**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Sarnane kuidas sobimatud andmetüübid töötab DocumentDB päringus, kui väärtus asukoht määratud üks argument on vigane või ei sobi ja seejärel selle hindab **määratlemata** ja hinnatud dokumendi põhjal päringutulemite vahele jätta. Kui teie päring tagastab tulemeid, käivitage ST_ISVALIDDETAILED abil silumine miks spatail tüüp on kehtetu.     

DocumentDB toetab ka läbimiseks pööratud päringud, st saate indekseerida hulknurga või DocumentDB rida ja seejärel päringu alal, mis sisaldavad määratud punkti. See muster on näiteks siis, kui auto sisestab või määratud ala lahkub tuvastamiseks logistikas levinud. 

**Päringu**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Tulemused**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID ja ST_ISVALIDDETAILED saab kontrollida, kui ruumiline objekt on lubatud. Näiteks järgmine päring kontrollib kehtivus punkti koos kontorist väljas vahemiku laiuskraad väärtus (-132.8). ST_ISVALID tagastab ainult kahendväärtuse ja ST_ISVALIDDETAILED tagastab selle kahendmuutuja ja string, mis sisaldab põhjus, miks peetakse sobimatu.

**Päringu**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Tulemused**

    [{
      "$1": false
    }]

Neid funktsioone saab kasutada ka hulknurga kinnitamiseks. Näiteks siin kasutame ST_ISVALIDDETAILED valideerimiseks hulknurga, mis on suletud. 

**Päringu**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Tulemused**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>Päringute .NET SDK LINQ

DocumentDB .NET SDK ka pakkujate kategooria meetodite `Distance()` ja `Within()` LINQ avaldiste kasutamiseks. DocumentDB LINQ pakkuja vaste nende meetod kõned võrdväärse SQL-i sisseehitatud funktsiooni kõned (ST_DISTANCE ja ST_WITHIN vastavalt). 

Siin on näide LINQ päring, mis otsib kõik dokumendid "asukoht" väärtusega on määratud 30km raadiusega osutage abil LINQ DocumentDB kogumist.

**LINQ päringu vahemaa**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Samuti on siin kõik dokumendid, mille "asukoht" on määratud väljale/Hulknurk otsimise päringu. 

**LINQ päringu jaoks sees**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nüüd kus oleme võtnud vaadata, kuidas päringu abil LINQ ja SQL-i dokumente, vaatame vaadata, kuidas konfigureerida DocumentDB ruumilise indekseerimine.

## <a name="indexing"></a>Indekseerimine

Kirjeldatud me [Skeemi agnostik indekseerimine koos Azure'i DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paberi, lõime DocumentDB's andmebaasimootor olema tõeliselt skeemi agnostik ja esimese klassi toetab JSON. Kirjutamine optimeeritud andmebaasimootor, DocumentDB algupäraselt mõistab ruumilise andmed (punkte, hulknurga ja jooned) esindatud GeoJSON standardne.

Lühidalt, geomeetria on prognoositud kaudu geodeetilised koordinaadid peale 2D lennuk siis järk-järgult jagatud lahtrite lisamine **quadtree**abil. Need lahtrid on vastendatud lahtrit, **Hilbert ruumi täitmise kõvera**, mis säilitab punktide asukoht asukoht põhineb 1D. Lisaks, kui asukohaandmed on indekseeritud, see läheb läbi protsessi nimetatakse **mosaiiki**, st kõik lahtrid, mis kattuks asukoht on tuvastatud ja salvestatud DocumentDB registri võtmed. Päringu ajal argumendid, näiteks punktide ja hulknurga on ka tessellated oluline ID lahtrivahemike eraldada, siis registri andmete toomiseks kasutada.

Kui määrate indekseerimise poliitika, mis sisaldab ruumilist registri / * (kõik teed), siis kõik punktid leitud kogumis on indekseeritud tõhusa ruumilise päringuid (ST_WITHIN ja ST_DISTANCE). Ruumilise registrid täpsuse väärtus ja Kasuta alati arvutustäpsuse vaikeväärtus.

>[AZURE.NOTE] DocumentDB toetab automaatse indekseerimise punkte, hulknurga ja LineStrings

Järgmised JSON koodilõigu näitab indekseerimise poliitika ruumilise indekseerimine lubatud, st indekseerida mis tahes GeoJSON punkti leiti dokumentide jaoks abil. Kui muudate Azure'i portaalis indekseerimise poliitika, saate määrata järgmised JSON indekseerimise poliitika lubamiseks ruumilise indekseerimise oma saidikogumi.

**Saidikogumi indekseerimine poliitika JSON ruumiline punktid ja hulknurga lubatud**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Siin on koodilõik .net-i, mis näitab, kuidas luua kogumi ruumilise indekseerimine kõik teed, mis sisaldab punktide jaoks sisse lülitatud. 

**Ruumilise indekseerimine kogumi loomine**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Ja siin on, saate muuta mõne olemasoleva saidikogumi ära ruumilise indekseerimine üle kõik punktid, mis on salvestatud dokumendid.

**Muuta mõne olemasoleva saidikogumi koos ruumilise indekseerimine**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Kui asukoht GeoJSON väärtus dokumendis on vigane või ei sobi, siis see kuvatakse ei saada indekseeritud abil. Saate kinnitada asukoha väärtused ST_ISVALID ja ST_ISVALIDDETAILED abil.
>
> Kui teie saidikogumi määratlus sisaldab sektsiooni võtme, teisendus edenemise indekseerimine ei teatatud. 

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui te olete selle kohta, kuidas alustada ruumilisel tugi DocumentDB õppinud, saate teha järgmist.

- Käivitage [ruumilisel .NET koodinäiteid github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs) kodeerimine
- Get käed ruumilisel päringute [DocumentDB päringu mänguväljak](http://www.documentdb.com/sql/demo#geospatial) veebisaidil
- Lisateavet [DocumentDB päring](documentdb-sql-query.md)
- Lisateavet [DocumentDB indekseerimine poliitika](documentdb-indexing-policies.md)
