<properties
    pageTitle="Azure'i otsingu abil indexers DocumentDB ühendavad | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas Azure'i otsingu indekseerija koos DocumentDB andmeallikana kasutada."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Azure'i otsingu abil indexers DocumentDB ühendavad

Kui otsite rakendada hea otsing kogemusi DocumentDB andmete, kasutage Azure'i otsingu indekseerija DocumentDB! Selles artiklis näitame teile kuidas Azure'i DocumentDB integreerimine Azure otsing ilma säilitamiseks indekseerimise taristu koodi kirjutamiseks!

Selle häälestanud on [häälestamine konto Azure otsing](../search/search-create-service-portal.md) (te ei pea võtta kasutusele standardne otsing) ja seejärel helistage [Azure'i Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) DocumentDB **andmeallika** ja **indekseerija** selle andmeallika loomine.

Tellimuse Saada kutsed suhelda REST API-d, saate [postiljon](https://www.getpostman.com/), [viiuldaja](http://www.telerik.com/fiddler)või mis tahes tööriista oma eelistus.


##<a id="Concepts"></a>Azure'i otsingu indekseerija mõisted

Azure'i otsingu toetab loomist ja haldamist andmete allikatest (sh DocumentDB) ja indexers, mida kasutada nendele andmeallikatele.

**Andmeallika** saate määrata, milliseid andmeid on vaja indekseerida, identimisteabe juurdepääsuks andmed ja poliitikate Azure'i otsingu tõhus tuvastamiseks muudatuste andmeid (nt muuta või kustutada oma saidikogumi dokumendid). Andmeallikas on määratletud on sõltumatu ressurss, et seda saab kasutada mitut indexers.

Mõne **indekseerija** kirjeldatakse, kuidas andmed juhtimine andmeallikast pärinevaid andmeid rakendusse target otsing indeks. Plaanite peaks ühe indekseerija target index ja andmete allikas kombinatsioonide loomise kohta. Samas võite on mitu sama registrisse kirjutamine indexers, indekseerija saab ainult kirjutada ühe indeks. Indekseerija kasutatakse:

- Ühekordse andmetega asustada registri koopia teha.
- Registri ja ajakava andmeallika muudatused sünkroonida. Graafik on osa indekseerija määratlus.
- Autonoomsest nõudmisel värskendused registri vastavalt vajadusele.

##<a id="CreateDataSource"></a>Samm 1: Andmeallika loomine

Probleemi HTTP POST taotluse luua uue andmeallika Azure'i otsingu teenus, sh järgmised taotluse päised.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Funktsiooni `api-version` on nõutav. Lubatud väärtused sisaldavad `2015-02-28` või uuem versioon. Külastage [API versioonid Azure otsingus](../search/search-api-versions.md) kõigi toetatud otsingu API versioonide vaatamiseks.

Taotluse sisu sisaldab andmeallika määratluse, mis peaks sisaldama järgmised väljad:

- **nimi**: valige mis tahes tähistada DocumentDB andmebaasi nimi.

- **Tüüp**: kasutamine `documentdb`.

- **mandaadi**:

    - **connectionString**: nõutav. Määrake ühenduse teave andmebaasi Azure DocumentDB järgmises vormingus:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **container**:

    - **nimi**: nõutav. Määrake DocumentDB saidikogumi indekseerida.

    - **päringu**: valikuline. Saate määrata päringu Ühenda suvalise JSON dokumendile üheks tasapinnalise skeem, mis Azure'i otsing saate indekseerida.

- **dataChangeDetectionPolicy**: valikuline. Lugege teemat [andmete muutmine tuvastamise poliitika](#DataChangeDetectionPolicy) allpool.

- **dataDeletionDetectionPolicy**: valikuline. Lugege teemat [andmete kustutamise tuvastamise poliitika](#DataDeletionDetectionPolicy) .

Allpool on [näide koosolekukutse kehasse](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Hõivamine muudetud dokumendid

Andmete muutmine tuvastamise poliitika eesmärk on andmeid muudetud üksuste tõhus tuvastamiseks. Praegu on ainus toetatud poliitika on `High Water Mark` poliitika abil soovitud `_ts` viimati muudetud ajatempli vara DocumentDB - mis on määratletud järgmiselt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Samuti peate lisama `_ts` sisse projektsiooni ja `WHERE` klausel päringu. Näiteks:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Kustutatud dokumentide hõivamine

Kui read on kustutatud lähtetabel, peaks need read kustutada need otsinguregistrisse. Andmete kustutamise tuvastamise poliitika eesmärk on kustutatud andmete tõhus tuvastada. Praegu on ainus toetatud poliitika on `Soft Delete` poliitika (kustutamise on mingi lipuga märgitud), mis on määratletud järgmiselt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Peate atribuudi softDeleteColumnName kaasamine SELECT-klauslis, kui kasutate kohandatud projektsiooni.

###<a id="CreateDataSourceExample"></a>Koosolekukutse kehasse näide

Järgmises näites luuakse andmeallika koos kohandatud päringu ja poliitika vihjeid:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Vastus

Kui andmeallika on loodud, kuvatakse HTTP 201 loodud vastuse.

##<a id="CreateIndex"></a>Samm 2: Registri loomine

Kui teil pole seda juba target Azure'i otsinguindeksi loomine Saate seda teha [Azure portaali UI](../search/search-create-index-portal.md) või [Luua registri API](https://msdn.microsoft.com/library/azure/dn798941.aspx)kaudu.

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Veenduge, et teie target index skeemiga on skeemi andmeallika JSON dokumentide või oma kohandatud päringu projektsiooni väljund.

>[AZURE.NOTE] Sektsioonitud saidikogumid, on vaikimisi dokumendi võti DocumentDB's `_rid` atribuut, mille saab uueks nimeks `rid` Azure otsing. Lisaks DocumentDB `_rid` väärtused sisaldama märgid, mis ei sobi Azure'i otsingu Keys; Seetõttu on `_rid` väärtused on kodeeritud Base64.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Joonis A: vastenduse JSON andmetüübid ja Azure otsingu andmetüübid

| JSON-ANDMETÜÜP|   ÜHILDUVAD TARGET INDEX VÄLJATÜÜBID|
|---|---|
|Bool|Edm.Boolean Edm.String|
|Arvud, mis näeb välja täisarvude|Edm.Int32, Edm.Int64, Edm.String|
|Arvud, mis näevad välja nagu ujuv punktid|Edm.Double Edm.String|
|String|Edm.String|
|Massiive primitiivne tipib nt "a", "b" \ "c" |Collection(EDM.string)|
|Stringide, mis näeb välja kuupäevad| Edm.DateTimeOffset Edm.String|
|GeoJSON objektid, nt {"tüüp": "Punkt", "koordinaadid": [pikk, Läti]} | Edm.GeographyPoint |
|JSON-objektid|N/A|

###<a id="CreateIndexExample"></a>Koosolekukutse kehasse näide

Järgmises näites luuakse registri väljaga id ja kirjeldus:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Vastus

Kui indeks on loodud, kuvatakse HTTP 201 loodud vastuse.

##<a id="CreateIndexer"></a>Samm 3: Looge indekseerija

Järgmised päistega HTTP POST-taotluse abil saate luua uue indekseerija on teenuses Azure otsing.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Taotluse sisu sisaldab indekseerija määratlus, mis peaks sisaldama järgmised väljad:

- **nimi**: nõutav. Selle indekseerija nimi.

- **dataSourceName**: nõutav. Olemasoleva andmeallika nimi.

- **targetIndexName**: nõutav. Olemasoleva registri nimi.

- **ajakava**: valikuline. Vaadake [indekseerimise ajakava](#IndexingSchedule) allpool.

###<a id="IndexingSchedule"></a>Töötab indexers ajakava

Soovi korral saate indekseerija määratleda ajakava. Kui ajakava on olemas, käivituvad selle indekseerija perioodiliselt ühe ajakava. Graafik sisaldab järgmisi atribuute.

- **intervalli**: nõutav. Kestus väärtus, mis määrab intervalli, või perioodi indekseerija töötab. Väikseim lubatud intervall on 5 minuti jooksul; pikima on üks päev. See peab vormindatud XSD-"dayTimeDuration" väärtus (piiratud Alamhulk on [ISO 8601 kestus](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -väärtus). See muster on: `P(nD)(T(nH)(nM))`. Näited: `PT15M` iga 15 minuti `PT2H` iga 2 tundi.

- **startTime**: nõutav. UTC datetime, mis määrab, millal peaks algama selle indekseerija töötab.

###<a id="CreateIndexerExample"></a>Koosolekukutse kehasse näide

Järgmises näites luuakse indekseerija, mis kopeerib andmete kogumise viidatud selle `myDocDbDataSource` andmeallikat soovitud `mySearchIndex` index ajakavas, mis algab 1 jaan 2015 UTC ja töötab tunnis.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Vastus

Kui selle indekseerija on loodud, kuvatakse HTTP 201 loodud vastuse.

##<a id="RunIndexer"></a>Samm 4: Käivita indekseerija

Lisaks töötab perioodiliselt ajakava, indekseerija saate ka kasutada nõudmisel järgmised HTTP POST taotluse väljastamise:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Vastus

Saate HTTP 202 aktsepteeritud vastuse, kui selle indekseerija käivitati edukalt.

##<a name="GetIndexerStatus"></a>Juhis 5: Get registraatori olekut

Saate välja tuua praeguse oleku ja täitmise ajalugu indekseerija HTTP GET-päring:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Vastus

Kuvatakse HTTP 200 OK vastuse tagastatud koos vastuse sisu, mis sisaldab teavet üldine registraatori seisund olekut, viimase indekseerija kutsumise, samuti tehtud indekseerija manamine ajalugu (kui see on olemas).

Vastuse peaks välja nägema umbes järgmine:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Täitmise ajalugu sisaldab kuni 50 viimase lõpuleviidud täitmised, mis on pööratud ajalises järjestuses sorditud (nii, et uusim täitmise jõuab vastusesse).

##<a name="NextSteps"></a>Järgmised sammud

Palju õnne! Te just teada, kuidas Azure'i DocumentDB integreerimine Azure otsingu abil soovitud indekseerija DocumentDB.

 - Vaadake lisateavet kuidas Azure'i DocumentDB, [DocumentDB teenus](https://azure.microsoft.com/services/documentdb/).

 - Siit saate teada, kuidas Lisateavet Azure otsing, lugege teemat [otsingu teenus](https://azure.microsoft.com/services/search/).
