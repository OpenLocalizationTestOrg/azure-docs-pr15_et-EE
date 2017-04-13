<properties
    pageTitle="Päringu abil .NET SDK Azure'i otsinguregistri | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Azure'i Otsingus Otsingupäringu koostamiseks ja kasutada otsinguparameetreid otsingutulemuste filtreerimine ja sortimine."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Päringu Azure'i otsinguregistri SDK .net-i abil
> [AZURE.SELECTOR]
- [Ülevaade](search-query-overview.md)
- [Portaal](search-explorer.md)
- [.NET-I](search-query-dotnet.md)
- [ÜLEJÄÄNUD](search-query-rest-api.md)

Selles artiklis näitab teile, kuidas päringu registri [Azure'i otsingu .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)abil.

Enne alustamist juhendis, peaks teil juba [loodud registri Azure'i otsingu](search-what-is-an-index.md) ja [seda andmetega täidetud](search-what-is-data-import.md).

Pange tähele, et kõik proovi kood selles artiklis on kirjutatud C#. Täielik allika leidmiseks [github](http://aka.ms/search-dotnet-howto)kood.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Ma. Azure'i otsingu teenuse päringu api-võti tuvastamine
Nüüd, kui olete loonud registri Azure'i otsing, olete valmis peaaegu probleemi kasutades .NET SDK päringuid. Esmalt peate hankima päringu api-võtmed, mis loodi teile ette valmistatud otsinguteenuse jaoks. .NET SDK saadab see api-võti iga taotluse teenust. Kas teil on kehtiv võti luuakse usalda vahelise rakenduse taotlus ja teenus, mis tegeleb selle saatmise kohta taotluse põhjal.

1. Oma teenuse api-klahvid leidmiseks logige sisse [Azure'i portaal](https://portal.azure.com/)
2. Minge Azure'i otsingu teenuse blade
3. Klõpsake ikooni "Klahve"

Teie teenus on *haldus klahve* ja *päringu võtmed*.

  - Teie esmaseid ja teiseseid *haldus klahve* anda täielikud õigused kõik toimingud, sh võimalus hallata teenust, loomine ja kustutamine registrid, indexers ja andmeallikad. On kaks võtit, nii et saate jätkata teisese võtme kasutada, kui te otsustate taastada primaarvõtme ja vastupidi.
  - Oma *päringu klahvid* kirjutuskaitstud juurdepääsu registrite ja dokumentide ja on tavaliselt jaotatud klientrakenduste, mis probleemi otsing taotlused.

Selleks et päringute registri, saate oma päringu kiirklahvidele. Teie haldus klahve saab kasutada ka päringuid, kuid peaksite järgima päringu klahvi rakenduse koodis järgmiselt seda paremini [vähimate õiguste põhimõte](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Klassi SearchIndexClient eksemplari loomine
Azure'i otsingu .NET SDK väljaandmisel päringud, peate eksemplari loomine soovitud `SearchIndexClient` klassi. See tund on mitu ehitajatel. Soovitud võtab oma otsingu teenuse nimi, index nimi ja `SearchCredentials` objekti parameetrid. `SearchCredentials`murtakse api-tootevõti.

Loob uue allpool koodi `SearchIndexClient` "hotellist" registri ( [Loo Azure'i otsinguregistri .NET SDK abil](search-create-index-dotnet.md)loodud) kasutades väärtusi otsingu teenuse nimi ja api võti, mis on talletatud rakenduse config faili (`app.config` või `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`on mõne `Documents` atribuut. Kõik meetodid, mida soovite päringu Azure'i otsing registrite pakub.

## <a name="iii-query-your-index"></a>III. Päringu oma index
.NET SDK-ga otsimise on sama lihtne kui helistate funktsiooni `Documents.Search` meetodi oma `SearchIndexClient`. See meetod võtab mõne parameetrite, sh Otsitav tekst, koos mõne `SearchParameters` objekt, mida saab kasutada päringu täpsustamiseks.

#### <a name="types-of-queries"></a>Tüüpi päringud
Kaks peamised [päringutüübid](search-query-overview.md#types-of-queries) kasutate on `search` ja `filter`. A `search` otsib kõik teie index _otsitavate_ väljade ühe või mitme tingimuse päringu. A `filter` päringu hindab loogikaavaldis üle kõik _filtreeritavad_ väljad registri.

Otsingud nii filtrid teostamise abil soovitud `Documents.Search` meetod. Otsingufraas võib edastada soovitud `searchText` parameeter, samas kui filtriavaldise võib edastada soovitud `Filter` atribuut on `SearchParameters` klassi. Ilma otsimise filtreerimiseks lihtsalt mööda `"*"` jaoks soovitud `searchText` parameeter. Filtreerimata otsimiseks jäta soovitud `Filter` atribuudi Andmebaasiparooli tühistamine, või liigu on `SearchParameters` näiteks üldse.

#### <a name="example-queries"></a>Näide päringud

Järgmine kood näidis kuvatakse päringu "hotellist" registri loomine [Azure otsinguregistrisse .NET SDK-ga](search-create-index-dotnet.md#DefineIndex)määratletud mitu võimalust. Pange tähele, et dokumendid tagastatakse otsingutulemite esinemisjuhud on `Hotel` klassi, mis on määratletud [Azure'i otsingu abil .NET SDK andmete importimist](search-import-data-dotnet.md#HotelClass). Proovi kood kasutab mõnda `WriteDocuments` meetodit väljund otsingutulemite konsooli. See meetod on kirjeldatud järgmises jaotises.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Otsingutulemite toime
Funktsiooni `Documents.Search` meetod tagastab soovitud `DocumentSearchResult` objekti, mis sisaldab päringu tulemused. Näide eelmises jaotises kasutatud meetodit nimetatakse `WriteDocuments` väljund otsingutulemite konsooli:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Siin on tulemused näha päringute eelmises jaotises eeldades "hotellist" index sisustatakse näidisandmetega [andmete](search-import-data-dotnet.md)importimine Azure'i otsingus SDK .net-i abil.

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Ülaltoodud proovi kood kasutab konsooli väljund otsingutulemid. Samuti peate oma rakenduses otsingutulemite kuvamiseks. Näide sellest, kuidas muuta otsingutulemite MVC ASP.net-i-põhine veebirakenduses [selle valimi github](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) kuvamine
