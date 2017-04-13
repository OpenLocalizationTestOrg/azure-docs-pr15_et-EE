<properties
    pageTitle="Andmete üleslaadimiseks Azure'i otsingu abil .NET SDK | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Saate teada, kuidas Azure'i otsingus kasutades .NET SDK registri andmete üleslaadimiseks."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Azure'i otsingu abil .NET SDK andmete üleslaadimine
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-data-import.md)
- [.NET-I](search-import-data-dotnet.md)
- [ÜLEJÄÄNUD](search-import-data-rest-api.md)

Selles artiklis näitab teile, kuidas [Azure'i otsingu .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) abil andmete importimine registri Azure'i otsing.

Enne alustamist juhendis, peaks olema juba [loodud registri Azure'i otsing](search-what-is-an-index.md). Ka selles artiklis eeldatakse, et olete juba loonud mõne `SearchServiceClient` objekti, nagu on näidatud [loomine Azure otsinguregistri .NET SDK abil](search-create-index-dotnet.md#CreateSearchServiceClient).

Pange tähele, et kõik proovi kood selles artiklis on kirjutatud C#. Täielik allika leidmiseks [github](http://aka.ms/search-dotnet-howto)kood.

Selleks, et teie kasutades .NET SDK index lükake dokumentide, peate:

  1. Luua mõne `SearchIndexClient` objekti otsinguregistri ühenduse.
  2. Saate luua ka `IndexBatch` sisaldavate dokumentide lisada, muuta või kustutada.
  3. Kõne on `Documents.Index` meetodit oma `SearchIndexClient` saata selle `IndexBatch` oma otsinguregistrisse.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Ma. Klassi SearchIndexClient eksemplari loomine
Andmete importimine oma index Azure'i otsingu .NET SDK abil peate eksemplari loomine soovitud `SearchIndexClient` klassi. Saate koostada eksemplar ise, kuid see on lihtsam kui teil on juba mõne `SearchServiceClient` eksemplari helistamiseks selle `Indexes.GetClient` meetod. Näiteks siin on, kuidas te soovite saada on `SearchIndexClient` nimega "hotellist" kaudu indeks on `SearchServiceClient` nimega `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] Tüüpilised otsingurakenduse index haldus ja populatsiooni teostab eraldi komponendi otsingupäringuid. `Indexes.GetClient`on mugav ilma vaevata luua registri, kuna säästab probleeme anda teise `SearchCredentials`. See sooritab administraator võti loomiseks kasutatud funktsiooni `SearchServiceClient` uus `SearchIndexClient`. Teie rakendus, mis käivitab päringute osa, on parem luua selle `SearchIndexClient` otse nii, et te saate päringu võti asemel mõne administraator klahvi. See on [vähimate õiguste põhimõte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ja aitab muuta rakenduse kasutamist turvalisemaks. Siit leiate lisateavet haldus klahve ja päringu võtmed [Azure'i Search REST API viide MSDN-is](https://msdn.microsoft.com/library/azure/dn798935.aspx).

`SearchIndexClient`on mõne `Documents` atribuut. Selle atribuudi pakub kõik meetodid, mida soovite lisamine, muutmine, kustutamine või päring oma registri dokumendid.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Otsustada, millist indekseerimise toimingu kasutamiseks
Saate importida andmed, kasutades .NET SDK, peate sisse oma andmed paketti on `IndexBatch` objekti. Mõne `IndexBatch` kapseloi kogumi `IndexAction` objekte, mis sisaldab dokumendi ja atribuut, mis ütleb Azure'i otsingu toimingu sooritamiseks dokument (Laadi üles, kooste, Kustuta, jne). Sõltuvalt sellest, millised on toimingud all valimist ainult teatud väljad tuleb lisada iga dokumendi:

Toiming | Kirjeldus | Iga dokumendi vajalikud väljad | Märkmete
--- | --- | --- | ---
`Upload` | Mõne `Upload` toiming sarnaneb mõne "upsert" kus dokumenti lisada, kui see on uus ja värskendatud/asendatakse kui see on olemas. | klahv pluss muud väljad, mida soovite määratlemine | Kui värskendamine/asendades olemasoleva dokumendi, mis tahes välja, mis on määratud taotluse on selle välja määratud `null`. See juhtub isegi siis, kui väli on varem määratud muu väärtus.
`Merge` | Olemasoleva dokumendi värskendab määratud väljad. Kui dokument pole registris, kooste ei õnnestu. | klahv pluss muud väljad, mida soovite määratlemine | Saate määrata kooste välja asendab olemasoleva välja dokumendis. See hõlmab väljad, mille tüüp on `DataType.Collection(DataType.String)`. Oletagem näiteks, et dokument sisaldab välja `tags` väärtusega `["budget"]` ja te kooste väärtusega `["economy", "pool"]` jaoks `tags`, lõppväärtus on `tags` väli `["economy", "pool"]`. See ei ole `["budget", "economy", "pool"]`.
`MergeOrUpload` | See toiming käitub nagu `Merge` antud võti dokumendi registris juba olemas. Kui dokument pole, käitub nagu `Upload` uue dokumendiga. | klahv pluss muud väljad, mida soovite määratlemine | -
`Delete` | Eemaldab määratud dokumenti registri. | ainult võti | Kõik väljad saate määrata, kui võtme välja ignoreeritakse. Kui soovite eemaldada dokumendist üksiku välja, kasutage `Merge` hoopis ja lihtsalt seada välja konkreetselt null.

Saate määrata toimingu, mida soovite kasutada staatilise erineval soovitud `IndexBatch` ja `IndexAction` klassid, nagu on näidatud järgmises jaotises.

## <a name="iii-construct-your-indexbatch"></a>III. Ehitada oma IndexBatch
Nüüd, kui teate, milliseid toiminguid teha dokumentidega, olete valmis ehitada selle `IndexBatch`. Järgmises näites kujutatakse paketi loomine mõne muu toimingute. Pange tähele, et siinses näites kasutab kohandatud klassi nimetatakse `Hotel` mis kaardid "hotellist" registri dokumendi.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

Selles näites me kasutame `Upload`, `MergeOrUpload`, ja `Delete` kutsunud meetodite määratletud meie otsingu tegevustena on `IndexAction` klassi.

Oletame, et see näide "hotellist" index lisatakse juba dokumentide arv. Pange tähele, kuidas me ei ole määramiseks kõik võimalikud dokumendi väljad kasutamisel `MergeOrUpload` ja kuidas ainult määratud dokumendi võti (`HotelId`) kasutamisel `Delete`.

Samuti, võtke arvesse, et te ainult võib sisaldada kuni 1000 dokumentide ühe indekseerimise päringu.

> [AZURE.NOTE] Selles näites me on erinevaid toiminguid rakendamine eri dokumentide. Kui soovite sama toiminguid üle kõik dokumendid paketi, selle asemel, et helistate `IndexBatch.New`, võite kasutada muude staatilise meetodid `IndexBatch`. Näiteks saate luua pakettidena helistades `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, või `IndexBatch.Delete`. Nende meetodite võtta dokumendikogumi (objekte, mis tüüpi `Hotel` selles näites) asemel `IndexAction` objektid.

## <a name="iv-import-data-to-the-index"></a>IV. Andmete importimine indeks
Nüüd, kui teil on lähtestatud `IndexBatch` objekti, saate selle saata indeks helistades `Documents.Index` sisse oma `SearchIndexClient` objekti. Järgmises näites kujutatakse helistamiseks `Index`, samuti nagu mõned lisatoimingud, peate tegema:

```csharp
try
{
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

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Märkus selle `try` / `catch` ümbritseva kõne edastamiseks selle `Index` meetod. Blokeeri saagi tegeleb olulised tõrge puhul indekseerimine. Oma Azure'i otsinguteenuse nurjumisel registrisse osa dokumentide paketi, mis `IndexBatchException` on `Documents.Index`. See võib juhtuda siis, kui teil on indekseerimine dokumentide ajal teenust on suur koormus. **Soovitame tungivalt konkreetselt töötlemise sel juhul koodi.** Saate viivitamine ja proovige seejärel uuesti indekseerimine nurjus dokumentide või saate sisse logida ja jätkake nagu valimi ei või saate teha midagi muud, sõltuvalt teie taotlus andmete järjepidevuse nõuetele.

Lõpuks kood ülaloleval viivitused kaks sekundit all. Indekseerimise juhtub asünkroonselt Azure'i otsingu teenust, nii, et valimi rakendus peab ootama lühike kellaaeg tagamaks, et dokumendid on saadaval otsimiseks. Viivitusi, nagu see on tavaliselt ainult demos, kontrollib ja valimi rakenduste korral.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Kuidas käsitleb .NET SDK dokumendid

Võib tekkida, kuidas Azure'i otsingu .NET SDK on üles laadida nagu kasutaja määratletud klassi eksemplarid `Hotel` indeks. Selleks, et küsimusele vastata, vaatame selle `Hotel` klassi, mis on registri skeemi loomine [Azure otsinguregistri .NET SDK-ga](search-create-index-dotnet.md#DefineIndex)määratletud kaardid:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Pange tähele, et esmalt on see, et iga avaliku atribuudi `Hotel` vastab väljale registri määratlemine, kuid on üks oluline erinevus: iga välja nimi algab väiketähed kirja ("kaamel väiketähti"), samal ajal iga avaliku atribuudi nimi `Hotel` käivitab suurtähed tähega "(Pascal juhul). See on stsenaarium, kus sooritamiseks andmete Köitmine, kus target skeem on rakenduse arendaja ei kontrolli .net-i rakendusi. Selle asemel, et minna vastuollu .net-i nime panemise juhised, muutes atribuudi nimed camel puhul, öelda SDK vastendamiseks atribuutide nimesid automaatselt camel puhul on `[SerializePropertyNamesAsCamelCase]` atribuut.

> [AZURE.NOTE] Azure'i otsingu .NET SDK kasutab [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) teegi serialiseerida ja andmeatribuutide oma kohandatud mudeli objektid ja sealt JSON. Vajaduse korral saate selle sariväljaanne kohandada. Rohkem üksikasju leiate [Azure'i otsingu .NET SDK versiooni 1.1 versioonile](search-dotnet-sdk-migration.md#WhatsNew). Üks näide on kasutada funktsiooni `[JsonProperty]` atribuut klõpsake soovitud `DescriptionFr` atribuudi ülaltoodud proovi kood.

Teine oluline teave on `Hotel` klassi on avalik atribuutide andmetüübid. .Net-i tüüpi atribuutidest Vastendage oma võrdväärse Väljatüübid index määratlus. Näiteks kuvatakse `Category` string atribuut kaardid on `category` välja, mille tüüp on `DataType.String`. On sarnane tüüp vaheliste vastenduste `bool?` ja `DataType.Boolean`, `DateTimeOffset?` ja `DataType.DateTimeOffset`jne. Teatud tüüpi vastenduse reeglid on avaldatud koos selle `Documents.Get` meetod [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx)-is.

See võimalus kasutada oma tunnid dokumentidena töötab mõlemast suunast; Saate otsingutulemite toomiseks ja on SDK automaatselt andmeatribuutide neid oma valiku tüüp, nagu on näidatud [järgmisel artikkel](search-query-dotnet.md).

> [AZURE.NOTE] Azure'i otsingu .NET SDK toetab ka dünaamiliselt tipitud dokumente, kasutades funktsiooni `Document` klassi, mis on /-väärtuse kaardistamine väljanimede välja väärtused. See on kasulik stsenaariumid, kui te ei tea, index skeemi koostamise ajal või kui oleks ebamugav sidumiseks konkreetse mudeli tunnid. Kõik meetodid SDK, mis käsitlevad dokumendid on ülekoormuse, kus on `Document` klassi, samuti tugevalt tipitud ülekoormuse, mis võtavad üldine tüüp parameeter. Selles artiklis proovi kood kasutatakse ainult. Võite leida lisateavet kohta on `Document` klassi [MSDN-is](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Tähtis teade andmetüübid**

Oma mudeli tunnid vastendamiseks Azure'i otsingu registri kujundamisel soovitame deklareerimise atribuutide väärtus tüübid, näiteks `bool` ja `int` olema nullable (nt `bool?` asemel `bool`). Kui kasutate-nullable atribuut, peate selleks, et **tagada** , et teie registri pole üldse dokumente sisaldavad tühiväärtus vastava välja. Azure'i otsinguteenuse ega SDK aitab teil selle jõustamiseks.

See ei ole lihtsalt hüpoteetilist muret: Kujutage ette stsenaariumi, kus saate lisada uue välja olemasolevat registrit, mille tüüp on `DataType.Int32`. Pärast värskendamist registri määratlemine, kõik dokumendid on tühiväärtus uue välja (kuna on nullable Azure'i otsing). Kui kasutate seejärel ainekursuse mudel koos mitte-nullable `int` selle välja atribuudi, saate mõne `JsonSerializationException` nagu siin, kui proovite laadida dokumente:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Seetõttu soovitame kasutada hea tava oma mudeli klassid nullable tüübid.

## <a name="next"></a>Järgmise
Pärast ilma vaevata luua Azure'i otsinguregistri, saab valmis alustama, emissiooni päringute dokumentide otsimiseks. Üksikasjad leiate [Päringu teie Azure'i otsinguregistrisse](search-query-overview.md) .
