<properties
   pageTitle="Täiendamine versiooniks Azure'i otsingu .NET SDK versiooni 1.1 | Microsoft Azure'i | Majutatud pilveteenuses otsing"
   description="Azure'i otsingu .NET SDK versiooni 1.1 versioonile üleminekut"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Azure'i otsingu .NET SDK versioon 2.0-Preview üleminekut

Kui kasutate versiooni 1.1 või vanemate [Azure'i otsingu .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), see artikkel aitab teil rakenduse kasutamiseks järgmise põhiversiooni 2.0 – eelvaate täiendamine.

Sh näiteid SDK üldisema lühiülevaade teemast [Azure otsingu kaudu .net-i rakenduse kasutamine](search-howto-dotnet-sdk.md).

Azure'i otsingu .NET SDK versioon 2.0 eelvaade sisaldab varasemate versioonide muudatusi. Need on enamasti väike, nii, et muuta oma kood tuleks nõua ainult vaevaga. Vaadake [juhiseid versioonile](#UpgradeSteps) juhised, kuidas muuta oma koodi kasutada SDK uus versioon.

> [AZURE.NOTE] Kui kasutate 0,13 eelvaates või vanemat versiooni, peaksite versioonile versiooni 1.1 ees- ja seejärel versioonile 2.0 – eelvaade. Vt [liites: juhiseid versiooni 1.1](#UpgradeStepsV1) juhised.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Mis on uut rakenduses versiooni 2.0 – eelvaade

Versioon 2.0 – eelvaade on Azure otsingu .NET SDK Azure'i Search REST API spetsiaalselt 2015-02-28-eelvaade eelvaateversiooni suunata esimese versiooni. See võimaldab kasutada paljudes eelvaade funktsioone Azure'i otsingu .net-i rakendusest, sh järgmist:

- [Kohandatud analüsaatorid](https://aka.ms/customanalyzers)
- [Azure'i bloobimälu](search-howto-indexing-azure-blob-storage.md) ja [Azure Tabelimäluga](search-howto-indexing-azure-tables.md) indekseerija tugi
- Indekseerija kohandamise kaudu [välja vastendused](search-indexer-field-mappings.md)
- ETags toetavad lubamiseks index määratlused, indexers ja andmeallikate turvalised samaaegseid värskendamine
- .NET Core ja .NET Portable profiil 111 tugi

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Juhised täiendamine

Esmalt värskendada oma Nugeti viide `Microsoft.Azure.Search` Nugeti Package Manager konsooli abil või mõõt sisse oma projekti viited paremklõpsata ja valida käsu "Halda Nugeti pakettide..." Visual Studios. Veenduge, et lubate, valides "Kaasata väljalaske-eelne" Nugeti "Halda paketid" väljalaske-eelse pakettide akna Visual Studio või kasutades funktsiooni `-IncludePrerelease` vahetada, kui kasutate Nugeti Package Manager konsooli.

Kui Nugeti on alla laaditud uusi pakette ja nende sõltuvusi, taastada oma projekti. Sõltuvalt sellest, kuidas teie kood on struktureeritud, selle võib taastada edukalt. Jah, kui olete valmis!

Kui teie Koosta nurjub, peaksite nägema Koosta tõrge, näiteks järgmine:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Järgmise sammuna Koosta tõrke lahendamiseks. Mis põhjustab vea ja kuidas seda parandada üksikasjad leiate [suurte muudatuste versiooni 2.0 – eelvaates](#ListOfChanges) .

Teile võidakse kuvada täiendavad Koosta hoiatused seotud aegunud meetodite või atribuudid. Hoiatused sisaldavad juhiseid, mis taunitud funktsiooni asemel kasutada. Näiteks kui teie rakendus kasutab funktsiooni `SearchRequestOptions.RequestId` atribuudi, saate peaks hoiatuse, mis ütleb, et`"This property is deprecated. Please use ClientRequestId instead."`

Kui te lahendasite Koosta vigu, saate uute funktsioonide eeliseid kui soovite rakenduse muudatusi teha. [Mis](#WhatsNew)on uut versiooni 2.0 – preview's on üksikasjalikult SDK uued funktsioonid.

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Suurte muudatuste versioonis 2.0 – eelvaade

On ainult üks server muutunud versioon 2.0 eelvaade, mis võib olla vaja koodi muudatusi Lisaks taastamine rakenduse.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient tagastuse tüüp

Funktsiooni `Indexes.GetClient` meetodil on uue tagastuse tüüp. Varem see tagastatud `SearchIndexClient`, kuid see on muutunud `ISearchIndexClient` versiooni 2.0 – eelvaates. See on klientidele, kes soovivad mõnitama tugi on `GetClient` meetodit üksuse tagastada fiktiivne rakendamine `ISearchIndexClient`.

#### <a name="example"></a>Näide

Kui teie kood näeb välja umbes järgmine:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Saate muuta seda parandada Koosta vigu:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Kokkuvõte
Kui vajate rohkem üksikasju Azure'i otsingu .NET SDK kasutamise kohta, vaadake meie viimati värskendatud [juhiseid](search-howto-dotnet-sdk.md).

Oodatud SDK kohta tagasisidet. Kui teil tekib probleeme, Julgelt Küsi abi [Azure'i otsingu MSDN-i Foorum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Kui te ei leia viga, saate faili probleemi [Azure .NET SDK GitHub hoidla](https://github.com/Azure/azure-sdk-for-net/issues). Veenduge, et teie probleemi tiitli eesliite "Otsi SDK:".

Täname, Azure'i otsingu abil

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Lisa: Juhiseid versiooni 1.1 

> [AZURE.NOTE] See jaotis kehtib ainult Azure otsingu .NET SDK versiooni 0,13-eelvaade ja vanemad kasutajad.

Esmalt värskendada oma Nugeti viide `Microsoft.Azure.Search` Nugeti Package Manager konsooli abil või mõõt sisse oma projekti viited paremklõpsata ja valida käsu "Halda Nugeti pakettide..." Visual Studios.

Kui Nugeti on alla laaditud uusi pakette ja nende sõltuvusi, taastada oma projekti.

Kui olete varem kasutanud versiooni 1.0.0-preview, 1.0.1-preview või 1.0.2-preview, tuleks õnnestub koostamine ja olete valmis!

Kui olete varem kasutanud versioon 0.13.0-preview või vanem, peaksite nägema koostada tõrgete umbes järgmine:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Järgmise sammuna Koosta tõrgete ükshaaval. Enamik on vaja muuta mõne tunni ja meetod nimed, mis on ümber nimetatud SDK. [Suurte muudatuste versiooni 1.1 loend](#ListOfChangesV1) sisaldab loendi nimi muudatustest.

Kui kasutate kohandatud tunnid mudel dokumente ja nende klassid on lihtsad-nullable tüüpi atribuutide (nt `int` või `bool` C#), siis teadke SDK versiooni 1.1 on vigu parandada. Üksikasjalikumat teavet teemast [versiooni 1.1 veaparandused](#BugFixesV1) .

Lõpuks, kui te lahendasite Koosta vigu, saate muudatused ära uusi funktsioone, kui soovite rakenduse.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Suurte muudatuste versiooni 1.1 loend

Järgmises loendis on tellitud tõenäosus muutmine mõjutab oma rakenduse koodi.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch ja IndexAction muudatused

`IndexBatch.Create`on ümber, et `IndexBatch.New` ja ei ole enam mõnda `params` argument. Saate kasutada `IndexBatch.New` töölehed, mille segada erinevaid toiminguid (ühendab, kustutab) jaoks. Lisaks on uue staatilise meetodit loomise pakettidena kui kõik toimingud on samad: `Delete`, `Merge`, `MergeOrUpload`, ja `Upload`.

`IndexAction`pole enam avaliku ehitajatel ja selle atribuudid on nüüd püsiv. Kasutage uue staatilise meetodite loomise toimingute mitmel otstarbel: `Delete`, `Merge`, `MergeOrUpload`, ja `Upload`. `IndexAction.Create`on eemaldatud. Kui kasutasite ülekoormuse, mis võtab ainult dokumendi, veenduge, et kasutada `Upload` asemel.

##### <a name="example"></a>Näide

Kui teie kood näeb välja umbes järgmine:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Saate muuta seda parandada Koosta vigu:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Kui soovite, saate selle täpsemaks lihtsustada järgmine:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException muudatused

Funktsiooni `IndexBatchException.IndexResponse` atribuut on ümber, et `IndexingResults`, ja selle tüüpi on nüüd `IList<IndexingResult>`.

##### <a name="example"></a>Näide

Kui teie kood näeb välja umbes järgmine:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Saate muuta seda parandada Koosta vigu:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Toiming meetod muudatused

Iga toimingu Azure'i otsingu .NET SDK on esitatud kogumi meetod ülekoormuse sünkroonse ja asünkroonse helistajate jaoks. Allkirjad ja nende meetod ülekoormuse, faktooring on muutunud versioonis 1.1.

Näiteks "Saada Index statistika" toimingu vanemates versioonides SDK esitatud allkirja:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Sama toimingut versiooni 1.1 meetod signatuure välja nägema selline:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Alustades versiooni 1.1, Azure'i otsingu .NET SDK korraldab toiming meetodid teisiti:

 - Valikulised parameetrid on nüüd modelleerida pigem parameetrid vaikimisi kui täiendavad meetod ülekoormuse. See meetod ülekoormuse, arv mõnikord oluliselt vähendab.
 - Pikendamise võimalused peitmiseks nüüd palju HTTP kaudu helistaja kõrvalised üksikasju. Näiteks varasemate versioonide SDK tagastatud vastus objekti HTTP oleku koodi, mis ei vaja sageli vaadata, sest toiming meetodite põhjustada `CloudException` jaoks mis tahes olekukoodi, mis näitab viga. Uue pikendamise võimalused lihtsalt tagasi mudeli objektid, säästab vaeva, lahtipakkimiseks need koodi.
 - Seevastu tuum liidesed nüüd jätke meetodid, mis teile rohkem võimalusi HTTP tasemel, kui teil on vaja. Nüüd saab edastada kohandatud HTTP päistes kaasatavate taotlusi ja uue `AzureOperationResponse<T>` tagastuse tüüp pakub otsest juurdepääsu soovitud `HttpRequestMessage` ja `HttpResponseMessage` toimimiseks. `AzureOperationResponse`on määratletud funktsiooni `Microsoft.Rest.Azure` nimeruum ja asendab `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters muudatused

Uue nimega klassi `ScoringParameter` on lisatud uusim SDK lihtsam pakuvad hinded profiilid otsingupäringu parameetrid. Varem on `ScoringProfiles` atribuut on `SearchParameters` klassi tipitud nimega `IList<string>`; Nüüd, kui see on kirjutatud `IList<ScoringParameter>`.

##### <a name="example"></a>Näide

Kui teie kood näeb välja umbes järgmine:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Saate muuta seda parandada Koosta vigu: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Mudeli klassi muudatused

Allkirja tõttu muutub kirjeldatud [toiming meetod muutub](#OperationMethodChanges), palju tunde on `Microsoft.Azure.Search.Models` nimeruum on ümber nimetatud või eemaldatud. Näiteks:

 - `IndexDefinitionResponse`on asendatud`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`Kui soovite uus nimi`DocumentSearchResult`
 - `IndexResult`Kui soovite uus nimi`IndexingResult`
 - `Documents.Count()`Tagastab nüüd on `long` asemel Count dokumendi lisamine`DocumentCountResponse`
 - `IndexGetStatisticsResponse`Kui soovite uus nimi`IndexGetStatisticsResult`
 - `IndexListResponse`Kui soovite uus nimi`IndexListResult`

Kokkuvõttes, `OperationResponse`-saadud tunnid, et ainult mähkimiseks mudeli objekt on eemaldatud. Ülejäänud klassid on olnud nende järelliite muutunud `Response` et `Result`.

##### <a name="example"></a>Näide

Kui teie kood näeb välja umbes järgmine:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Saate muuta seda parandada Koosta vigu:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Vastuse tunnid ja IEnumerable

Täiendavad muutmine, mis võivad mõjutada teie kood on vastus tunnid saidikogumid reguleerivad enam rakendada `IEnumerable<T>`. Selle asemel saate saidikogumi atribuudi juurde otse. Näiteks kui teie kood näeb välja umbes järgmine:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Saate muuta seda parandada Koosta vigu:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Olulise teabe märge veebirakenduste

Kui teil on veebirakendus, mis serializes `DocumentSearchResponse` otse brauseris otsingutulemite saatmiseks peate muuta oma kood või tulemused kuvatakse serialiseerida pole õigesti. Näiteks kui teie kood näeb välja umbes järgmine:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Saate muuta selle saada soovitud `.Results` atribuudi otsingu vastuse Otsingu tulem renderdamise parandamiseks:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

On teil otsida sellisel juhul koodi ise; **Koostaja hoiatab teid ei** Kuna `JsonResult.Data` tüüp on `object`.

#### <a name="cloudexception-changes"></a>CloudException muudatused

Funktsiooni `CloudException` klassi on liikunud soovitud `Hyak.Common` nimeruumi soovitud `Microsoft.Rest.Azure` nimeruum. Lisaks selle `Error` atribuut on ümber, et `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient ja SearchIndexClient muudatused

Tüüp soovitud `Credentials` atribuut on muutunud `SearchCredentials` selle base klassi `ServiceClientCredentials`. Kui teil on vaja juurdepääsu soovitud `SearchCredentials` , on `SearchIndexClient` või `SearchServiceClient`, kasutage uue `SearchCredentials` atribuut.

Vanemates versioonides Tarkvaraarenduskomplektist, `SearchServiceClient` ja `SearchIndexClient` oli ehitajatel, mis leidsid mõne `HttpClient` parameeter. Need on asendatud ehitajatel, mis võtavad mõne `HttpClientHandler` ja massiivi `DelegatingHandler` objektid. See on hõlpsam kohandatud käitleja eelnevalt töödelda HTTP päringuid vajaduse korral installida.

Lõpetuseks, mis leidsid ehitajatel on `Uri` ja `SearchCredentials` on muutunud. Oletagem näiteks, kui teil on kood, mis näeb välja umbes järgmine:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Saate muuta seda parandada Koosta vigu:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Samuti võtke arvesse, et identimisteabe parameetri tüüp väärtus on muutunud `ServiceClientCredentials`. See ei mõjuta teie koodi alates tõenäoliselt `SearchCredentials` on tuletatud `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Läbides taotluse ID

SDK vanemates versioonides ei saanud määrata taotluse ID on `SearchServiceClient` või `SearchIndexClient` ja see sisaldaks iga taotluse REST API-ga. See on kasulik, kui teil on vaja tugiteenuste oma otsinguteenuse tõrkeotsingu. See on kasulik määramiseks taotluse kordumatu ID iga toimingu jaoks asemel kasutada sama ID kõik toimingud. Seetõttu on `SetClientRequestId` meetodid `SearchServiceClient` ja `SearchIndexClient` on eemaldatud. Selle asemel saate saab edastada taotluse ID iga toimingu meetodi kaudu valikuline `SearchRequestOptions` parameeter.

> [AZURE.NOTE] Tulevaste väljalasete SDK, lisame uue süsteemi häälestuse taotluse ID globaalselt kliendi objektide kooskõlas lähenemisviisi, mida muud Azure'i SDK-d.

#### <a name="example"></a>Näide

Kui teil on kood, mis näeb välja umbes järgmine:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Saate muuta seda parandada Koosta vigu:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Kasutajaliidese nimemuudatuste

Toiming rühma kasutajaliidese nimed kõik muutunud kooskõlas nende vastavate atribuutide nimesid:

 - Tüübi `ISearchServiceClient.Indexes` on ümber nimetatud `IIndexOperations` et `IIndexesOperations`.
 - Tüübi `ISearchServiceClient.Indexers` on ümber nimetatud `IIndexerOperations` et `IIndexersOperations`.
 - Tüübi `ISearchServiceClient.DataSources` on ümber nimetatud `IDataSourceOperations` et `IDataSourcesOperations`.
 - Tüübi `ISearchIndexClient.Documents` on ümber nimetatud `IDocumentOperations` et `IDocumentsOperations`.

See muudatus ei mõjuta teie koodi juhul, kui lõite õppuste katsed nende liideste katsetamiseks tõenäoliselt.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Versiooni 1.1 veaparandusi

Varasemate versioonide Azure'i otsingu .NET SDK seotud sariväljaanne kohandatud mudeli tunde oli viga. Viga võib ilmneda juhul, kui olete loonud kohandatud mudeli klassi atribuut-nullable väärtuse tüüp.

#### <a name="steps-to-reproduce"></a>Juhised probleemi

Luua kohandatud mudel klassi atribuut-nullable väärtuse tüüp. Näiteks lisada avaliku `UnitCount` atribuudi tüüpi `int` asemel `int?`.

Kui soovite indekseerida vaikeväärtust, milleks on seda tüüpi dokumenti (nt 0 `int`), väli on null Azure'i otsing. Kui otsite hiljem dokumendi, kuvatakse `Search` kõne võib põhjustada `JsonSerializationException` kurdavad, et seda ei saa teisendada `null` abil `int`.

Filtrite ei pruugi ka, kuna null kirjutatud registrisse ettenähtud väärtuse asemel ootuspäraselt.

#### <a name="fix-details"></a>Lahendada üksikasjad

Meil on fikseeritud probleemi versiooni 1.1 SDK. Nüüd, kui teil on mudeli klassi umbes järgmine:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

ja seate `IntValue` 0, selle väärtus on nüüd õigesti seeriasertide 0-na kaabel sisse ja 0-na registris talletatud. Round rakendub ka töötab ootuspäraselt.

On ühe võimalikud probleem, millega peaks arvestama seda moodust: kui kasutate-nullable atribuut mudel tüüp, teil on selleks, et **tagada** , et teie registri pole üldse dokumente sisaldavad vastava välja tühiväärtus. Azure'i Search REST API ega SDK aitab teil selle jõustamiseks.

See ei ole lihtsalt hüpoteetilist muret: Kujutage ette stsenaariumi, kus saate lisada uue välja olemasolevat registrit, mille tüüp on `Edm.Int32`. Pärast värskendamist registri määratlemine, kõik dokumendid on tühiväärtus uue välja (kuna on nullable Azure'i otsing). Kui kasutate seejärel ainekursuse mudel koos mitte-nullable `int` selle välja atribuudi, saate mõne `JsonSerializationException` nagu siin, kui proovite laadida dokumente:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Seetõttu soovitame ikkagi, kasutage hea tava oma mudeli klassid nullable tüübid.

Lisateavet selle vea ja määrata, leiate [selle probleemi github](https://github.com/Azure/azure-sdk-for-net/issues/1063).
