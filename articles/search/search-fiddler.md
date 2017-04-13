<properties
    pageTitle="Viiuldaja hinnata ja testida Azure'i Search REST API-de kasutamine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Kasutage viiuldaja kood tasuta kontrollib saadavuse Azure'i otsimine ja proovida REST API-de lähenemisviisi."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Hindab ja testida Azure'i Search REST API-de viiuldaja abil
> [AZURE.SELECTOR]
- [Ülevaade](search-query-overview.md)
- [Otsingu Explorer](search-explorer.md)
- [Viiuldaja](search-fiddler.md)
- [.NET-I](search-query-dotnet.md)
- [ÜLEJÄÄNUD](search-query-rest-api.md)

Selles artiklis selgitatakse, kuidas kasutada Fiddler, mis on saadaval [tasuta alla laadida Telerik](http://www.telerik.com/fiddler), probleemi HTTP päringuid ja Kuva vastuste Azure'i Search REST API kasutamine ilma koodi kirjutamiseks. Azure'i otsing on täielikult õnnestus, majutatud otsingu pilveteenuses Microsoft Azure, hõlpsasti programmeeritavat .net-i ja REST API-de kaudu. [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)-is on avaldatud Azure'i otsinguteenuse REST API-d.

Järgmistes juhistes kuvatakse registri loomine, dokumentide üleslaadimine, päringu registri ja seejärel päringu teenuseteabe süsteem.

Nende juhiste täitmiseks peate teenuse Azure otsingu ja `api-key`. Vaadake [portaalis Azure otsinguteenuse](search-create-service-portal.md) juhised, kuidas alustada.

## <a name="create-an-index"></a>Registri loomine

1. Käivitage viiuldaja. Klõpsake menüüs **fail** **Jäädvustada liikluse** peitmiseks kõrvalised HTTP tegevust, mis on seotud praeguse tööülesande välja lülitada.

3. Menüü **helilooja** tuleb koostada taotlus, mis näeb välja umbes järgmine pilt.

    ![][1]

2. Valige **panna**.

3. Sisestage URL, mis määrab teenuse URL-i, taotluse atribuute ja api-versioon. Mõned näpunäited, mida arvesse:
   + Kasutage HTTPS-i eesliide.
   + Koosolekukutse atribuut on "/ registrid/hotellist". See ütleb otsingu nimega "hotellist" registri loomine.
   + API-versioon on väiketähtedes, määratud kujul "? api-versioon 2015-02-28 =". API versioonid on oluline, sest Azure otsingu kasutajaks värskendused regulaarselt. Harva, võib teenuse update tutvustada API server muutmine. Seetõttu nõuab Azure'i otsingu api-versioon iga taotluse nii, et teil on täielik kasutusõigus üle, millise neist kasutatakse.

    Täielik URL peaks välja nägema umbes järgmine näide.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Määrake taotluse päis, asendades host ja api võti väärtused, mis sobivad teenust.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Koosolekukutse kehasse kleepida väljad, mis moodustavad registri määratlemine.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klõpsake **käivitada**.

Mõne sekundi, peaksite nägema vastuse HTTP 201 seanss loendist, mis näitab, register on loodud.

Kui teil tekib HTTP 504, veenduge, et URL-i määrab HTTPS. Kui näete HTTP 400 või 404, märkige ruut Taotle sisu kinnitamiseks ilmnenud tõrkeid kopeerimine ja kleepimine. Mõne HTTP 403 näitab, et probleem api võti (kehtetu võtme või kuidas api-võti on määratud süntaks probleem).

## <a name="load-documents"></a>Asetage dokumendid

Klõpsake menüü **helilooja** näeb teie taotlus postitada dokumente järgmist. Taotluse sisu sisaldab 4 hotellist otsingu andmeid.

   ![][2]

1. Valige **POSTITA**.

2.  Sisestage URL, mis algab HTTPS-i, millele järgneb teie teenuse URL-i, millele järgneb "/indexes/ < indexname >' / dokumendid/register? api-versioon 2015-02-28 =". Täielik URL peaks välja nägema umbes järgmine näide.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Koosolekukutse päis peab olema sama nagu enne. Pidage meeles, et asendada väärtused, mis sobivad teenust host ja api võti.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Koosolekukutse sisusse sisaldab nelja dokumentide hotellist registri lisada.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klõpsake **käivitada**.

Mõne sekundi, peaksite loendis seansi vastuse HTTP 200. See näitab, et dokumendid on nüüd loodud. Kui teil tekib 207, vähemalt ühe dokumendi üleslaadimine nurjus. Kui teil tekib 404, peate süntaksiviga päisesse või kutse keha.

## <a name="query-the-index"></a>Päringu register

Nüüd, kui registri ja dokumentide laaditakse, saate väljastada päringute nende vastu.  Klõpsake menüü **helilooja** sarnanevad päringute teenust käsu **saada** järgmine pilt.

   ![][3]

1.  Valige **saada**.

2.  Sisestage URL, mis algab HTTPS-i, millele järgneb teie teenuse URL-i, millele järgneb "/indexes/ < indexname >' / dokumentide?", millele järgneb päringu parameetrid. Kasutage näiteks järgmine URL, asendades valimi hostinimi üks, mis on teie teenuse jaoks lubatud.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Selle päringu Termini "motel" otsib ja laadib hinnangute tahukas kategooriad.

3.  Koosolekukutse päis peab olema sama nagu enne. Pidage meeles, et asendada väärtused, mis sobivad teenust host ja api võti.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Vastuse kood tuleks 200 ja vastuse väljundi peaks välja nägema umbes järgmine pilt.

   ![][4]

Näiteks järgmine päring on MSDN-is [otsinguregistri toiming (Azure'i otsingu API)](http://msdn.microsoft.com/library/dn798927.aspx) . Selles teemas näide päringute sisaldavad tühikuid, mis pole lubatud viiuldaja. Iga space koos märk + enne kleepimist päringu string enne, kui proovite päringu viiuldaja Asenda.

**Enne tühikud asendatakse:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Pärast tühikud on asendatud +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Päringu süsteemi

Samuti saate teha päringuid dokumendi loendab ja salvestusruumi süsteem. Menüü **helilooja** teie taotlus näeb välja umbes järgmine ja vastus tagastab tulemuseks arvu dokumentide ja kasutatud ruumi.

 ![][5]

1.  Valige **saada**.

2.  Sisestage URL, mis sisaldab teie teenuse URL-i, millele järgneb "/ statistika/registrid/hotellist? api-versioon 2015-02-28 =":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Määrake taotluse päis, asendades host ja api võti väärtused, mis sobivad teenust.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Koosolekukutse sisusse tühjaks jätta.

5.  Klõpsake **käivitada**. Peaksite nägema HTTP 200 olekukoodi, seanss loendist. Valige oma käsu postitatud kirje.

6.  Klõpsake vahekaarti **inspektorid** , klõpsake vahekaarti **päised** ja valige JSON-vormingus. Peaksite nägema selle dokumendi count ja salvestusruumi maht (KB).

## <a name="next-steps"></a>Järgmised sammud

Teemast [Azure oma otsinguteenuse haldamine](search-manage.md) ei nõua koodi lähenemine haldamine ja Azure otsingu abil.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
