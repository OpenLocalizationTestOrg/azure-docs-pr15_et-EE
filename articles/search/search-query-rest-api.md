<properties
    pageTitle="Päringu abil REST API Azure'i otsinguregistri | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Azure'i Otsingus Otsingupäringu koostamiseks ja kasutada otsinguparameetreid otsingutulemuste filtreerimine ja sortimine."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Päringu Azure'i otsinguregistri REST API abil
> [AZURE.SELECTOR]
- [Ülevaade](search-query-overview.md)
- [Portaal](search-explorer.md)
- [.NET-I](search-query-dotnet.md)
- [ÜLEJÄÄNUD](search-query-rest-api.md)

Selles artiklis näitab teile, kuidas päringu registri [Azure'i Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)abil.

Enne alustamist juhendis, peaks teil juba [loodud registri Azure'i otsingu](search-what-is-an-index.md) ja [seda andmetega täidetud](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Ma. Azure'i otsingu teenuse päringu api-võti tuvastamine
Oluline osa iga otsingu toiming vastu Azure'i Search REST API on *api võti* , mis loodi teile ette valmistatud teenuse kohta. Kas teil on kehtiv võti luuakse usalda vahelise rakenduse taotlus ja teenus, mis tegeleb selle saatmise kohta taotluse põhjal.

1. Oma teenuse api-klahvid leidmiseks logige sisse [Azure'i portaal](https://portal.azure.com/)
2. Minge Azure'i otsingu teenuse blade
3. Klõpsake ikooni "Klahve"

Teie teenus on *haldus klahve* ja *päringu võtmed*.

 - Teie esmaseid ja teiseseid *haldus klahve* anda täielikud õigused kõik toimingud, sh võimalus hallata teenust, loomine ja kustutamine registrid, indexers ja andmeallikad. On kaks võtit, nii et saate jätkata teisese võtme kasutada, kui te otsustate taastada primaarvõtme ja vastupidi.
 - Oma *päringu klahvid* kirjutuskaitstud juurdepääsu registrite ja dokumentide ja on tavaliselt jaotatud klientrakenduste, mis probleemi otsing taotlused.

Selleks et päringute registri, saate oma päringu kiirklahvidele. Teie haldus klahve saab kasutada ka päringuid, kuid peaksite järgima päringu klahvi rakenduse koodis järgmiselt seda paremini [vähimate õiguste põhimõte](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-formulate-your-query"></a>II. Esita oma küsimus
On kaks võimalust [oma REST API abil registrist](https://msdn.microsoft.com/library/azure/dn798927.aspx)otsimiseks. Üks võimalus on HTTP POST taotlus kus oma päringu parameetrid on määratletud JSON objekti koosolekukutse kehasse. Teine võimalus on HTTP GET taotlus kus oma päringu parameetrid on määratletud taotluse URL-i. Pange tähele, et postituse veel [leevendada piirangud](https://msdn.microsoft.com/library/azure/dn798927.aspx) päringu parameetrite kui GET suurus. Seetõttu soovitame postituse enne, kui olete teisiti olukordades, kus abil too oleks mugavam.

POSTITUSE ja saada tuleb sisestada oma *teenuse nimi*, *index nimi*ja proper *API versioon* (API praegune versioon on `2015-02-28` ajal selle dokumendi avaldamiseks) taotluse URL-is. Saada, saab *päringustringi* lõpus olevat URL-i, kus esitate päringu parameetrid. Allpool URL-i vorming:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Vormingu postitust on samad, kuid ainult api-versioon on päringustringi parameetrite.



#### <a name="example-queries"></a>Näide päringud

Järgmisena mõni näide päringute registri nimega "hotellist". GET-ja postitus kuvatakse need päringud.

Termini "eelarve" kogu registrist otsimiseks ja tagastab ainult selle `hotelName` välja:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Filtri rakendamine registri leida hotellist odavam kui 150 $ öösel ja tagastada selle `hotelId` ja `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Kogu registrist, konkreetsel väljal tellimuse (`lastRenovationDate`) laskuvas järjestuses, võtta kahepoolse tulemusi ja kuvada ainult `hotelName` ja `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. HTTP-taotluse esitamine
Nüüd, kui teil on seadnud päringu oma HTTP taotluse URL-i (saada) või isiku (POSTITA) jaoks osa, saate määratleda taotluse päiste ja päringu esitamine.

#### <a name="request-and-request-headers"></a>Koosolekukutsete ja taotluse päised
Peate määratlema taotluse kahe päiste toomine või kolme post:
1. Funktsiooni `api-key` päise peab olema seatud leiate etapis ma ülaltoodud päringu võti. Pange tähele, et saate ka mõne klahvi administraator on `api-key` päis, kuid see on soovitatav kasutada päringu klahvi nagu seda määrab ainult kirjutuskaitstud juurdepääsu registrite ja dokumentide.
2. Funktsiooni `Accept` päise peab olema seatud `application/json`.
3. Post ainult selle `Content-Type` päise seadma ka `application/json`.

HTTP GET-päring abil Azure'i Search REST API lihtsa päringu, mis otsib Termini "motel" "hotellist" registrist otsimiseks allpool:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Siin on sama näide päringu, kasutades HTTP POST seekord.

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Eduka päringu taotluse tulemuseks olekukoodi `200 OK` ja Otsingu tulemused tagastatakse nimega JSON vastuse kehasse. Siin on millised tulemused ülaltoodud päringu välja näeb, eeldades, et "hotellist" index sisustatakse näidisandmetega [Azure'i Search REST API abil andmete importimist](search-import-data-rest-api.md) (Pange tähele, et selle JSON on vormindatud selgust).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Lisateabe saamiseks külastage "Vastus" jaotise [Otsing](https://msdn.microsoft.com/library/azure/dn798927.aspx)dokumendid. Muud HTTP olek koodid ajalootabelis tagastanud kohta leiate lisateavet teemast [HTTP olekukoodi (Azure'i otsing)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
