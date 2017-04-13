<properties
    pageTitle="Andmete üleslaadimiseks Azure'i Search REST API abil | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Saate teada, kuidas andmete üleslaadimine registri Azure'i Search REST API abil."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Azure'i Search REST API abil andmete üleslaadimine
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-data-import.md)
- [.NET-I](search-import-data-dotnet.md)
- [ÜLEJÄÄNUD](search-import-data-rest-api.md)

Selles artiklis näitab teile, kuidas kasutada [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) andmete importimiseks registri Azure'i otsing.

Enne alustamist juhendis, peaks olema juba [loodud registri Azure'i otsing](search-what-is-an-index.md).

Lükake dokumendid oma index REST API abil, et teie väljastate HTTP POST-taotluse oma index URL-i lõpp-punkti. HTTP taotluse keha sisu on JSON objekti, mis sisaldab dokumente lisatud, muudetud või kustutatud.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ma. Azure'i otsingu teenuse administraator api-võti tuvastamine
HTTP päringuid suhtes, kasutades REST API teenust väljaandmisel *iga* API taotlus peab sisaldama api võti, mida ette valmistatud otsinguteenuse jaoks loodud. Kas teil on kehtiv võti luuakse usalda vahelise rakenduse taotlus ja teenus, mis tegeleb selle saatmise kohta taotluse põhjal.

1. Oma teenuse api-klahvid leidmiseks logige sisse [Azure'i portaal](https://portal.azure.com/)
2. Minge Azure'i otsingu teenuse blade
3. Klõpsake ikooni "Klahve"

Teie teenus on *haldus klahve* ja *päringu võtmed*.

  - Teie esmaseid ja teiseseid *haldus klahve* anda täielikud õigused kõik toimingud, sh võimalus hallata teenust, loomine ja kustutamine registrid, indexers ja andmeallikad. On kaks võtit, nii et saate jätkata teisese võtme kasutada, kui te otsustate taastada primaarvõtme ja vastupidi.
  - Oma *päringu klahvid* kirjutuskaitstud juurdepääsu registrite ja dokumentide ja on tavaliselt jaotatud klientrakenduste, mis probleemi otsing taotlused.

Selleks et andmete importimise registri, saate kasutada ühte tootevõtit põhi- või administraator.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Otsustada, millist indekseerimise toimingu kasutamiseks
REST API kasutamisel kuvatakse HTTP POST taotlusi JSON taotluse asutustega Azure'i otsingu index lõpp-punkti URL-i välja. HTTP koosolekukutse kehas JSON objekt sisaldab ühte JSON massiivi nimega "value", mis sisaldab JSON objektide tähistav dokumendid, mida soovite lisada oma index, värskendamine või Kustuta.

"Value" massiivi iga JSON objekti tähistab dokumendi indekseerida. Kõigi nende objektide sisaldab dokumendi võti ja määrab soovitud Indekseerimise toimingu (Laadi üles, kooste, Kustuta, jne). Sõltuvalt sellest, millised on toimingud all valimist ainult teatud väljad tuleb lisada iga dokumendi:

@search.action | Kirjeldus | Iga dokumendi vajalikud väljad | Märkmete
--- | --- | --- | ---
`upload` | Mõne `upload` toiming sarnaneb mõne "upsert" kus dokumenti lisada, kui see on uus ja värskendatud/asendatakse kui see on olemas. | klahv pluss muud väljad, mida soovite määratlemine | Kui värskendamine/asendades olemasoleva dokumendi, mis tahes välja, mis on määratud taotluse on selle välja määratud `null`. See juhtub isegi siis, kui väli on varem määratud muu väärtus.
`merge` | Olemasoleva dokumendi värskendab määratud väljad. Kui dokument pole registris, kooste ei õnnestu. | klahv pluss muud väljad, mida soovite määratlemine | Saate määrata kooste välja asendab olemasoleva välja dokumendis. See hõlmab väljad, mille tüüp on `Collection(Edm.String)`. Oletagem näiteks, et dokument sisaldab välja `tags` väärtusega `["budget"]` ja te kooste väärtusega `["economy", "pool"]` jaoks `tags`, lõppväärtus on `tags` väli `["economy", "pool"]`. See ei ole `["budget", "economy", "pool"]`.
`mergeOrUpload` | See toiming käitub nagu `merge` antud võti dokumendi registris juba olemas. Kui dokument pole, käitub nagu `upload` uue dokumendiga. | klahv pluss muud väljad, mida soovite määratlemine | -
`delete` | Eemaldab määratud dokumenti registri. | ainult võti | Kõik väljad saate määrata, kui võtme välja ignoreeritakse. Kui soovite eemaldada dokumendist üksiku välja, kasutage `merge` hoopis ja lihtsalt seada välja konkreetselt null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. HTTP-päring ehitada ja keha taotlemine
Nüüd, kui olete kogunud registri toimingute jaoks vajalik välja väärtused, olete valmis ehitada tegelik HTTP-päring ja JSON koosolekukutse kehasse teie andmeid importida.

#### <a name="request-and-request-headers"></a>Koosolekukutsete ja taotluse päised
URL-i, peate esitada oma teenuse nimi, index nimi ("hotellist" sel juhul) kui ka proper API versioon (API praegune versioon on `2015-02-28` ajal selle dokumendi avaldamiseks). Peate määratlema selle `Content-Type` ja `api-key` taotlemine päised. Viimase puhul kasutage mõnda oma teenuste haldus klahve.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Koosolekukutse kehasse.

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

Selles näites me kasutame `upload`, `mergeOrUpload`, ja `delete` meie otsingu tegevustena.

Oletame, et see näide "hotellist" index lisatakse juba dokumentide arv. Pange tähele, kuidas me ei ole määramiseks kõik võimalikud dokumendi väljad kasutamisel `mergeOrUpload` ja kuidas ainult määratud dokumendi võti (`hotelId`) kasutamisel `delete`.

Samuti Pange tähele, et saab ainult kaasata kuni 1000 dokumendid (või 16 MB) ühe indekseerimise taotluse.

## <a name="iv-understand-your-http-response-code"></a>IV. Teie HTTP vastuse koodi mõistmine
#### <a name="200"></a>200
Pärast eduka indekseerimise esitamisel saate HTTP vastuse olek koodiga, `200 OK`. HTTP vastuse JSON sisu on järgmine:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Olekukoodi `207` tagastatakse vähemalt ühe üksuse on edukalt indekseeritud. HTTP vastuse JSON sisu sisaldab teavet õnnestu dokumendid.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] See tähendab sageli, et teie otsinguteenuse koormus jõuab punkti, kus indekseerimine taotlusi hakkavad tagasi `503` vastuseid. Sel juhul soovitame mis oma kliendi koodi tagasi välja ja oodake enne proovitakse uuesti. See annab süsteemi aega taastamiseks suurendada võimalusi, et tulevaste taotluste õnnestub. Kiiresti proovitakse uuesti oma taotlusi ainult pikendada olukord.

#### <a name="429"></a>429
Olekukoodi `429` tagastatakse, kui olete ületanud meiliteavitus kohta indeks dokumentide arvu.

#### <a name="503"></a>503
Olekukoodi `503` tagastatakse, kui ükski taotluse üksuste edukalt registrisse. See tõrge tähendab, et süsteemi on suur koormus ja teie taotlus ei saa praegu töödelda.

> [AZURE.NOTE] Sel juhul soovitame mis oma kliendi koodi tagasi välja ja oodake enne proovitakse uuesti. See annab süsteemi aega taastamiseks suurendada võimalusi, et tulevaste taotluste õnnestub. Kiiresti proovitakse uuesti oma taotlusi ainult pikendada olukord.

Dokumendi toimingud ja õnnestus/tõrge vastuseid leiate lisateavet teemast [lisamine, värskendamine ja kustutamine dokumentide](https://msdn.microsoft.com/library/azure/dn798930.aspx). Muud HTTP olek koodid ajalootabelis tagastanud kohta leiate lisateavet teemast [HTTP olekukoodi (Azure'i otsing)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Järgmise
Pärast ilma vaevata luua Azure'i otsinguregistri, saab valmis alustama, emissiooni päringute dokumentide otsimiseks. Üksikasjad leiate [Päringu teie Azure'i otsinguregistrisse](search-query-overview.md) .
