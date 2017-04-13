<properties
   pageTitle="Azure'i otsingu kaudu .net-i rakenduse kasutamine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
   description="Azure'i otsingu kaudu .net-i rakenduse kasutamine"
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
   ms.date="10/06/2016"
   ms.author="brjohnst"/>

# <a name="how-to-use-azure-search-from-a-net-application"></a>Azure'i otsingu kaudu .net-i rakenduse kasutamine

Selles artiklis on lühiülevaade tööks te [Azure'i otsingu .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx). Saate rakendada rikkaliku otsingut Azure'i otsingu abil rakenduse .NET SDK.

## <a name="whats-in-the-azure-search-sdk"></a>Mis on selle Azure SDK otsimine

SDK koosneb kliendi teek, `Microsoft.Azure.Search`. See võimaldab teil hallata registrid, andmeallikate ja indexers, kui ka üles laadida ja dokumentide haldamine ja käivitada päringuid, ilma HTTP ja JSON tegelema.

Kliendi teek määratleb tunnid, nt `Index`, `Field`, ja `Document`, samuti toimingud, nagu `Indexes.Create` ja `Documents.Search` klõpsake soovitud `SearchServiceClient` ja `SearchIndexClient` tunnid. Need klassid on korraldatud järgmised nimeruumid:

- [Microsoft.Azure.Search](https://msdn.microsoft.com/library/azure/microsoft.azure.search.aspx)
- [Microsoft.Azure.Search.Models](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.aspx)

Azure'i otsingu .NET SDK praegune versioon on nüüd üldiselt kättesaadav. Kui soovite lisada järgmise versiooni meile tagasiside andmiseks, külastage meie [leht Tagasiside](https://feedback.azure.com/forums/263029-azure-search/).

.NET SDK toetab versiooni `2015-02-28` Azure Search REST API, dokumenteerida [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)-is. See versioon on nüüd Lucene päringusüntaksit ja Microsoft keele analüsaatorid tugi. Uuemad funktsioonid, mis on *osa selle versiooni, nt tugi* on `moreLikeThis` otsida parameeter, [Preview](search-api-2015-02-28-preview.md) ja pole veel saadaval SDK. Kontrollige uuesti olekuvärskendusi, kas funktsiooni [Search teenuse Versioonimine](https://msdn.microsoft.com/library/azure/dn864560.aspx) .

Muud funktsioonid, mis ei toeta seda SDK on järgmised.

  - [Haldustoimingute](https://msdn.microsoft.com/library/azure/dn832684.aspx)kohta. Toimingute hulka kuuluvad Azure otsingu teenuste ettevalmistamine ja haldamine API võtmed. Neid ei toetata eraldi Azure'i otsingu .net-i halduse SDK tulevikus.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>SDK uusima versiooni uuendamine

Kui kasutate Azure otsingu .NET SDK varasem versioon ja soovite uue versiooni üldiselt kättesaadav, [selles artiklis](search-dotnet-sdk-migration.md) selgitatakse, kuidas.

## <a name="requirements-for-the-sdk"></a>Nõuded SDK

1. Visual Studio 2013 või Visual Studio 2015.

2. Oma Azure'i otsinguteenuse. SDK kasutamiseks on vaja teenust ja ühe või mitme API funktsiooni nime. [Loo portaalis teenuse](search-create-service-portal.md) aitab teil neid juhiseid järgides.

3. Laadige Azure'i otsingu .NET SDK [Nugeti pakett](http://www.nuget.org/packages/Microsoft.Azure.Search) "Halda Nugeti paketid" Visual Studio abil. Otsi paketi nimi `Microsoft.Azure.Search` NuGet.org kohta.

Azure'i otsingu .NET SDK toetab rakenduste suunatud .NET Framework 4.5 nagu Windowsi poe rakenduste Windows 8.1 ja Windows Phone 8.1 suunamise. Silverlighti ei toetata.

## <a name="core-scenarios"></a>Core stsenaariumid

On mitmeid asju, peate tegema oma otsingurakenduse. Selles õpetuses hõlmab järgmisi core stsenaariumid:

- Registri loomine
- Ilma vaevata luua registri dokumentidega
- Otsingu ja filtrite dokumentide otsimine

Proovi kood järgneva iseloomustab kõik need. Võite kasutada funktsiooni Koodilõigud oma rakenduses.

### <a name="overview"></a>Ülevaade

Uurime uurides rakenduse näidis loob uue nimega "hotellist" index kuvab selle mõned dokumendid ja seejärel aktiveeritakse mõned otsingupäringuid. Siin on programmi, näitab üldine kulgemist:

    // This sample shows how to delete, create, upload documents and query an index
    static void Main(string[] args)
    {
        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();
    }

Juhendame see samm-sammult. Kõigepealt on vaja luua uus `SearchServiceClient`. Selle objekti võimaldab teil hallata registrid. Ehitada üks, peate pakkuda oma Azure'i otsingu teenuse nimi on administraatori API võti.

        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

> [AZURE.NOTE] Kui esitate on vale võti (nt päringu klahvi kui ka administraator võti oli vaja), on `SearchServiceClient` võib põhjustada mõne `CloudException` viga sõnumi "Keelatud" esimest korda, kui helistate sisestusmeetod toimingut, näiteks `Indexes.Create`. Sel juhul kontrollige, kas meie API võti.

Järgmise mõned read kõne meetodite registri loomiseks tuleb nimega "hotellist", kustutage see esmalt, kui see on juba olemas. Me juhendab nende meetodite pisut hiljem.

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

Järgmiseks registri peab olema täidetud. Selle tegemiseks läheb vaja on `SearchIndexClient`. On kaks võimalust selle hankida: see ehitamine, helistades `Indexes.GetClient` kohta on `SearchServiceClient`. Kasutame viimase mugavuse.

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

> [AZURE.NOTE] Tüüpilised otsingurakenduse index haldus ja populatsiooni teostab otsingupäringute eraldi komponendi. `Indexes.GetClient`on mugav ilma vaevata luua registri, kuna säästab probleeme anda teise `SearchCredentials`. See sooritab administraator võti loomiseks kasutatud funktsiooni `SearchServiceClient` uus `SearchIndexClient`. Teie rakendus, mis käivitab päringute osa, on parem luua selle `SearchIndexClient` otse nii, et te saate päringu võti asemel mõne administraator klahvi. See on kooskõlas vähimate õiguste põhimõte ja aitab muuta rakenduse kasutamist turvalisemaks. Lisateavet haldus klahve ja päringu võtmed leiate [siit](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Nüüd, kui meil on `SearchIndexClient`, meil saate asustada indeks. Seda muul viisil, mis kuvatakse tutvustame hiljem.

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

Lõpuks me käivitada mõne otsingupäringuid ja Kuva tulemused, uuesti, kasutades funktsiooni `SearchIndexClient`:

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();

Kui käivitate selle rakenduse kehtiv teenusleping nimi ja API võti, väljund peaks välja nägema selline:

    Deleting index...

    Creating index...

    Uploading documents...

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []
    Complete.  Press any key to end application...

Täielik lähtekoodi rakendus on esitatud käesoleva artikli lõpus.

Järgmiseks võtame lähemalt kutsunud meetodi `Main`.

### <a name="creating-an-index"></a>Registri loomine

Loomise järel on `SearchServiceClient`, järgmine asi `Main` ei kustuta on index "hotellist", kui see on juba olemas. Mis on tehtud järgmisel viisil:

    private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
    {
        if (serviceClient.Indexes.Exists("hotels"))
        {
            serviceClient.Indexes.Delete("hotels");
        }
    }

See meetod kasutab funktsiooni antud `SearchServiceClient` märkige see ruut, kui indeks on olemas, ja kui jah, kustutage see.

> [AZURE.NOTE] Selle artikli koodi näide kasutab Azure otsingu .NET SDK sünkroonse meetodid lihtne. Soovitame kasutada hoida neid scalable ja reageeri oma rakendustes asünkroonne meetoditest. Näiteks eeltoodud meetodi võib kasutada `ExistsAsync` ja `DeleteAsync` asemel `Exists` ja `Delete`.

Järgmise, `Main` loob uue "hotellist" index seda meetodit helistades:

    private static void CreateHotelsIndex(SearchServiceClient serviceClient)
    {
        var definition = new Index()
        {
            Name = "hotels",
            Fields = new[]
            {
                new Field("hotelId", DataType.String)                       { IsKey = true },
                new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
            }
        };

        serviceClient.Indexes.Create(definition);
    }

See meetod loob uue `Index` objekti loendi `Field` objekte, mis määratleb skeemi uus indeks. Iga välja on mitu atribuute, mis määratlevad selle otsingu käitumine nimi ja andmetüüp. Lisaks väljad, võite sisestada ka hinded profiilid, suggesters või CORS suvandid indeks (need on välja jäetud valimist lühike jaoks). Lisateavet registri objekti ja selle osade leiate SDK viide [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.index_members.aspx)-is kui ka [Azure Search REST API viide](https://msdn.microsoft.com/library/azure/dn798935.aspx).

### <a name="populating-the-index"></a>Ilma vaevata luua registri

Järgmine juhis `Main` asustamiseks äsja loodud indeks on. Seda tehakse järgmisel viisil:

    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var documents =
            new Hotel[]
            {
                new Hotel()
                {
                    HotelId = "1058-441",
                    HotelName = "Fancy Stay",
                    BaseRate = 199.0,
                    Category = "Luxury",
                    Tags = new[] { "pool", "view", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                    Rating = 5,
                    Location = GeographyPoint.Create(47.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "666-437",
                    HotelName = "Roach Motel",
                    BaseRate = 79.99,
                    Category = "Budget",
                    Tags = new[] { "motel", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                    Rating = 1,
                    Location = GeographyPoint.Create(49.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "970-501",
                    HotelName = "Econo-Stay",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "pool", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(46.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "956-532",
                    HotelName = "Express Rooms",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "wifi", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(48.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "566-518",
                    HotelName = "Surprisingly Expensive Suites",
                    BaseRate = 279.99,
                    Category = "Luxury",
                    ParkingIncluded = false
                }
            };

        try
        {
            var batch = IndexBatch.Upload(documents);
            indexClient.Documents.Index(batch);
        }
        catch (IndexBatchException e)
        {
            // Sometimes when your Search service is under load, indexing will fail for some of the documents in
            // the batch. Depending on your application, you can take compensating actions like delaying and
            // retrying. For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
        }

        // Wait a while for indexing to complete.
        Thread.Sleep(2000);
    }

Sellel meetodil on neli osa. Esimene loob massiivi `Hotel` objekte, mis toimib meie sisendandmete registri üleslaadimiseks. Andmed on raske koodiga jaoks lihtne. Oma rakenduses oma andmete tulevad tõenäoliselt näiteks SQL-andmebaasi välisest andmeallikast.

Teine osa loob mõne `IndexBatch` sisaldavad dokumendid. Teie määratud toimingu rakendamiseks paketi loomist, sel juhul helistades ajal `IndexBatch.Upload`. Azure'i Otsinguindeksi, seejärel laadida paketi selle `Documents.Index` meetod.

> [AZURE.NOTE] Selles näites me lihtsalt üleslaadimise dokumendid. Kui soovite muudatused ühendada olemasolevad dokumendid või dokumentide kustutamine, saate luua pakettidena helistades `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, või `IndexBatch.Delete` asemel. Võite ka segada erinevaid toiminguid ühe paketina, helistades `IndexBatch.New`, mis võtab kogumi `IndexAction` objekte, mis ütleb Azure'i Otsing dokumendi teatud toimingu sooritamiseks. Saate luua iga `IndexAction` tegevusest helistades vastavate meetodit nagu `IndexAction.Merge`, `IndexAction.Upload`, jne.

Selle meetodi kolmas osa on saagi Blokeeri, mis tegeleb olulised tõrge puhul indekseerimine. Oma Azure'i otsinguteenuse nurjumisel registrisse osa dokumentide paketi, mis `IndexBatchException` on `Documents.Index`. See võib juhtuda siis, kui teil on indekseerimine dokumentide ajal teenust on suur koormus. **Soovitame tungivalt konkreetselt töötlemise sel juhul koodi.** Saate viivitamine ja proovige seejärel uuesti indekseerimine nurjus dokumentide või saate sisse logida ja jätkake nagu valimi ei või saate teha midagi muud, sõltuvalt teie taotlus andmete järjepidevuse nõuetele.

Lõpuks viis viivitused kaks sekundit all. Indekseerimise juhtub asünkroonselt Azure'i otsingu teenust, nii, et valimi rakendus peab ootama lühike kellaaeg tagamaks, et dokumendid on saadaval otsimiseks. Viivitusi, nagu see on tavaliselt ainult demos, kontrollib ja valimi rakenduste korral.

#### <a name="how-the-net-sdk-handles-documents"></a>Kuidas käsitleb .NET SDK dokumendid

Võib tekkida, kuidas Azure'i otsingu .NET SDK on üles laadida nagu kasutaja määratletud klassi eksemplarid `Hotel` indeks. Selleks, et küsimusele vastata, vaatame selle `Hotel` klassi:

    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }

Pange tähele, et esmalt on see, et iga avaliku atribuudi `Hotel` vastab väljale registri määratlemine, kuid on üks oluline erinevus: iga välja nimi algab väiketähed kirja ("kaamel väiketähti"), samal ajal iga avaliku atribuudi nimi `Hotel` käivitab suurtähed tähega "(Pascal juhul). See on stsenaarium, kus sooritamiseks andmete Köitmine, kus target skeem on rakenduse arendaja ei kontrolli .net-i rakendusi. Selle asemel, et minna vastuollu .net-i nime panemise juhised, muutes atribuudi nimed camel puhul, öelda SDK vastendamiseks atribuutide nimesid automaatselt camel puhul on `[SerializePropertyNamesAsCamelCase]` atribuut.

> [AZURE.NOTE] Azure'i otsingu .NET SDK kasutab [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) teegi serialiseerida ja andmeatribuutide oma kohandatud mudeli objektid ja sealt JSON. Vajaduse korral saate selle sariväljaanne kohandada. Lisateavet leiate teemast [Kohandatud sariväljaanne JSON.NET](#JsonDotNet).
 
Teine oluline teave on `Hotel` klassi on avalik atribuutide andmetüübid. .Net-i tüüpi atribuutidest Vastendage oma võrdväärse Väljatüübid index määratlus. Näiteks kuvatakse `Category` string atribuut kaardid on `category` välja, mille tüüp on `Edm.String`. On sarnane tüüp vaheliste vastenduste `bool?` ja `Edm.Boolean`, `DateTimeOffset?` ja `Edm.DateTimeOffset`jne. Teatud tüüpi vastenduse reeglid on avaldatud koos selle `Documents.Get` meetod [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx)-is.

See võimalus kasutada oma tunnid dokumentidena töötab mõlemast suunast; Saate otsingutulemite toomiseks ja on SDK automaatselt andmeatribuutide neid oma valiku tüüp, nagu järgmises jaotises näeme.

> [AZURE.NOTE] Azure'i otsingu .NET SDK toetab ka dünaamiliselt tipitud dokumente, kasutades funktsiooni `Document` klassi, mis on /-väärtuse kaardistamine väljanimede välja väärtused. See on kasulik stsenaariumid, kui te ei tea, index skeemi koostamise ajal või kui oleks ebamugav sidumiseks konkreetse mudeli tunnid. Kõik meetodid SDK, mis käsitlevad dokumendid on ülekoormuse, kus on `Document` klassi, samuti tugevalt tipitud ülekoormuse, mis võtavad üldine tüüp parameeter. Ainult viimase kasutatakse proovi kood selles õpetuses. Võite leida lisateavet kohta on `Document` klassi [siin](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Tähtis teade andmetüübid**

Oma mudeli tunnid vastendamiseks Azure'i otsingu registri kujundamisel soovitame deklareerimise atribuutide väärtus tüübid, näiteks `bool` ja `int` olema nullable (nt `bool?` asemel `bool`). Kui kasutate-nullable atribuut, peate selleks, et **tagada** , et teie registri pole üldse dokumente sisaldavad tühiväärtus vastava välja. Azure'i otsinguteenuse ega SDK aitab teil selle jõustamiseks.

See ei ole lihtsalt hüpoteetilist muret: Kujutage ette stsenaariumi, kus saate lisada uue välja olemasolevat registrit, mille tüüp on `Edm.Int32`. Pärast värskendamist registri määratlemine, kõik dokumendid on tühiväärtus uue välja (kuna on nullable Azure'i otsing). Kui kasutate seejärel ainekursuse mudel koos mitte-nullable `int` selle välja atribuudi, saate mõne `JsonSerializationException` nagu siin, kui proovite laadida dokumente:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Seetõttu soovitame kasutada hea tava oma mudeli klassid nullable tüübid.

<a name="JsonDotNet"></a>
#### <a name="custom-serialization-with-jsonnet"></a>Kohandatud sariväljaanne JSON.NET

SDK kasutab JSON.NET jaotamine ja deserializing dokumendid. Saate kohandada sariväljaanne ja vajadusel määratledes oma deserialization `JsonConverter` või `IContractResolver` ( [JSON.NET dokumentatsiooni kohta](http://www.newtonsoft.com/json/help/html/Introduction.htm) lisateabe saamiseks vt). See võib olla kasulik, kui soovite kohandada oma rakenduse kasutamiseks Azure'i otsingu-ja muude täpsemate stsenaariumid olemasolevasse mudeli klassi. Näiteks kohandatud sariväljaanne saate teha järgmist.

 - Kaasamine või välistamine teatud mudeli tunni atribuudid on salvestatud dokumendi väljadena.
 - Vastendage atribuutide nimed koodi ja väljanimede indeks.
 - Saate luua kohandatud atribuute, mida saab kasutada nii dokumendi väljade ja atribuutide vastendamise samuti loomise vastava registri määratlemine.

Leiate Azure'i otsingu .NET SDK github ühiku testide kohandatud sariväljaanne rakendamise näited. Hea alguspunkt on [see kaust](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). See sisaldab tunnid, mida kasutavad kohandatud sariväljaanne testide.

### <a name="searching-for-documents-in-the-index"></a>Registris olev dokumentide otsimine

Viimases etapis valimi rakenduse on mõned registri otsing. Seda teeb järgmisel viisil:

    private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
    {
        // Execute search based on search text and optional filter
        var sp = new SearchParameters();

        if (!String.IsNullOrEmpty(filter))
        {
            sp.Filter = filter;
        }

        DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
        foreach (SearchResult<Hotel> result in response.Results)
        {
            Console.WriteLine(result.Document);
        }
    }

Kõigepealt see meetod loob uue `SearchParameters` objekti. Seda kasutatakse päringu nt sortimine, filtreerimine, lehtede nummerdamine ja faceting täiendavate suvandite määramiseks. Selles näites me ainult seades selle `Filter` atribuut.

Järgmiseks on tegelikult päringu otsing. Seda tehakse abil soovitud `Documents.Search` meetod. Sel juhul võtame Otsitav tekst stringina kasutada lisaks varem loodud otsingu parameetrid. Me ei määrata ka `Hotel` jaoks parameetrina tüüp `Documents.Search`, mis ütleb SDK deserialiseerimiseks dokumentide otsingutulemite objektid, mille tüüp on `Hotel`.

Lõpuks seda meetodit itereerib läbi kõik vasted otsingutulemites konsooli iga dokumendi printimine.

Vaatame lähemalt uurida, kuidas seda meetodit nimetatakse:

    SearchDocuments(indexClient, searchText: "fancy wifi");

    SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

Esimene, Otsime kõik dokumendid, mis sisaldavad päringu terminite "ilustatud" või "wifi". Teine kõne Otsitav tekst on seatud "*", mis tähendab "Otsi kõike". Leiate otsingu päringu avaldisesüntaksi lisateabe saamiseks [siin](https://msdn.microsoft.com/library/azure/dn798920.aspx).

Teine kõne kasutab OData `$filter` avaldis, `category eq 'Luxury'`. See piirab otsingu tagastab ainult dokumendid kui selle `category` välja vastab täpselt string "Luxury". Lisateavet Azure otsingu toetava OData süntaks leiate [siit](https://msdn.microsoft.com/library/azure/dn798921.aspx).

Nüüd, kui teate, et need kaks kõnesid teha, tuleks oleks mugavam vaadata, miks nende väljund näeb välja umbes järgmine:

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []

Esimene otsing tagastab kahe dokumendid. Esimene on "Meeldib" nimi, kuigi teine "wifi" on `tags` välja. Teine otsing tagastab kahe dokumendid, mis juhtub olema registri ainult dokumente, millel on selle `category` väärtuseks "Luxury".

See toiming on lõpule jõudnud õpetuse, kuid ei takista siin. **Järgmised toimingud** pakub täiendavaid ressursse rohkem teada Azure'i otsing.

## <a name="next-steps"></a>Järgmised sammud

- Sirvige viiteid [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) ja [REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) MSDN-is.
- Süvendada oma teadmisi kuni [videote ja muude näidised ja õpetused](search-video-demo-tutorial-list.md).
- Lugege funktsioone ja võimalusi Azure'i otsingu SDK selles versioonis: [Azure'i otsingu ülevaade](https://msdn.microsoft.com/library/azure/dn798933.aspx)
- Vaadake üle [Nimepaneku tavad](https://msdn.microsoft.com/library/azure/dn857353.aspx) reeglid erinevad objektidele nime panemise õppida.
- Vaadake üle [toetatud andmetüübid](https://msdn.microsoft.com/library/azure/dn798938.aspx) Azure'i otsing.


## <a name="sample-application-source-code"></a>Lähtekoodi näidis

Siin on täielik lähtekoodi valimi rakendus, mida kasutatakse selle jalutuskäiguga kaudu. Pange tähele, et peate teenuse nimi ja API võti kohatäidete Program.cs asendamiseks oma väärtustega, kui soovite luua ja käivitada valimi.

Program.CS:

```csharp
using System;
using System.Configuration;
using System.Linq;
using System.Threading;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    class Program
    {
        // This sample shows how to delete, create, upload documents and query an index
        static void Main(string[] args)
        {
            // Put your search service name here. This is the hostname portion of your service URL.
            // For example, if your service URL is https://myservice.search.windows.net, then your
            // service name is myservice.
            string searchServiceName = "myservice";

            string apiKey = "Put your API admin key here.";

            SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

            Console.WriteLine("{0}", "Deleting index...\n");
            DeleteHotelsIndexIfExists(serviceClient);

            Console.WriteLine("{0}", "Creating index...\n");
            CreateHotelsIndex(serviceClient);

            ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

            Console.WriteLine("{0}", "Uploading documents...\n");
            UploadDocuments(indexClient);

            Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
            SearchDocuments(indexClient, searchText: "fancy wifi");

            Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
            SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

            Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("hotels"))
            {
                serviceClient.Indexes.Delete("hotels");
            }
        }

        private static void CreateHotelsIndex(SearchServiceClient serviceClient)
        {
            var definition = new Index()
            {
                Name = "hotels",
                Fields = new[]
                {
                    new Field("hotelId", DataType.String)                       { IsKey = true },
                    new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                    new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                    new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                    new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                    new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
                }
            };

            serviceClient.Indexes.Create(definition);
        }

        private static void UploadDocuments(ISearchIndexClient indexClient)
        {
            var documents =
                new Hotel[]
                {
                    new Hotel()
                    {
                        HotelId = "1058-441",
                        HotelName = "Fancy Stay",
                        BaseRate = 199.0,
                        Category = "Luxury",
                        Tags = new[] { "pool", "view", "concierge" },
                        ParkingIncluded = false,
                        LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                        Rating = 5,
                        Location = GeographyPoint.Create(47.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "666-437",
                        HotelName = "Roach Motel",
                        BaseRate = 79.99,
                        Category = "Budget",
                        Tags = new[] { "motel", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                        Rating = 1,
                        Location = GeographyPoint.Create(49.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "970-501",
                        HotelName = "Econo-Stay",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "pool", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(46.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "956-532",
                        HotelName = "Express Rooms",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "wifi", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(48.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "566-518",
                        HotelName = "Surprisingly Expensive Suites",
                        BaseRate = 279.99,
                        Category = "Luxury",
                        ParkingIncluded = false
                    }
                };

            try
            {
                var batch = IndexBatch.Upload(documents);
                indexClient.Documents.Index(batch);
            }
            catch (IndexBatchException e)
            {
                // Sometimes when your Search service is under load, indexing will fail for some of the documents in
                // the batch. Depending on your application, you can take compensating actions like delaying and
                // retrying. For this simple demo, we just log the failed document keys and continue.
                Console.WriteLine(
                    "Failed to index some of the documents: {0}",
                    String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
            }

            // Wait a while for indexing to complete.
            Thread.Sleep(2000);
        }

        private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
        {
            // Execute search based on search text and optional filter
            var sp = new SearchParameters();

            if (!String.IsNullOrEmpty(filter))
            {
                sp.Filter = filter;
            }

            DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
            foreach (SearchResult<Hotel> result in response.Results)
            {
                Console.WriteLine(result.Document);
            }
        }
    }
}
```

Hotel.CS:

```csharp
using System;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }
}
```
