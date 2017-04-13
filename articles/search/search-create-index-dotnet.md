<properties
    pageTitle="Kasutades .NET SDK Azure'i otsingu registri loomine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Registri loomine abil Azure'i otsingu .NET SDK-koodi."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Kasutades .NET SDK Azure'i otsingu registri loomine
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-an-index.md)
- [Portaal](search-create-index-portal.md)
- [.NET-I](search-create-index-dotnet.md)
- [ÜLEJÄÄNUD](search-create-index-rest-api.md)


Selles artiklis juhendab teid Azure otsingu [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) [Azure'i otsingu .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)abil loomise protsess.

Enne pärast selle juhendi ja registri loomise, peaks teil olema juba [loodud teenuse Azure otsing](search-create-service-portal.md).

Pange tähele, et kõik proovi kood selles artiklis on kirjutatud C#. Täielik allika leidmiseks [github](http://aka.ms/search-dotnet-howto)kood.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ma. Azure'i otsingu teenuse administraator api-võti tuvastamine
Nüüd, kui on ette valmistatud teenuse Azure otsing, olete valmis peaaegu probleemi taotlusi vastu teie teenuse lõpp-punkti .NET SDK abil. Esmalt peate hankima administraator api-võtmed, mis loodi teile ette valmistatud otsinguteenuse jaoks. .NET SDK saadab see api-võti iga taotluse teenust. Kas teil on kehtiv võti luuakse usalda vahelise rakenduse taotlus ja teenus, mis tegeleb selle saatmise kohta taotluse põhjal.

1. Oma teenuse api-klahvid leidmiseks logige sisse [Azure'i portaal](https://portal.azure.com/)
2. Minge Azure'i otsingu teenuse blade
3. Klõpsake ikooni "Klahve"

Teie teenus on *haldus klahve* ja *päringu võtmed*.

  - Teie esmaseid ja teiseseid *haldus klahve* anda täielikud õigused kõik toimingud, sh võimalus hallata teenust, loomine ja kustutamine registrid, indexers ja andmeallikad. On kaks võtit, nii et saate jätkata teisese võtme kasutada, kui te otsustate taastada primaarvõtme ja vastupidi.
  - Oma *päringu klahvid* kirjutuskaitstud juurdepääsu registrite ja dokumentide ja on tavaliselt jaotatud klientrakenduste, mis probleemi otsing taotlused.

Selleks et registri loomise, saate kasutada ühte tootevõtit põhi- või administraator.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Klassi SearchServiceClient eksemplari loomine
Azure'i otsingu .NET SDK kasutamise alustamiseks peate eksemplari loomine soovitud `SearchServiceClient` klassi. See tund on mitu ehitajatel. Soovitud võtab oma otsingu teenuse nimi ja `SearchCredentials` objekti parameetrid. `SearchCredentials`murtakse api-tootevõti.

Alljärgnev kood loob uue `SearchServiceClient` kasutamine väärtuste otsimine teenuse nimi ja api võti, mis on talletatud rakenduse config faili (`app.config` või `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`on ka `Indexes` atribuut. Selle atribuudi pakub kõik meetodid, mida peate looma, loendi, värskendada ja kustutada Azure'i otsingu registrid.

> [AZURE.NOTE] Funktsiooni `SearchServiceClient` klassi haldab teie otsinguteenuse ühendused. Vältimaks avamine liiga palju ühendusi, peaksite proovima jagada ühe eksemplari `SearchServiceClient` oma rakenduse võimalusel. Oma meetodite on sellise ühiskasutuse lubamiseks.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Määratleda Azure'i otsingu registri abil soovitud `Index` klassi
Ühe kõne on `Indexes.Create` meetod loob teie indeks. See meetod suunab parameetrina on `Index` objekti, mis määratleb Azure'i otsinguregistri. Peate looma ka `Index` objekti ja lähtestage see järgmiselt:

1. Määrata selle `Name` atribuut on `Index` objekti nimi oma register.
2. Määrata selle `Fields` atribuut on `Index` objekti massiivi `Field` objektid. Iga soovitud `Field` objektide määratleb välja toimimist oma registris. Saate sisestada välja nimi ehitaja, koos andmetüüp (või stringi väljad analüsaator). Saate seada ka muid atribuute, nagu `IsSearchable`, `IsFilterable`jne.

On oluline hoida oma otsingu kasutaja kogemusi ja ettevõtte vajadustele arvesse oma indeks iga kujundamisel `Field` peab olema määratud [sobiv atribuudid](https://msdn.microsoft.com/library/azure/dn798941.aspx). Millised väljad rakendada nende atribuutide kontrollimiseks, mis otsida funktsioonid (filtreerimine, faceting, sortimise otsingu jne). Mis tahes atribuudi saate konkreetselt pole määratud, kuvatakse `Field` klassi vaikimisi keelamine vastavate otsingufunktsiooni juhul, kui lubate seda täpsemalt.

Selles näites oleme nimega meie index "hotellist" ja meie väljad määratletud järgmiselt:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Valisime hoolikalt atribuudiväärtused iga `Field` põhjal, kuidas me arvate, et neid kasutatakse rakenduses. Näiteks, on tõenäoline, et läheduses otsimine inimeste huvi märksõna vastet, klõpsake selle `description` välja, et võimaldada selle välja Otsingu seadmisega `IsSearchable` et `true`.

Pange tähele selle tüüpi indeks täpselt ühe välja `DataType.String` peab olema määratud väljana _klahvi_ seadmisega `IsKey` et `true` (vt `hotelId` eeltoodud näites).

Registri määratlus eespool kasutab kohandatud keele analüsaator jaoks soovitud `description_fr` välja, kuna see on mõeldud Prantsuse teksti talletamiseks. Vaadake [teemast MSDN-i tugiteenuste keele](https://msdn.microsoft.com/library/azure/dn879793.aspx) kui ka selle vastav [ajaveebipostitus](https://azure.microsoft.com/blog/language-support-in-azure-search/) keele analüsaatorid kohta lisateabe.

> [AZURE.NOTE]  Pange tähele, et sooritab `AnalyzerName.FrLucene` ehitaja, on `Field` saab automaatselt tüüpi `DataType.String` ja on `IsSearchable` seatud `true`.

## <a name="iv-create-the-index"></a>IV. Registri loomine
Nüüd, kui teil on lähtestatud `Index` objekti, saate luua registri lihtsalt helistades `Indexes.Create` sisse oma `SearchServiceClient` objekti:

```csharp
serviceClient.Indexes.Create(definition);
```

Eduka taotlemise meetodit tagastada tavalisel viisil. Näiteks lubamatu parameeter taotluse probleem korral meetod on põhjustada `CloudException`.

Kui registri töötamise lõpetanud ja soovite selle kustutada, helistage soovitud `Indexes.Delete` meetodi oma `SearchServiceClient`. Näiteks see on, kuidas me oleks "hotellist" registri kustutada:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Selle artikli koodi näide kasutab Azure otsingu .NET SDK sünkroonse meetodid lihtne. Soovitame kasutada hoida neid scalable ja reageeri oma rakendustes asünkroonne meetoditest. Näiteks ülaltoodud näidetes võib kasutada `CreateAsync` ja `DeleteAsync` asemel `Create` ja `Delete`.

## <a name="next"></a>Järgmise
Pärast Azure otsingu registri loomise, saab valmis [registrisse sisu üles laadida](search-what-is-data-import.md) nii, et saate alustada andmete otsimine.
