<properties
    pageTitle="Luua registri Azure'i Search REST API abil | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Registri loomine abil Azure'i otsingu HTTP REST API-koodi."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Registri Azure'i Search REST API abil loomine
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-an-index.md)
- [Portaal](search-create-index-portal.md)
- [.NET-I](search-create-index-dotnet.md)
- [ÜLEJÄÄNUD](search-create-index-rest-api.md)


Selles artiklis juhendab teid loomine Azure otsingu [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) , kasutades Azure Search REST API-ga.

Enne pärast selle juhendi ja registri loomise, peaks teil olema juba [loodud teenuse Azure otsing](search-create-service-portal.md).

Registri loomiseks tuleb Azure'i Search REST API abil, on probleemi ühe HTTP POST päringu Azure'i otsingu teenuse URL-i lõpp-punkti. Definitsiooni indeks on toodud taotluse keha regulaarne JSON sisu.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ma. Azure'i otsingu teenuse administraator api-võti tuvastamine
Nüüd, kui on ette valmistatud teenuse Azure otsing, saate väljastada HTTP päringuid vastu teie teenuse URL-i lõpp-punkti REST API abil. Siiski *Kõik* API taotlused peavad sisaldama api võti, mida ette valmistatud otsinguteenuse jaoks loodud. Kas teil on kehtiv võti luuakse usalda vahelise rakenduse taotlus ja teenus, mis tegeleb selle saatmise kohta taotluse põhjal.

1. Oma teenuse api-klahvid leidmiseks logige sisse [Azure'i portaal](https://portal.azure.com/)
2. Minge Azure'i otsingu teenuse blade
3. Klõpsake ikooni "Klahve"

Teie teenus on *haldus klahve* ja *päringu võtmed*.

 - Teie esmaseid ja teiseseid *haldus klahve* anda täielikud õigused kõik toimingud, sh võimalus hallata teenust, loomine ja kustutamine registrid, indexers ja andmeallikad. On kaks võtit, nii et saate jätkata teisese võtme kasutada, kui te otsustate taastada primaarvõtme ja vastupidi.
 - Oma *päringu klahvid* kirjutuskaitstud juurdepääsu registrite ja dokumentide ja on tavaliselt jaotatud klientrakenduste, mis probleemi otsing taotlused.

Selleks et registri loomise, saate kasutada ühte tootevõtit põhi- või administraator.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Azure'i otsinguregistri abil regulaarne JSON määratlemine
Ühe HTTP POST päringu teenust loob teie indeks. HTTP POST-taotluse sisu sisaldab JSON üksikobjektiks, mis määratleb Azure'i otsinguregistri.

1. Esimesele atribuudile JSON objekt on teie registri nimi.
2. Teine atribuudi JSON objekti on JSON massiiv nimega `fields` , mis sisaldab eraldi JSON objekti iga välja indeks. Kõigi nende JSON objektide sisaldavad mitut nime ja väärtuse paarideks iga välja atribuudid sh "nimi", "tüüp" jne.

On oluline hoida oma otsingu kasutaja kogemusi ja ettevõtte vajadustele arvesse oma registri kujundamisel, nagu iga välja peab olema määratud [õige atribuute](https://msdn.microsoft.com/library/azure/dn798941.aspx). Millised väljad rakendada nende atribuutide kontrollimiseks, mis otsida funktsioonid (filtreerimine, faceting, sortimise otsingu jne). Mis tahes atribuut, mida te ei määra, saab vaikimisi lubamiseks vastavate otsingufunktsiooni kui spetsiaalselt keelate.

Selles näites oleme nimega meie index "hotellist" ja meie väljad määratletud järgmiselt:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Valisime hoolikalt iga välja alusel, kuidas me arvate, et neid kasutatakse rakenduses registri atribuudid. Näiteks `hotelId` on kordumatu võti, et inimesed, otsides hotellist tõenäoliselt ei tea, nii, et keelame otsingu välja seadmisega `searchable` et `false`, mis salvestab ruumi indeks.

Pange tähele selle tüüpi indeks täpselt ühe välja `Edm.String` peab olema määratud väljana "key".

Registri määratlus eespool kasutab kohandatud keele analüsaator jaoks soovitud `description_fr` välja, kuna see on mõeldud Prantsuse teksti talletamiseks. Vaadake [teemast MSDN-i tugiteenuste keele](https://msdn.microsoft.com/library/azure/dn879793.aspx) kui ka selle vastav [ajaveebipostitus](https://azure.microsoft.com/blog/language-support-in-azure-search/) keele analüsaatorid kohta lisateabe.

## <a name="iii-issue-the-http-request"></a>III. HTTP taotlus
1. Kasuta definitsiooni index koosolekukutse sisusse, probleemi HTTP POST-taotluse oma Azure'i otsingu teenuse lõpp-punkti URL. URL-i, olla kindel, et kasutada oma teenuse nimi hostinimi ja sellele õige `api-version` nimega päringustringi (API praegune versioon on `2015-02-28` ajal selle dokumendi avaldamiseks).
2. Taotluse päised, määrake soovitud `Content-Type` nimega `application/json`. Peate ka oma teenuse administraator võtit, mida te sammus I selle `api-key` päis.


On teil esitada oma teenuse nimi ja api võti allpool taotlus:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Eduka taotlus, peaksite nägema olekukoodi 201 (loodud). REST API kaudu registri loomise kohta lisateabe saamiseks külastage API viide [MSDN-i](https://msdn.microsoft.com/library/azure/dn798941.aspx). Muud HTTP olek koodid ajalootabelis tagastanud kohta leiate lisateavet teemast [HTTP olekukoodi (Azure'i otsing)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Kui registri töötamise lõpetanud ja soovite selle kustutada, vaid probleemi taotluse HTTP kustutada. Näiteks see on, kuidas me oleks "hotellist" registri kustutada:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Järgmise
Pärast Azure otsingu registri loomise, saab valmis [registrisse sisu üles laadida](search-what-is-data-import.md) nii, et saate alustada andmete otsimine.
