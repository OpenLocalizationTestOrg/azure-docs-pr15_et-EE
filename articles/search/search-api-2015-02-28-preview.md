<properties
   pageTitle="Azure'i otsingu teenuse REST API versioon 2015-02-28-eelvaade | Microsoft Azure'i | Azure'i otsingu eelvaade API"
   description="Azure'i otsingu teenuse REST API versioon 2015-02-28-eelvaade kuuluvad katse loomulikus keeles analüsaatorid ja moreLikeThis otsinguid."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure'i otsingu teenuse REST API: Versioon 2015-02-28-eelvaade

See artikkel on viide dokumentatsioonist `api-version=2015-02-28-Preview`. Selles eelvaates laiendab üldiselt kättesaadav praegune versioon, [api-versioon = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), andes katse järgmised funktsioonid:

- `moreLikeThis`päringu parameetrite [Otsingu dokumentide](#SearchDocs) API. See otsib muud dokumendid, mis on seotud teise kindla dokumendi.

Mõned täiendavad osad on `2015-02-28-Preview` REST API on avaldatud eraldi. Järgmised:

- [Hinded profiilid](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexers](search-api-indexers-2015-02-28-preview.md)

Azure'i otsinguteenuse on saadaval mitut versiooni. Üksikasju vaadake [Otsingu teenuse](http://msdn.microsoft.com/library/azure/dn864560.aspx) versioone.

## <a name="apis-in-this-document"></a>Selles dokumendis API-d

Azure'i otsingu teenuse API toetab kahte URL-i lauseõpetuse API-analüüsitoiminguid: lihtne ja OData (vt [OData (Azure'i otsingu API) toe](http://msdn.microsoft.com/library/azure/dn798932.aspx) üksikasjad). Järgmises loendis on esitatud lihtsa süntaks.

[Registri loomine](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Registri värskendamine](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Saada Index](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Registrite loetelu](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Saada register statistika](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Testige analüsaator](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Registri kustutamine](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Lisa, Kustuta ja registri andmete värskendamine](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Dokumentide otsimine](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Dokumendi otsing](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Dokumentide arv](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Soovitused](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Registri toimingud

Saate luua ja hallata registrite Azure'i otsinguteenuse kaudu lihtne HTTP päringuid (Kustuta postituse toomine, pane) suhtes antud index ressursi. Registri loomiseks esimese kirja JSON dokument, mis kirjeldab index skeemi. Skeem määratleb registri, nende andmetüübid ja kuidas neid kasutada (nt väljaandes otsinguid kogu teksti, filtrid, sortimise või faceting) väljad. Samuti määratletakse hinded profiilid, suggesters ja muude atribuutide konfigureerimine registri toimimist.

Järgmises näites on toodud joonise skeemi otsimiseks teha teavet määratletud kaks tekst väljale Kirjeldus. Pange tähele, kuidas määrata atribuute, kuidas kasutatakse välja. Näiteks kuvatakse `hotelId` kasutatakse dokumendi võti (`"key": true`) ja on välistatud täisteksti otsingud (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Kui indeks on loodud, laaditakse dokumente, mis asustada registri. Järgmise toimingu [Lisa või Värskenda dokumentide](#AddOrUpdateDocuments) kuvamine

Video tutvustus indekseerimine Azure'i otsingus, leiate [Azure'i otsingu episood kanali 9 Cloud katta](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Registri loomine

Registri on peamised vahendid korraldamine ja dokumentide Azure'i otsing, mis on sarnane kuidas tabeli korraldab andmebaasi kirjete otsimine. Iga indeks on dokumendikogumi kõik vastavad index skeemi (väljanimesid, andmetüübid ja atribuudid), et registrid määrata ka täiendavad kooste (suggesters, hinded profiilid ja CORS suvandid), mis määratlevad muud käitumised otsing.

Saate luua uue registri abil HTTP postituse või pane taotluse Azure'i otsingu teenuses. Taotluse sisu on JSON skeem, mis täpsustab, register ja konfiguratsiooni teave.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Teise võimalusena saate kasutada ja määrata URI indeksi nimi. Kui indeks olemas, luuakse see.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Registri loomise määratleb dokumentide talletatud ja kasutada otsingu struktuuri. Ilma vaevata luua registri on eraldi toiming. Selle toimingu abil saate ka [indekseerija](https://msdn.microsoft.com/library/azure/mt183328.aspx) on (Toetatud andmeallikate jaoks saadaval) või [lisamine, värskendamine või dokumentide kustutamine](https://msdn.microsoft.com/library/azure/dn798930.aspx) toimingut. Pööratud indeks luuakse dokumentide sisestamisel.

**Märkus**: registrite lubatud maksimumarv oleneb hinnad taseme. Tasuta teenus võimaldab kuni 3 registrid. Standard teenus võimaldab 50 registrid otsinguteenuse kohta. Üksikasjad leiate [piirangud ja piirangutega](http://msdn.microsoft.com/library/azure/dn798934.aspx) .

**Taotlemine**

HTTPS on vaja kõik hooldustaotlused. **Registri loomine** taotluse saate ehitatakse postituse või pane meetodi abil. POSTITUSE kasutamisel peate sisestama indeksi nime koosolekukutse kehasse koos registri skeemi määratlemine. Registri nime panna, on osa URL-i. Kui indeks pole olemas, on loodud. Kui see on juba olemas, värskendatakse uue määratluse.

Indeksi nimi peab olema väiketähtedes, käivitage tähe või numbri, on pole kaldkriipsud või punkti ja olema väiksem kui 128 märki. Pärast indeksi nimi algab tähe või numbri, ülejäänud nimi võib sisaldada mis tahes tähe, arv ja kriipsud, kui kriipsud mittejärjestikuste.

Funktsiooni `api-version` on nõutav. Saadaolevate versioonide loendi leiate [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `Content-Type`: Nõutav. See määramine`application/json`
- `api-key`: Nõutav. Funktsiooni `api-key` on kasutatud
- autentida taotluse otsingu teenust. See on kordumatu teenust stringiväärtus. **Registri loomine** taotluse peab sisaldama mõne `api-key` päise määramine teie administraator võti (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

<a name="RequestData"></a>
**Koosolekukutse kehasse süntaks**

Taotluse sisu sisaldab skeemi määratlus, mis sisaldab andmeid väljaloendi dokumente, mis rikastavad selle indeksi, andmetüübid, atribuutide, samuti valikuline loendit hinded profiilid, mis on kasutatud vastavaid dokumente päringu ajal sees.

Pange tähele, et POST taotlus, tuleb teil määrata indeksi nimi koosolekukutse kehasse.

Seal saab ainult üks väli indeks. See peab olema string välja. Väli tähistab iga dokumendi, mis on talletatud registri ainuidentifikaator.

Järgmised registri põhiosa.

- `name`
- `fields`mis rikastavad selle indeksi, sh nimi, andmetüüp ja atribuudid, mis määratlevad selle välja lubatud toimingud.
- `suggesters`kasutada automaatteksti või ettetippimise päringud.
- `scoringProfiles`kasutada kohandatud otsingu Keskmine järjestamine. Üksikasjad leiate [Lisa hinded profiilid](https://msdn.microsoft.com/library/azure/dn798928.aspx) .
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` abil saate määratleda, kuidas teie dokumente ja päringud jagatud registreerida/Otsitav sõned. Üksikasjad leiate [Azure'i otsingu analüüsi](https://aka.ms//azsanalysis) .
- `defaultScoringProfile`kirjutada hinded käitumist vaikimisi kasutada.
- `corsOptions`Kui soovite lubada rist-origin päringute vastu indeks.

Taotluse last struktureerimine süntaks on järgmine. Valimi taotlus on esitatud selles teemas edasi.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Registri atribuudid**

Registri loomisel, saate määrata järgmised atribuudid. Hinded ja hinded profiilide kohta leiate üksikasjalikumat teavet teemast [lisada hinded profiilid](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Määrab välja nimi.

`type`-Määrab välja andmetüüp. Lugege teemat [Toetatud andmetüübid](#DataTypes) toetatud failitüüpide loendit.

`searchable`-Märgib välja nagu täisteksti otsingu võimalus. See tähendab, et see läbivad näiteks poolitus ajal indekseerimine. Kui määratud on `searchable` välja nagu "päike päeval", ettevõttesiseselt selle väärtuseks on jagatud üksikute sõned "päike" ja "päeva". See võimaldab täisteksti otsib järgmiste terminite tähendust. Väljad, mille tüüp on `Edm.String` või `Collection(Edm.String)` on `searchable` vaikimisi. Muud tüüpi välja ei saa `searchable`.

  - **Märkus**: `searchable` väljad peab olema oma index lisaruumi Kuna Azure'i otsingu salvestab täiendavad tokenized versiooni täisteksti otsingupäringutest välja väärtust. Kui soovite oma index ruumi säästmiseks ning te ei pea välja kaasatavate otsingud, määrake `searchable` abil `false`.

`filterable`– Võimaldab välja viidata `$filter` päringud. `filterable`erineb `searchable` kuidas stringide käsitletakse sisse. Väljad, mille tüüp on `Edm.String` või `Collection(Edm.String)` , mis on `filterable` läbima poolitus, et võrdlused on täpsed vasted ainult. Näiteks kui seate sellise välja `f` "päike päeval", et `$filter=f eq 'sunny'` otsib vasteid, kuid `$filter=f eq 'sunny day'` kuvatakse. Kõik väljad on `filterable` vaikimisi.

`sortable`-Vaikimisi süsteemi sordib tulemuste Keskmine, kuid palju kogemusi kasutajad soovite sortida väljade dokumendid. Väljad, mille tüüp on `Collection(Edm.String)` ei saa `sortable`. Kõik muud väljad on `sortable` vaikimisi.

`facetable`– Otsingutulemite esitluse, mis sisaldab tabas count kategooriate kaupa (näiteks otsimine digi ja vaadake tabamust kaubamärgi, megapikslit, hind, jne.) tavaliselt kasutatakse. See suvand ei saa kasutada väljad, mille tüüp on `Edm.GeographyPoint`. Kõik muud väljad on `facetable` vaikimisi.

  - **Märkus**: väljad, mille tüüp on `Edm.String` , mis on `filterable`, `sortable`, või `facetable` võib olla kuni 32 KB pikkus. Selle põhjuseks sellised väljad käsitletakse ühe otsingutermin ja Azure otsingus Termini maksimumpikkus on 32KB. Kui teil on vaja salvestada rohkem kui see tekst väljale ühe stringi, peate konkreetselt määratud `filterable`, `sortable`, ja `facetable` et `false` index definitsiooni sisse.

  - **Märkus**: kui väli on ükski eeltoodud atribuutide määramine `true` (`searchable`, `filterable`, `sortable`, või`facetable`) välja jäetakse tõhus pööratud indeks. See suvand on kasulik väljad, mida ei kasutata päringute, kuid on vaja otsingutulemites. Välja sellised väljad registri parandab jõudlust.

`key`-Märgib välja sisaldavaks kordumatute tunnuste dokumentide piires indeks. Täpselt ühe välja tuleb valida soovitud `key` välja ja see peab olema tüüpi `Edm.String`. Võtmeväljade saab otsida dokumente otse [Otsingu API](#LookupAPI)kaudu.

`retrievable`-Määrab, kas väli tagastatavad otsingutulemus sisse.  See on kasulik, kui soovite kasutada (nt veeris) välja filtrina, sortimise või punktide andmise, kuid ei soovi olla nähtavad ka lõppkasutaja välja. See atribuut peab olema `true` jaoks `key` väljad.

`analyzer`-Määrab Analyzer kasutada otsingu ja indekseerimise aja välja nimi. Lubatud väärtuste kogumi, leiate teemast [analüsaatorid](https://msdn.microsoft.com/library/mt605304.aspx). See suvand saab kasutada ainult `searchable` väljad ja seda ei saa määrata, kas koos `searchAnalyzer` või `indexAnalyzer`.  Kui analüsaator on valitud, ei saa muuta välja.

`searchAnalyzer`-Määrab Analyzer kasutada otsingu ajal välja nimi. Lubatud väärtuste kogumi, leiate teemast [analüsaatorid](https://msdn.microsoft.com/library/mt605304.aspx). See suvand saab kasutada ainult `searchable` väljad. Peab olema seatud koos `indexAnalyzer` ja seda ei saa määrata koos selle `analyzer` suvand. See analyzer saab värskendada olemasoleva välja.

`indexAnalyzer`-Määrab Analyzer kasutada indekseerimise ajal välja nimi. Lubatud väärtuste kogumi, leiate teemast [analüsaatorid](https://msdn.microsoft.com/library/mt605304.aspx). See suvand saab kasutada ainult `searchable` väljad. Peab olema seatud koos `searchAnalyzer` ja seda ei saa määrata koos selle `analyzer` suvand. Kui analüsaator on valitud, ei saa muuta välja.

`suggesters`-Määrab otsingu režiimi ja soovituste sisu allika väljad. Üksikasjad leiate [Suggesters](#Suggesters) .

`scoringProfiles`-Määratleb kohandatud hinded käitumist, mis võimaldavad teil mõjutada millised üksused kuvatakse otsingutulemites kõrgema. Hinded profiil koosneb välja kaalu ja funktsioonid. Hinded profiili kasutatakse atribuutide kohta lisateabe saamiseks vaadake [lisamine hinded profiilid](https://msdn.microsoft.com/library/azure/dn798928.aspx) .

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Keelte tugi**

Otsitavate väljade läbima analüüsi selle kõige sagedamini hõlmab poolitus, teksti normaliseerimine ja filtreerimisega tingimustel. Vaikimisi analüüsitakse otsitavate väljad Azure'i [Apache Lucene Standard analüsaator](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) , jaotab teksti elemente, järgides["Unicode'i teksti osadeks"](http://unicode.org/reports/tr29/) . Lisaks standard analüsaator teisendab kõik märgid nende väiketähti. Indekseeritud dokumendid nii otsinguterminid läbida analüüsi indekseerimine ja päringu töötlemise ajal.

Azure'i otsingu toetab erinevaid keeli. Iga keele jaoks on vaja mittestandardsed teksti analüsaator, mis moodustab antud keele omadused. Azure'i otsing pakub kahte tüüpi analüsaatorid:

- 35 analüsaatorid Lucene toetavad.
- 50 analüsaatorid kaitstud Microsoft loomulikus keeles töötlemise tehnoloogia kasutusel Office ja Bingi toetavad.

Mõned arendajad soovida Lucene tuttavat lihtne, avatud lähtekoodi lahendus. Lucene analüsaatorid on kiirem, kuid Microsoft analüsaatorid on täiustatud võimalused, nt lemmatization, Wordi decompounding (nt saksa, Taani, hollandi, Rootsi, Norra, Eesti, valmis, Ungari, slovaki keeles) ja üksuse tuvastus (URL-id, e-kirju, kuupäevad, arvud). Võimaluse korral käivitate Microsoft nii Lucene analüsaatorid otsustada, milline neist on paremini sobib võrrelda.

***Kuidas need võrdlus***

Inglise keele Lucene analyzer laiendab standard analyzer. Omastavad asesõnad (lõputühikud 's) eemaldab sõnu, mis ühe [algoritmi Porter mis](http://tartarus.org/~martin/PorterStemmer/)kehtib ja eemaldab inglise keele [sõnu peatamine](http://en.wikipedia.org/wiki/Stop_words).

Võrdluse, teeb Microsoft analyzer lemmatization asemel mis. See tähendab, et seda võib käsitleda käändelist ja korrapäratu Wordis vormide palju parem mis tulemuste asjakohasemate otsingutulemite (Vaata moodul 7 [Azure'i otsingu MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) esitluse rohkem üksikasju).

Indekseerimise abil Microsoft analüsaatorid on Keskmine aeglasem kui Lucene, olenevalt keele kaks kuni kolm korda. Otsinguks ei tohiks keskmise suurusega päringuid märkimisväärselt mõjutada.

***Konfigureerimine***

Iga väli index määratlus, saate määrata selle `analyzer` analüsaator nimi, mida saate määrata, milline keel ja tarnija atribuut. Sama analüsaator rakendatakse siis, kui indekseerimise ja otsingu välja.
Näiteks saate määrata inglise, prantsuse ja Hispaania teha kirjeldus, mis on olemas kõrvuti sama registri eraldi väljadel. Määrake väli, kus keelekohaste otsimiseks vastu päringute ['searchFields' Päringuparameetri](#SearchQueryParameters) abil. Saate vaadata päringu näited, mis sisaldavad soovitud `analyzer` atribuudi [Otsi dokumente](#SearchDocs). 

***Analüsaator loend***

Allpool on toetatud keelte koos Lucene ja Microsoft analüsaator nimede loend.

<table style="font-size:12">
    <tr>
        <th>Keel</th>
        <th>Microsofti analyzer nimi</th>
        <th>Lucene analüsaator nimi</th>
    </tr>
    <tr>
        <td>Araabia</td>
        <td>AR.Microsoft</td>
        <td>AR.lucene</td>      
    </tr>
    <tr>
        <td>Armeenia</td>
        <td></td>
        <td>HY.lucene</td>
    </tr>
    <tr>
        <td>Bangla</td>
        <td>BN.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baski</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>Bulgaaria</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>Katalaani</td>
        <td>ca.Microsoft</td>
        <td>ca.lucene</td>          
    </tr>
    <tr>
        <td>Lihtsustatud hiina</td>
        <td>ZH-Hans.microsoft</td>
        <td>ZH-Hans.lucene</td>     
    </tr>
    <tr>
        <td>Traditsiooniline hiina</td>
        <td>ZH-Hant.microsoft</td>
        <td>ZH-Hant.lucene</td>     
    <tr>
    <tr>
        <td>Horvaadi</td>
        <td>Hr.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tšehhi</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>      
    </tr>    
    <tr>
        <td>Taani</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>Hollandi</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>  
    </tr>    
    <tr>
        <td>Inglise</td>        
        <td>EN.Microsoft</td>
        <td>EN.lucene</td>      
    </tr>
    <tr>
        <td>Eesti</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Soome</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>Prantsuse</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>Galeegi</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>Saksa</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>Kreeka</td>
        <td>EL.Microsoft</td>
        <td>EL.lucene</td>      
    </tr>
    <tr>
        <td>Gudžarati</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Heebrea</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>      
    </tr>
    <tr>
        <td>Ungari</td>      
        <td>HU.Microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>Islandi</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indoneesia (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>Iiri</td>
        <td></td>
        <td>ga.lucene</td>
    </tr>
    <tr>
        <td>Itaalia</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>Jaapani</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Korea</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Läti</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>Leedu</td>
        <td>LT.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalami</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malai (ladina tähestik)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norra</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>Pärsia</td>
        <td></td>
        <td>FA.lucene</td>      
    </tr>
    <tr>
        <td>Poola</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>      
    </tr>
    <tr>
        <td>Portugali (Brasiilia)</td>
        <td>PT-Br.microsoft</td>
        <td>PT-Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugali (Portugal)</td>
        <td>PT-Pt.microsoft</td>        
        <td>PT-Pt.lucene</td>
    </tr>
    <tr>
        <td>Pandžabi</td>
        <td>PA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumeenia</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Vene</td>
        <td>ru.Microsoft</td>
        <td>ru.lucene</td>  
    </tr>
    <tr>
        <td>Serbia (kirillitsa)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbia (ladina tähestik)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovaki</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Sloveeni</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hispaania</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Rootsi</td>
        <td>Sv.Microsoft</td>
        <td>Sv.lucene</td>
    </tr>

    <tr>
        <td>Tamili</td>
        <td>ta.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Tai</td>
        <td>th.Microsoft</td>
        <td>th.lucene</td>
    </tr>
    <tr>
        <td>Türgi</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>      
    </tr>
    <tr>
        <td>Ukraina</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnami</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Lisaks Azure otsing pakub keele diagnostika analyzer konfiguratsioone</td>
    <tr>
        <td>Standardse ASCII viimistlemine</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode'i teksti osadeks (Standard Tokenizer)</li>
            <li>ASCII tagaistme filter - teisendab Unicode'i märki, mis ei kuulu komplekti esmalt 127 ASCII märkide sisse oma ASCII võrdlus. See on abiks eemaldamine Diakriitikute valimine.</li>
        </ul>
        </td>
    </tr>
</table>

Kõik analüsaatorid nimedega märgitud <i>lucene</i> on tootja [Apache Lucene keele analüsaatorid](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Lisateavet filtri volditud ASCII leiate [siit](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` määratleb, milliseid väljadel registri kasutatakse toetamiseks automaatteksti otsingud. Tavaliselt saadetakse osalist otsingut stringide [Soovituste API](#Suggestions) samal ajal, kui kasutaja tipib otsinguväljale päring ja API tagastab kogumi soovitatud fraasidest. Suggester, mille määrate registri määratleb, milliseid välju kasutatavad terminite ettetippimise otsing. Konfiguratsiooni üksikasjad leiate [Suggesters](#Suggesters) .

**Hinded profiilid**

A `scoringProfile` määratleb kohandatud hinded käitumist, mis võimaldavad teil mõjutada millised üksused kuvatakse otsingutulemite kõrgema. Hinded profiil koosneb välja kaalu ja funktsioonid. Neid kasutada, saate määrata profiili nime päringu string.

Vaikimisi profiili hinded töötab taustal arvutada otsingu Keskmine tulemite hulk iga üksuse jaoks. Saate kasutada ettevõttesisese, kasvavas hinded profiili. Teise võimalusena seadmine `defaultScoringProfile` kasutada kohandatud profiili vaikeprinteriks kasutada iga kord, kui kohandatud profiili pole määratud päringu string.

Üksikasjad leiate [Lisa hinded profiilid Otsinguindeksi (Azure'i otsingu teenuse REST API)](search-api-scoring-profiles-2015-02-28-preview.md) .

**CORS suvandid**

Kliendipoolne JavaScripti ei saa helistada ühtegi vaikimisi, kuna brauseri hoiab ära kõik rist-origin taotlused. Luba CORS (Cross-päritolu ressursside ühiskasutus), seades selle `corsOptions` atribuudi Luba rist-origin päringute oma registrisse. Pange tähele, et ainult päringu API-d toetama CORS turvalisuse põhjustel. CORS saab määrata järgmised suvandid:

- `allowedOrigins`(nõutav): see on päritolu, et anda juurdepääsu oma index loend. See tähendab, et nende päritolu kella JavaScripti koodi lubatakse päringu oma index (eeldades, et see pakub õige API võti). Iga origin on tavaliselt vormi `protocol://fully-qualified-domain-name:port` kuigi pordi sageli puudub. Leiate lisateavet [käesoleva artikli](http://go.microsoft.com/fwlink/?LinkId=330822) .
 - Kui soovite lubada juurdepääsu kõik päritolu, kaasa `*` nimega ühe üksuse, klõpsake selle `allowedOrigins` massiiv. Pange tähele, et **see pole soovitatav tava otsingu tootmisteenused.** Siiski võib olla kasulik või silumine eesmärgil.
- `maxAgeInSeconds`(valikuline): brauserid seda väärtust kasutama määratlemiseks kestus (sekundites) vahemälu CORS eelkontrolli vastuseid. See peab olema negatiivne täisarv. Mida suurem see väärtus on, saab parema jõudluse, kuid enam kulub CORS poliitika muudatuste jõustumiseks. Kui see pole määratud, kasutatakse vaikimisi kestab 5 minutit.

<a name="CreateUpdateIndexExample"></a>
**Koosolekukutse kehasse näide**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Vastus**

Eduka taotlus: "201 loodud".

Vaikimisi sisaldab vastuse keha JSON index määratluse, mis on loodud. Kui soovitud `Prefer` taotluse päis on seatud `return=minimal`, vastuse sisu on tühi ja edu olekukoodi saab "204 sisu" asemel "201 loodud". See kehtib sõltumata sellest, kas registri loomiseks kasutatud panna või POSTITUSSE.

**Märkused**

Praegu on piiratud tugi index skeemi värskendusi. Skeemi värskendusi, mis nõuavad näiteks muutmine Väljatüübid uuesti indekseerimine pole praegu toetatud. Kuigi olemasolevaid välju ei saa muuta ega kustutada, saate uusi välju lisada olemasolevat registrit igal ajal. Kui lisatakse uus väli, on kõik olemasolevad dokumendid registris olev automaatselt selle välja tühiväärtus. Täiendava salvestusruumi pole on tarbitud kuni uued dokumendid lisatakse indeks.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Azure'i otsingu funktsiooni soovitused on ettetippimise või automaatteksti päringu võimalus, võimalikud otsinguterminid osaline stringi sisendina otsing väljale on sisestatud vastuseks loendit. Olete märganud päringusoovituste äri web otsingumootorid kasutamisel: tingimused loendi tippimine Bingi ".NET" annab tulemiks ".NET 4.5", ".NET Framework 3.5", jne. Otsinguteenuse REST API kasutamisel rakendamise soovituste kohandatud Azure'i otsingurakenduse kasutamiseks on vaja järgmist:

- Luba soovitusi, lisades **suggester** ehituse oma registri nimi, otsingu režiimi ja väljad, mille ettetippimise kutsumisel loendit. Näiteks kui määratlete andmeallika välja, tippides osalist otsingut string "Sea" nimega "cityName" tulemuseks "Seattle", "Seaside" ja "Seatac" (kõik kolm on tegelik linna nimed) pakutud päringusoovituste kasutajale.

- Rakenduse koodis [Soovituste API](#Suggestions) helistades autonoomsest soovitusi. Tavaliselt saadetakse osalist otsingut stringide teenuse samal ajal, kui kasutaja tipib otsinguväljale päring ja selle API tagastab kogumi soovitatud fraasidest.

Selles artiklis selgitatakse, kuidas konfigureerida on **suggester**. Peaksite [Soovituste API](#Suggestions) on suggester kasutamise kohta.

**Kasutus**

`Suggesters`luuakse register ja töö parim kasutatuna soovitada teatud dokumendid, mitte lahti terminid või fraasid. Parima testversiooni väljad on pealkirjad, nimed ja muud suhteliselt fraase, mida saate tuvastada üksuse. Vähem tõhus on korduvate väljadel kategooriad ja sildid, või väga pikaks väljadel väljade kirjelduste või kommentaarid.

Registri määratlemine osana, saate lisada ühe suggester, et selle `suggesters` saidikogumi. Järgmised atribuudid, mis määratlevad mõne suggester.

- `name`: Selle suggester nimi. Kasutate selle suggester nime helistamisel on `suggest` API.
- `searchMode`: Strateegia testversiooni fraase otsida. Pole praegu toetatud ainult režiimi `analyzingInfixMatching`, mis teeb paindlik sobitamine fraasidest alguses või lause keskel.
- `sourceFields`: Üks või mitu väljad, mis on soovituste sisu allikas loendi. Ainult välja tüüp `Edm.String` ja `Collection(Edm.String)` võib soovituste allikad. Saab kasutada ainult väljad, mis pole kohandatud keele analüsaator, määrata.

**Suggester näide**

Registri lisamine suggester kuulub. Ainult üks suggester võivad esineda soovitud `suggesters` saidikogumi kõrval väljad saidikogumi uuemas versioonis ja `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Kui kasutasite Azure'i otsing avaliku eelvaateversiooni `suggesters` asendab vanema kahendmuutujaga vara (`"suggestions": false`) mis toetab ainult eesliite soovitusi lühike stringide (3 – 25 märki). Selle asendamine `suggesters`, toetab infiksi sobib leiab vastavaid termineid, alguses või keskel välja sisu, otsing stringide vigu parema hälbe. Alustades üldiselt kättesaadav väljaanne, see on nüüd ainult rakendamise soovituste API. Vanemate `suggestions` atribuut, mis võeti kasutusele `api-version=2014-07-31-Preview` see versioon töötab, kuid ei toimi, klõpsake selle `2015-02-28` või uuemad versioonid Azure otsing.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Registri värskendamine

Saate värskendada olemasolevat registrit Azure'i otsingu abil HTTP sellele taotluse sees. Värskendused võivad sisaldada uusi välju olemasoleva skeemi lisamine, muutmine CORS suvandid ja hinded profiilid muutmine. Lisateabe saamiseks vaadake [lisamine hinded profiilid](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Registri värskendamiseks taotluse URI nime määramine

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Oluline:** Registri skeemi värskendused tugi on piiratud toiminguid, mis ei nõua taastamine otsinguregistrisse. Skeemi värskendusi, et oleks vaja uuesti indekseerimise muutmise Väljatüübid, nt pole praegu toetatud. Kuigi olemasolevaid välju ei saa muuta ega kustutada saab igal ajal lisada uusi välju. See kehtib `suggesters`. Võib lisada uusi välju on suggester ajal väljad on lisatud, kuid ei saa eemaldada välju `suggesters` ja olemasolevaid välju ei saa lisada `suggesters`.

Uue välja lisamisel registri on kõik olemasolevad dokumendid registris olev automaatselt selle välja tühiväärtus. Täiendava salvestusruumi pole on tarbitud kuni uued dokumendid lisatakse indeks.

**Taotlemine**

HTTPS on vaja kõik hooldustaotlused. **Värskenda register** taotluse ehitatakse abil HTTP panna. Registri nime panna, on osa URL-i. Kui indeks pole olemas, on loodud. Kui indeks on juba olemas, värskendatakse uue määratluse.

Indeksi nimi peab olema väiketähtedes, käivitage tähe või numbri, on pole kaldkriipsud või punkti ja olema väiksem kui 128 märki. Pärast indeksi nimi algab tähe või numbri, ülejäänud nimi võib sisaldada mis tahes tähe, arv ja kriipsud, kui kriipsud mittejärjestikuste.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `Content-Type`: Nõutav. See määramine`application/json`
- `api-key`: Nõutav. Funktsiooni `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Värskenda register** taotluse peab sisaldama mõne `api-key` päise määramine teie administraator võti (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse süntaks**

Olemasoleva registri värskendamisel keha peab sisaldama algse skeemi määratluse, pluss uusi välju, mida soovite lisada, samuti muudetud hinded profiilid, suggesters ja CORS suvandid, kui mõni. Kui ei muudate hinded profiilid ja CORS suvandid, peate lisama originaali registri loomise kaudu. Üldiselt parima mustri kasutamiseks värskendused on tuua registri määratlemine koos saamiseks, seda muuta, siis värskendage seda ja pange.

Registri loomiseks kasutatud skeemi süntaks on esitatud siin mugavuse. Üksikasjalikumat teavet teemast [Registri loomine](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Vastus**

Eduka taotlus: "204 sisu".

Vaikimisi on tühi vastuse keha. Juhul, kui selle `Prefer` taotluse päis on seatud `return=representation`, vastuse keha sisaldab värskendatud index määratlemiseks JSON. Sel juhul tuleb edu olekukoodi "200 OK".

**Registri määratlemine koos kohandatud analüsaatorid värskendamine**

Kui mõni analüsaator, on tokenizer, Turbeloa filtri või filtri char on määratletud, ei saa muuta. Uusi faile saab lisada olemasolevat registrit ainult juhul, kui selle `allowIndexDowntime` lipu väärtuseks true registri värskendamine taotluse: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Pange tähele, et see toiming paneb index ühenduseta vähemalt mõne sekundi, põhjustab teie indekseerimine ja päringu kutsed nurjuvad. Registri jõudlus ja kirjutage kättesaadavus võib olla mõne minuti pärast registri värskendatakse kahjustatud või enam väga suurte registrid.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Registrite loend

**Loendi registrid** toiming tagastab loendi indeksid praegu oma Azure'i otsinguteenuse.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Taotlemine**

HTTPS on vaja kõik hooldustaotlused. **Loendi registrid** taotluse saate ehitatakse GET meetodi abil.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Nõutav. Funktsiooni `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Loendi registrid** taotluse peab sisaldama mõne `api-key` seada väärtuseks on administraator (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Mitte keegi.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

Siin on näide vastuse keha.

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Pange tähele, et vastus allapoole just teile huvi atribuudid saate filtreerida. Näiteks, kui soovite ainult index nimede loend, kasutage OData `$select` päringu suvand:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

Sel juhul vastuse eeltoodud näites oleks kuvatakse järgmiselt:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

See on kasulik tehnika läbilaskevõime salvestada, kui teil on palju registrid oma otsinguteenuse.

<a name="GetIndex"></a>
## <a name="get-index"></a>Saada Index

**Saada Index** toimingu saab registri määratlemine Azure'i otsing.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Taotlemine**

HTTPS on vaja hooldustaotlused. **Saada Index** taotluse saate ehitatakse GET meetodi abil.

[Registri nimi] taotluse URI saate määrata, milline indeks registrid kollektsioonist tagastamiseks.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Registri saada** taotlus peab sisaldama mõne `api-key` seada väärtuseks on administraator (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Mitte keegi.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

Vt näide JSON [loomine ja värskendamine registri](#CreateUpdateIndexExample) jaoks vastuse last näide.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Registri kustutamine

**Registri kustutamine** toimingut eemaldab teie Azure'i otsinguteenuse registri ja seotud dokumente. Registri nime saate teenuse armatuurlaua Azure portaali kaudu või API. Üksikasjad leiate [Loendi registrid](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Taotlemine**

HTTPS on vaja hooldustaotlused. **Registri kustutamine** taotluse saate ehitatakse Kustuta meetodi abil.

[Registri nimi] taotluse URI saate määrata, milline indeks registrid kollektsioonist kustutamine.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Nõutav. Funktsiooni `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenuse URL-i stringiväärtus. **Registri kustutamine** taotluse peab sisaldama mõne `api-key` päise määramine teie administraator võti (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Mitte keegi.

**Vastus**

Olekukoodi: 204 sisu puudub, tagastatakse eduka vastuse ootamine.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Saada register statistika

**Saada register statistika** toiming tagastab Azure'i otsingust dokumentide arvu praeguse index pluss salvestusruumi kasutuse.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Count ja salvestusruumi dokumendimaht statistika kogutakse iga mõni minut, mitte reaalajas. Seega selle API tagastatud statistika võib kajastavad muudatused põhjustatud indekseerimise tehtud toimingud.

**Taotlemine**

HTTPS on vaja kõiki teenuseid kutsed. **Saada register statistika** taotluse saate ehitatakse GET meetodi abil.

[Registri nimi] taotluse URI ütleb teenuse register statistika jaoks määratud indeks tagastamiseks.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.


**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Registri statistika saada** taotlus peab sisaldama mõne `api-key` seada väärtuseks on administraator (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Mitte keegi.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

Vastuse sisu on järgmises vormingus:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Testige analüsaator

**Analüüsi API** näitab, kuidas mõni analüsaator jaotab teksti märkide.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Taotlemine**

HTTPS on vaja kõiki teenuseid kutsed. POSTITUSE meetodi abil saate ehitatakse **Analüüsida API** taotluse.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.


**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Analüüsi API** taotluse peab sisaldama mõne `api-key` seada väärtuseks on administraator (erinevalt päringu klahvi).

Te peate registri nimi ja teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

või

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

Funktsiooni `analyzer_name`, `tokenizer_name`, `token_filter_name` ja `char_filter_name` peavad olema sobivad nimed eelmääratletud või kohandatud analüsaatorid, tokenizers, Turbeloa filtrite ja registri char filtrid. Lisateavet protsessi leksikaalse analüüsi leiate [Azure'i otsingu analüüsi](https://aka.ms/azsanalysis).

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

Vastuse sisu on järgmises vormingus:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Analüüsi API näide**

**Taotlemine**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Vastus**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Dokumendi tegevus

Azure'i otsinguväljale registri talletatakse pilves ja täidetud JSON dokumentide üleslaadimiseks teenuse abil. Kõik dokumendid, mida saate üles laadida hõlmavad andmete otsing kogumina. Sisaldavad väljad, mis on tokenized üheks otsinguterminid nagu nad on üles laaditud dokumendid. Funktsiooni `/docs` URL-i lõigu Azure'i otsingu API tähistab registri dokumentide kogum. Kõigi toimingutest saidikogumi näiteks üleslaadimine, ühendamise, kustutamine või päringute dokumentide tee paigutada ühe indeks, kontekstis nii URL-ide jaoks neid toiminguid alati alustada `/indexes/[index name]/docs` antud registri nimi.

Rakenduse koodis tuleb luua kas JSON dokumentide üleslaadimine Azure'i otsing või abil saate ka [indekseerija](https://msdn.microsoft.com/library/dn946891.aspx) asetage dokumendid, kui andmeallika on Azure'i SQL-andmebaasi või DocumentDB. Tavaliselt täidetakse teie ühe andmekogumi registrid.

Mida peaks plaani, millel ühe dokumendi iga üksuse, mida soovite otsida. Filmi laenutus rakendus võib-olla ühte dokumenti kohta filmi, poe rakendus võib-olla ühte dokumenti kohta SKU-ga, online õppematerjali rakendus võib-olla ühte dokumenti kursuse kohta, uuringute firma võib on ühe dokumendi iga akadeemiline paberi oma hoidla jne.

Dokumendid, olla üks või mitu välja. Väljade võib sisaldada teksti, mis on Azure otsing sisse otsinguterminid tokenized, samuti mitte tokenized või mittetekstilisi väärtusi, mida saab kasutada filtrit või hinded profiilid. Nimed, andmetüübid ja iga välja jaoks toetatud otsinguvõimalused määratakse index skeemi järgi. Ühe välja iga index skeemi olema määratud ID ja iga dokumendi ID välja, mis tuvastab kordumatult selle dokumendi registri väärtus peab olema. Kõik dokumendi väljad pole kohustuslikud ja vaikimisi tühiväärtus kui vasakule määramata. Pange tähele, et tühiväärtusi võta ruumi otsinguregistrisse.

Enne kui saate dokumente üles laadida, peab teil juba loodud indeks teenuse. [Registri loomine](#CreateIndex) kohta vaadake teavet see kõigepealt.

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Lisamine, värskendamine või dokumentide kustutamine

Saate üles laadida, määratud indeks, kasutades HTTP POST ühendada, kooste või laadi või Kustuta dokumente. Suure hulga värskendused, on soovitatav partiide dokumentide kuni (partii 1000 dokumendid) või umbes 16 MB paketi kohta.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Taotlemine**

HTTPS on vaja kõik hooldustaotlused. Saate üles laadida, määratud indeks, kasutades HTTP POST ühendada, kooste või laadi või Kustuta dokumente.

Taotluse URI sisaldab [registri nimi], määrata, milline indeks dokumentide postitamiseks. Saate sisestada ainult üks indeks dokumentide korraga.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `Content-Type`: Nõutav. See määramine`application/json`
- `api-key`: Nõutav. Funktsiooni `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Dokumentide lisamine** taotluse peab sisaldama mõne `api-key` päise määramine teie administraator võti (erinevalt päringu klahvi).

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Taotluse sisu sisaldab ühe või mitme dokumendi indekseerida. Dokumentide tuvastatakse kordumatu võti. Iga dokumendi on seostatud toimingu: üles, kooste, mergeOrUpload või Kustuta. Laadi taotlused peavad sisaldama dokumendi andmete /-väärtuse paarideks kogumina.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Dokumendi klahvid võib sisaldada ainult tähti, numbreid, kriipsjooned ("-"), allakriipsutatud märkideks ("_") ja võrdsete märgid ("="). Lisateavet leiate teemast [nimetamise reeglid](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Dokumendi toimingud**

- `upload`: Toimingu üleslaadimine on sarnane "upsert" dokumenti lisatud kus, kui see on uus ja värskendatud/asendada, kui see on olemas. Pange tähele, et update juhul asendatakse kõik väljad.
- `merge`: Kirjakooste värskendab olemasoleva dokumendi määratud väljad. Kui dokument pole olemas, kooste nurjub. Saate määrata kooste välja asendab olemasoleva välja dokumendis. See hõlmab väljad, mille tüüp on `Collection(Edm.String)`. Oletagem näiteks, et dokument sisaldab välja "sildid" väärtusega `["budget"]` ja te kooste väärtusega `["economy", "pool"]` "sildid", "sildid" väljale lõppväärtus saab `["economy", "pool"]`. See **ei** saa `["budget", "economy", "pool"]`.
- `mergeOrUpload`: käitub nagu `merge` antud võti dokumendi registris juba olemas. Kui dokument pole see toimib nagu `upload` uue dokumendiga.
- `delete`: Kustuta eemaldab määratud dokumenti registri. Pange tähele, et kõik väljad saate määrata mõne `delete` kui võtme välja ignoreeritakse. Kui soovite eemaldada dokumendist üksiku välja, kasutage `merge` hoopis ja lihtsalt seada välja konkreetselt juurde `null`.

**Vastus**

Olekukoodi 200 (OK), tagastatakse eduka vastust, mis tähendab, et kõik üksused indekseeritud. Seda näitab funktsiooni `status` atribuut on seatud tõene kõigi üksuste nimega soovitud `statusCode` atribuudiga 201 (üleslaaditud dokumentide) jaoks või 200 (ühendatud või kustutatud dokumendid):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Kui vähemalt üks üksus on edukalt indekseeritud, tagastatakse olekukoodi 207 (mitme olek). Üksused, mis pole indekseeritud on selle `status` väärtuseks false. Funktsiooni `errorMessage` ja `statusCode` atribuudid näitab indekseerimise tõrke põhjuse:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Järgmises tabelis selgitatakse erinevate kohta dokumendi olek koodide vastuse tagastanud. Pange tähele, et mõned probleemid taotlus ise, näidates ajal teistele näidata ajutine viga tingimused. Viimane peaks uuesti, viivitusega.

<table style="font-size:12">
    <tr>
        <th>Olekukoodi</th>
        <th>Tähendus</th>
        <th>Retryable</th>
        <th>Märkmete</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument on edukalt muudetud või kustutatud.</td>
        <td>n/a</td>
        <td>Kustuta toimingud, <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Seega isegi juhul, kui dokumendi klahvi pole registris, proovite Kustutustoimingu selle klahvi tulemuseks 200 olekukoodi.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument on loodud.</td>
        <td>n/a</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Dokument, mille tõttu ei indekseeritud ilmnes tõrge.</td>
        <td>Ei</td>
        <td>Mis on valesti dokumendi näitab vastus kuvatakse tõrketeade.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Dokumendi ei saa ühendada registri pole antud võti.</td>
        <td>Ei</td>
        <td>See tõrge ei esine üleslaadimiseks, kuna need uusi dokumente luua ja see ei toimu kustutab jaoks, kuna need on <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Versiooni konflikt tuvastati indekseerida dokumendi katsel.</td>
        <td>Jah</td>
        <td>See võib juhtuda siis, kui proovite indekseerimine rohkem kui üks kord samaaegselt samas dokumendis.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Registri pole ajutiselt saadaval, kuna seda värskendati 'allowIndexDowntime' lipp on seatud "true".</td>
        <td>Jah</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Oma otsingu teenus pole ajutiselt saadaval, võib-olla suur koormus.</td>
        <td>Jah</td>
        <td>Teie kood tuleks oodake proovimiseks sel juhul või riski te pikendada teenuse andnud.</td>
    </tr>
</table> 

**Märkus**: kui sageli tekib oma kliendi kood 207 vastust, on võimalik, arvutisüsteemi koormuse. Saate kinnitada, märkides ruudu soovitud `statusCode` atribuudi 503. Kui see on nii, soovitame ***pidurdamise indekseerimise taotlused***. Muul juhul, kui liikluse indekseerimine ei kaovad, süsteemi võiks alustada lükake need tagasi kõik kutsed 503 vigadega.

Olekukoodi 429 näitab, et olete ületanud meiliteavitus kohta indeks dokumentide arvu. Peate Loo uus indeks või suurema võimsuse piiranguid uuendada.

**Näide:**

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
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Dokumentide otsimine

**Otsingu** toiming on välja toomine või POSTITUSSE päringuga ja määrab parameetrid, mis annavad kriteeriumidele vastavaid dokumente valimise.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Millal kasutada postituse asemel hankimine**

Kui kasutate helistamiseks **Otsingu** API HTTP GET, peate võtke arvesse, et taotluse URL-i pikkus ei tohi ületada 8 KB. See on tavaliselt piisavalt enamiku rakenduste puhul. Siiski teatud rakenduste tootma väga suurte päringute või OData filtriavaldiste. Nende rakenduste abil HTTP POST on parem valik kuna see lubab suuremat filtrid ja päringud kui GET. POSTITUSE, on termineid või päringu klauslitest arvu piiramine tegur, ei mahu töötlemata päringu taotluse postituse mahupiirang on umbes 16 MB.

> [AZURE.NOTE] POSTITUSE taotluse mahupiirangu on väga suurte, otsingupäringuid ja filtriavaldiste ei saa olla meelevaldselt keerukas. Lisateavet [Lucene päringusüntaksit](https://msdn.microsoft.com/library/mt589323.aspx) ja [avaldisesüntaksi OData](https://msdn.microsoft.com/library/dn798921.aspx) otsingu päringu ja filtreerida keerukuse piirangute kohta.

**Taotlemine**

HTTPS on vaja hooldustaotlused. **Otsingu** taotluse saate ehitatakse meetoditega toomine või POSTITUSSE.

Taotluse URI saate määrata, milline indeks päringusse kõigi dokumentide, mis vastavad parameetrid. Parameetrite on ära toodud päringustringi puhul Saada kutsed ja taotluse taotleb keha postituse puhul.

Hea tava Saada kutsed loomisel, ärge unustage [URL-i kodeerida](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) konkreetse päringu parameetrite otse REST API helistades. **Otsingu** toimingute, see hõlmab:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-i kodeerimine on soovitatav ainult päringu ülaltoodud parameetritel. Kui te tahtmatult URL-i kodeerida kogu päringustringi (kõik pärast selle?), kutsed kuvatakse leheküljepiiri.

Lisaks URL-i kodeeringus on vajalik ainult helistades otse kasutades GET REST API-ga. URL-i kodeerimine on vaja helistamisel **Otsingu** abil postituse või kasutades [.net-i kliendi teek](https://msdn.microsoft.com/library/dn951165.aspx), mis tegeleb URL-i kodeerimine teie eest.

<a name="SearchQueryParameters"></a>
**Päringu parameetrite**

**Otsingu** aktsepteerib Päringukriteeriumide anda ja määrata ka otsing käitumine mitmed parameetrid. Esitate järgmiste parameetrite URL-i päringu stringi helistamisel **Otsingu** kaudu hankimine ja JSON atribuutidena koosolekukutse kehasse helistamisel **Otsingu** postiga. Mõned parameetrid süntaks on pisut erinev hankimine ja POSTITA. Erinevused andmeteks rakendatav all:

`search=[string]`(valikuline) - teksti otsida. Kõik `searchable` väljad on vaikimisi otsingut, v.a juhul, kui `searchFields` on määratud. Kui otsite `searchable` väljad Otsitav tekst ise tokenized nii mitme terminite saab eraldada tühimiku (nt: `search=hello world`). Mis tahes Termini vastavaks kasutada `*` (see võib olla kasulik kahendmuutujaga filter päringuid). Faktitabeli see parameeter on sama mõju, seades `*`. [Lihtsa päringusüntaksit](https://msdn.microsoft.com/library/dn798920.aspx) leiate otsingu süntaksi üksikasjad.

  - **Märkus**: tulemusi võib mõnikord olla üllatav, kui kursor päringu `searchable` väljad. Funktsiooni tokenizer sisaldab loogika käsitlema juhul levinud inglise keele teksti nagu ülakomad, komad arvude jne. Näiteks `search=123,456` vastavad ühe Termini 123456, mitte üksikute terminite 123 ja 456, kuna komad kasutatakse tuhat eraldavate palju inglise keeles. Seetõttu soovitame kasutada tühja ruumi, mitte kirjavahemärgid eraldi terminite soovitud `search` parameeter.

`searchMode=any|all`(valikuline, on vaikimisi `any`) – kas mõnda otsinguterminid olema täidetud arvestamiseks dokumendi vaste.

`searchFields=[string]`(valikuline) - määratud teksti välja komaga eraldatud nimede loendi. Sihtrakenduse väljad peab olema tähistatud `searchable`.

`queryType=simple|full`(valikuline, on vaikimisi `simple`) – kui "lihtne" Otsitav tekst on seatud tõlgendatakse lihtpäringu keelt, mis võimaldab sümbolid näiteks +, * ja "". Päringute hinnatakse üle kõik otsitavad väljad (või väljad on toodud `searchFields`) igas dokumendis vaikimisi. Kui päringu tüüp on seatud `full` tõlgendatakse Otsitav tekst, mis võimaldab välja kohased ja kaalutud otsingud Lucene päringukeelt kasutavate. Klõpsake otsingu lauseõpetuse üksikasjad leiate [Lihtsa päringusüntaksit](https://msdn.microsoft.com/library/dn798920.aspx) ja [Lucene päringusüntaksit](https://msdn.microsoft.com/library/mt589323.aspx) . 
 
> [AZURE.NOTE] Vahemiku Lucene, ei toetata päringukeele kasuks, mis pakub sarnast funktsionaalsust $filter otsing.

`moreLikeThis=[key]`(valikuline) **Oluline:** See funktsioon on saadaval ainult `2015-02-28-Preview`. See suvand ei saa kasutada päringus, mis sisaldab parameetrit teksti otsing `search=[string]`. Funktsiooni `moreLikeThis` parameetri leiab dokumente, mis on määratud dokumendi võti dokumendi sarnaselt. Kui Otsingutaotluse on tehtud `moreLikeThis`, loendi otsinguterminid on loodud, sageduse ja haruldus lähtedokumendis terminite põhjal. Seejärel kasutatakse neid termineid taotluse. Vaikimisi kogu sisu `searchable` väljade käsitletakse kui `searchFields` kasutatakse piirata, millised väljad otsitakse.  

`$skip=#`(valikuline) – arvu otsingutulemite vahele; Ei tohi olla suurem kui 100 000. Kui peate kasutama dokumentide skannimine järjekorras, kuid ei saa kasutada `$skip` tõttu seda piirangut, kaaluge `$orderby` täiesti tellitud võti ja `$filter` vahemiku päringu selle asemel.

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `skip` asemel `$skip`.

`$top=#`(valikuline) – otsingutulemite toomiseks arv. Seda saab kasutada koos `$skip` kliendipoolne Piipar otsingutulemite rakendada.

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `top` asemel `$top`.

`$count=true|false`(valikuline, on vaikimisi `false`) – saate määrata, kas toomiseks tulemite koguarv. See on kõik dokumendid, mis vastavad arv soovitud `search` ja `$filter` parameetrid, ignoreerides `$top` ja `$skip`. Selle väärtuse seadmine `true` võivad mõjutada jõudlust. Pange tähele, et arvu tagastatud aitab.

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `count` asemel `$count`.

`$orderby=[string]`(valikuline) - komaga eraldatud avaldiste tulemite järgi sortimiseks loendit. Iga avaldise võib olla välja nime või kõne on `geo.distance()` funktsioon. Iga avaldise saate järgneb `asc` soovite tõusvas järjestuses, nagu on näidatud ja `desc` tähistamiseks laskuvas järjestuses. Vaikimisi on tõusvas järjestuses. TIES ei tööta, match hinded dokumentide. Kui pole `$orderby` pole määratud, vaikesortimisjärjestus on kahanevalt dokumendi match Keskmine. Piiratud 32-klauslitest jaoks `$orderby`.

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `orderby` asemel `$orderby`.

`$select=[string]`(valikuline) – toomiseks komaga eraldatud väljade loend. Kui määramata, kaasatakse kõik väljad, mis on märgitud nii tagastatava skeemi. Saate ka konkreetselt taotleda kõik väljad, parameetri `*`.

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `select` asemel `$select`.

`facet=[string]`(null või rohkem) – välja tahukas järgi. Soovi korral stringi võib sisaldada parameetreid faceting, komaga eraldatud väljendatud `name:value` paari. Lubatud parameetrid on:

- `count`(max arv tahukas terminite; vaikeväärtus on 10). On ülempiir puudub, kuid suurte väärtuste tekkida vastavate jõudluse karistust, eriti siis, kui lihvitud väljal suure hulga kordumatu tingimused.
  - Näide: `facet=category,count:5` saab viiel kohal kategooriad tahukas tulemused.  
  - **Märkus**: kui soovitud `count` parameeter on väiksem kui kordumatu terminite arvu, ei pruugi tulemused täpne. Selle põhjuseks faceting päringud on jaotatud üle shards viisi. Suurendab `count` , suureneb täpsuse Termini loeb, kuid jõudlus eest.
- `sort`(üks `count` arvu, järgi *laskuvas järjestuses* sortimiseks `-count` sortimiseks *tõusvas* arvu järgi, `value` sortimiseks *tõusvas* väärtuse alusel, või `-value` väärtuse järgi *laskuvas järjestuses* sortimiseks)
  - Näide: `facet=category,count:3,sort:count` top kolm kategooriaid saab tahukas tulemuste arvu dokumendid, millel on iga linna nime järgi laskuvas järjestuses. Näiteks kui ülemise kolmele on eelarve, Motel ja luksus, eelarve on 5 tabamust, Motel on 6 ja Luxury on 4, siis ämbrid on järjestuses Motel, eelarve, Luxury.
  - Näide: `facet=rating,sort:-value` toodab ämbrid kõigi võimalike hinnangute, väärtuse järgi laskuvas järjestuses. Näiteks kui selle hinnangute on 1 kuni 5, ämbrid tellitud 5, 4, 3, 2, 1, olenemata sellest, kui palju dokumente vastavad iga reiting.
- `values`(Triibu eraldatud arv või `Edm.DateTimeOffset` täpsustades dünaamiline tahukas kirje väärtuste kogumi väärtused)
  - Näide: `facet=baseRate,values:10|20` toodab kolme ämbrid: üks base määr 0 kuni, kuid välja arvatud kuni 10, üks 10, välja arvatud 20 ja üks 20 või uuem versioon.
  - Näiteks: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` toodab kaks ämbrid: üks enne veebruar 2010 renoveeritud läheduses, ja teine läheduses renoveeritud veebruar 1st, 2010 või uuemat versiooni.
- `interval`(täisarv intervall, mis on suurem kui 0, numbrite, või `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` jaoks kuupäev ja kellaaeg)
  - Näide: `facet=baseRate,interval:100` toodab ämbrid määr lahtrivahemike suurus 100 põhjal. Näiteks kui on kõik $60 ja 600 vahel, tekib ämbrid 0 – 100, 100-200, 200-300, 300-400, 400-500 ja 500-600.
  - Näide: `facet=lastRenovationDate,interval:year` annab tulemiks ühe kopp iga-aastane kui hotellist renoveeriti.
- `timeoffset`([+-]: hh: mm, [+-] TTMM, või [+-] [hh) `timeoffset` pole kohustuslik. See saab ainult ühendada funktsiooni `interval` suvand ja ainult siis, kui välja, mille tüüp on rakendatud `Edm.DateTimeOffset`. Väärtuse saate määrata UTC aega offset konto säte aeg piirid.
  - Näide: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` kasutab päeva piiri, mis käivitab 01:00:00 UTC (keskööst target ajavööndi)
- **Märkus**: `count` ja `sort` võib kombineerida sama tahukas määratlus, kuid nad ei saa kombineerida `interval` või `values`, ja `interval` ja `values` ei saa omavahel ühendatud.
- **Märkus**: intervall omadustega kuupäeva ja kellaaja alusel UTC aega, kui arvutatakse `timeoffset` pole määratud. Näide: jaoks `facet=lastRenovationDate,interval:day`, piiri algab 00:00:00 UTC päeva. 

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `facets` asemel `facet`. Samuti peate määrama selle JSON massiivi stringid, kus iga stringi on eraldi tahukas avaldis.

`$filter=[string]`(valikuline) - liigendatud otsinguavaldise standard OData süntaksit. Leiate Azure'i otsingu toetab OData avaldis grammatika alamhulga üksikasjad [OData avaldise süntaksisse](#ODataExpressionSyntax) .

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `filter` asemel `$filter`.

`highlight=[string]`(valikuline) – tulemus kasutatakse komaeraldusega väljanimesid kogumi tõstab esile. Ainult `searchable` välju saab kasutada tabas esiletõstmine.

`highlightPreTag=[string]`(valikuline, on vaikimisi `<em>`) - A silt, mis on olulised tabas prepends string. Peab olema seatud koos `highlightPostTag`.

> [AZURE.NOTE] Kui **Otsingu** abil toomine, URL-i reserveeritud märkide helistamiseks tuleb protsendi-kodeerida (nt asemel # % 23).

`highlightPostTag=[string]`(valikuline, on vaikimisi `</em>`) – stringi silt, mis lisab tabas olulised. Peab olema seatud koos `highlightPreTag`.

> [AZURE.NOTE] Kui **Otsingu** abil toomine, URL-i reserveeritud märkide helistamiseks tuleb protsendi-kodeerida (nt asemel # % 23).

`scoringProfile=[string]`(valikuline) – hinnata hinded profiili nimi vastavad hinded ühtivate selleks, et tulemite sortimise dokumentide jaoks.

`scoringParameter=[string]`(null või rohkem) – näitab, et iga parameetri hinded funktsioonis määratletud väärtused (nt `referencePointParameter`) vormingus `name-value1,value2,...`.

- Näiteks kui hinded profiili määratleb funktsiooni nimega "mylocation" parameetriga päringu stringi suvand oleks `&scoringParameter=mylocation--122.2,44.8`. Esimese Kriipsjoone eraldab nime väärtusteloendi, kuigi teine Kriipsjoone on osa esimese väärtuse (selles näites pikkuskraad).
- Hinded parameetrite nagu sildi saate suurendada, mis sisaldab komasid, ei pääse neid väärtusi, kasutades ülakomadega loendis. Kui väärtused ise sisaldavad ülakomadega, te ei pääse neile poole võrra.
  - Näiteks, kui teil on suurendamine nimega "mytag" parameeter sildi ja soovite suurendada sildi väärtust "Tere, O'Brien" ja "Kask", päringu stringi võimalus oleks `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Hinnapakkumised on ainult väärtused, mis sisaldavad komad eelduseks.

> [AZURE.NOTE] **Otsingu** abil postituse helistamisel see parameeter on nimega `scoringParameters` asemel `scoringParameter`. Samuti peate määrama JSON massiivi stringid, kus iga stringi on eraldi `name-values` sidumiseks.

`minimumCoverage`(valikuline, vaikeväärtus on 100) – arv vahemikus 0 – 100, tuleb selleks, et päring olema staatusega õnnestumise otsingupäringu katta registrit protsentides. Vaikimisi kogu registri peab olema saadaval või `Search` tagastab HTTP olekukoodi 503. Kui seate `minimumCoverage` ja `Search` õnnestub, see tagastada HTTP 200 ja kaasata on `@search.coverage` indeks, mis lisati päringu protsentides vastus väärtus.

> [AZURE.NOTE] Parameetri väärtus väiksem kui 100 võib olla kasu otsingu kättesaadavus isegi ainult üks koopia teenuste tagamiseks. Siiski kõiki vastavaid dokumente on tagatud Esita otsingutulemites. Kui otsingu tagasivõtmist olulisem rakenduse kättesaadavus, siis on parem jätta `minimumCoverage` vaikimisi väärtuses 100.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

Märkus: selle toimingu jaoks soovitud `api-version` on määratud Päringuparameetri URL-i sõltumata sellest, kas helistate **Otsingu** toomine või POSTITUSSE.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenuse URL-i stringiväärtus. Kutse **Otsing** saate määrata administraator klahv või päringu võti `api-key`.

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Saada: pole.

Post:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Osalise otsingu vastuste jätkamine**

Mõnikord Azure otsing ei saa tagastada kõik nõutud tulemused ühe otsingu vastuse. See võib juhtuda siis põhjustel, näiteks kui päring nõuab liiga palju dokumente ei määramisega `$top` või väärtus `$top` mis on liiga suur. Sellisel juhul hõlmab Azure'i otsing on `@odata.nextLink` marginaali vastuse kehas, kui ka `@search.nextPageParameters` kui see on POST taotlus. Saate koostada järgmise osa otsingu vastuse saamiseks teise Otsingutaotluse väärtused nende marginaalid. Seda nimetatakse ***jätku*** algse Otsingutaotluse ja marginaalid üldiselt nimetatakse ***jätku sõned***. Vaadake [alltoodud näites](#SearchResponse) üksikasjad nende marginaalid süntaks ja kus need kuvatakse vastuse kehas. 

Põhjused, miks Azure'i otsing võib tagastada jätku sõned on juurutuskohase ja võib muutuda. Robustne kliendid tuleb alati valmis, mida teha juhul, kui oodatust vähem dokumendid tagastatakse jätkamiseks luba on kaasatud jätkamiseks toomine dokumendid. Samuti võtke arvesse, et peate kasutama sama meetod HTTP esialgse taotlusega jätkamiseks. Näiteks kui olete saatnud GET-päringu, mis tahes jätku taotlusi saadate kasutama ka GET (ja ka POSTITA).

<a name="SearchResponse"></a>
**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Näited:**

Täiendavad näited leiate [Azure'i otsingu jaoks OData Avaldisesüntaksi](https://msdn.microsoft.com/library/azure/dn798921.aspx) lehel.

1)  Kuupäeva järgi laskuvas järjestuses sorditud registrist otsimiseks.


    HANGI /indexes/hotels/docs? otsingu = * & $orderby = lastRenovationDate desc & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "*", "orderby": "lastRenovationDate laskuv"}

2)  Lihvitud otsingus registrist otsimiseks ja tuua kategooriad, reiting, sildid, samuti üksused, mille puhul on teatud baseRate aspekte.


    GET /indexes/hotels/docs? otsingu = testi & tahukas = kategooria & tahukas = reiting & tahukas = sildid ja tahukas = baseRate väärtused: 80 | 150 | 220 & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "test", "omadustega": ["kategooria", "reiting", "sildid", "baseRate väärtused: 80 | 150 | 220"]}

3)  Filtri, abil otsingutulemusi eelmise lihvitud päringu pärast seda, kui kasutaja klõpsab hindamine 3 ja kategooria "Motel":


    GET /indexes/hotels/docs? otsingu = testi & tahukas = sildid ja tahukas = baseRate väärtused: 80 | 150 | 220 & $filter = reiting k 3 ja kategooria eq 'Motel"& api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "test", "omadustega": ["sildid", "baseRate väärtused: 80 | 150 | 220"], "filter": "reiting eq 3 ja kategooria eq"Motel""}

4) Lihvitud otsingus seada ülempiir päringu tagastatud kordumatu tingimused. Vaikimisi on 10, kuid te saate suurendada või vähendada, väärtuse abil soovitud `count` parameeter on `facet` atribuut:


    HANGI /indexes/hotels/docs? otsingu = testi & tahukas = city, count: 5 ja api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "test", "omadustega": ["linn, count: 5"]}

5)  Otsige sees kindlad väljad; Näiteks Keelekohased väli:


    HANGI /indexes/hotels/docs? otsingu = hôtel & searchFields = description_fr & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "hôtel", "searchFields": "description_fr"}

6) Registri otsida mitmest väljast. Näiteks saate talletada ja päringu otsitavate väljade piires kõik sama registri mitmes keeles.  Kui inglise ja Prantsuse kirjeldused eksisteerivad samas dokumendis, võib tagastada või kõigi päringutulemites:


    HANGI /indexes/hotels/docs? otsingu = teha & searchFields = kirjeldus, description_fr & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "teha", "searchFields": "kirjeldus, description_fr"}

Pange tähele, et saab ainult päringud üks indeks korraga. Luua mitme registrid iga keele jaoks, kui te ei plaani päringu ükshaaval.

7)  Piipar - Get üksuste 1st lehe (lehe suurus on 10):


    HANGI /indexes/hotels/docs? Otsi = * & $skip = 0 & $top = 10 & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "*", "vahele jätta": 0 "top": 10}

8)  Piipar - Get 2 lehe üksuste (lehe suurus on 10):


    HANGI /indexes/hotels/docs? otsingu = * & $skip = 10 & $top = 10 & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "*", "vahele jätta": "top" 10: 10}

9)  Saate tuua määratud väljad:


    HANGI /indexes/hotels/docs? otsingu = * & $select = hotelName, kirjeldus ja api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "*", "valige": "hotelName kirjeldus"}

10)  Saate tuua kindla filtriavaldise vastavaid dokumente:


    HANGI /indexes/hotels/docs? $filter = (baseRate ge 60 ja baseRate lt 300) või hotelName k ilustatud püsida kui & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versiooni 2015 02 28 eelvaates = {"filter": "(baseRate ge 60 ja baseRate lt 300) hotelName eq"Ilustatud jää"või"}

11) Funktsioonide index ja tagasi fragmendid koos tabas olulised otsimine


    HANGI /indexes/hotels/docs? otsing midagi = & esile tõsta = kirjeldus & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "midagi", "esiletõstmine": "kirjeldus"}

12) Registrist otsimiseks ja tagastab sorditud lähemale kaugemal eemale viite asukohast dokumendid


    HANGI /indexes/hotels/docs? otsing = midagi & $orderby=geo.distance (asukoht, geography'POINT(-122.12315 47.88121) ") & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "midagi", "orderby": "geo.distance (asukoht, geography'POINT(-122.12315 47.88121)") "}

13) Eeldusel, et on nimega "geo" hinded profiili registrist otsimiseks kaks kauguse hinded funktsioonidega, üks määratlemine parameeter nimega "currentLocation" ja üks parameeter nimega "lastLocation" määratlemine


    GET /indexes/hotels/docs? otsingu = midagi & scoringProfile = geo & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 ja api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "midagi", "scoringProfile": "geo", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}

14) Dokumentide otsimine registris olev [lihtsa päringu](https://msdn.microsoft.com/library/dn798920.aspx)süntaksit. See päring tagastab hotellist, kus otsitavad väljad sisaldavad soovitud terminite "mugavalt" ja "asukoht", kuid mitte "motel":


    GET /indexes/hotels/docs? otsing = mugavalt + asukoht – motel & searchMode = all & api-versioon = 2015 02 28 eelvaates

    POSTITUSE /indexes/hotels/docs/search? api-versioon 2015 02 28 eelvaates = {"otsing": "mugavalt + asukoht-motel", "searchMode": "kõik"}

Pange tähele kasutamine `searchMode=all` kohal. Sh parameeter alistab vaikimisi `searchMode=any`, tagades mis `-motel` tähendab "Ja ei" asemel "Või mitte". Ilma `searchMode=all`, saate "Või NOT" mida laiendatakse asemel piirab otsingutulemite ja seda saab Counter-intuitiivne mõnele kasutajale.

15) Dokumentide otsimine registris olev [lucene päringu](https://msdn.microsoft.com/library/mt589323.aspx)süntaksit. See päring tagastab hotellist, kus Kategooriaväli sisaldab ka terminit "eelarve" ja otsitavate kõik väljad, mis sisaldavad fraasi "vastremonditud". Dokumendid, mis sisaldavad fraasi "vastremonditud" on kõrgemal tulemusena Termini boost väärtus (3)

    GET /indexes/hotels/docs?search = kategooria: eelarve ja \"vastremonditud\"^ 3 ja searchMode = all & api-versioon 2015 02 28 eelvaates = & querytype = täielik

    POSTITUSE /indexes/hotels/docs/search?api-version 2015 02 28 eelvaates = {"otsing": "kategooria: eelarve ja \"vastremonditud\"^ 3", "queryType": "täis", "searchMode": "kõik"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Dokumendi otsing

Selle toimingu **Otsing dokumendi** toob dokumendi Azure'i otsing. See on kasulik, kui kasutaja klõpsab teatud otsingutulemus ja soovite selle dokumendi kohta üksikasjalikku teavet otsida.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Taotlemine**

HTTPS on vaja hooldustaotlused. **Otsing dokumendi** taotluse saate koostatud järgmiselt.

    GET /indexes/[index name]/docs/key?[query parameters]

Teise võimalusena saate olulisi otsing traditsiooniline OData süntaks:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Taotluse URI sisaldab [registri nimi] ja [klahv], määrata, milline dokument toomiseks Milline indeks. Saate ainult ühe dokumendi korraga. Kasutage **otsingut** saada mitme dokumendi ühe päringu.

**Päringu parameetrite**

`$select=[string]`(valikuline) – toomiseks komaga eraldatud väljade loend. Kui määratlemata või `*`, kõik väljad, mis on märgitud nii tagastatava skeemi kaasatud projektsiooni.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

Märkus: selle toimingu jaoks soovitud `api-version` on määratud Päringuparameetri.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenuse URL-i stringiväärtus. **Otsing dokumendi** taotluse saate määrata administraator klahv või päringu võti `api-key`.

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Mitte keegi.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Näide**

Otsing dokumendi, mis sisaldab klahvi "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Otsing dokumendi, mis on klahvi '3' OData süntaksi abil:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Dokumentide arv

**Count dokumentide** toiming toob arv otsing indeks dokumentide arv. Funktsiooni `$count` süntaks on osa OData Protocol (protokoll).

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Taotlemine**

HTTPS on vaja hooldustaotlused. **Count dokumentide** taotluse saate ehitatakse GET meetodi abil.

[Registri nimi] taotluse URI ütleb teenuse tagastamiseks kõik üksused dokumentide saidikogumi määratud indeks.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised.

- `Accept`: See väärtus peab olema seatud `text/plain`.
- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenuse URL-i stringiväärtus. **Count dokumentide** taotluse saate määrata administraator klahv või päringu võti `api-key`.

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Mitte keegi.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

Vastuse kehatekstis leiduvad count väärtus täisarv, mis on vormindatud tekstina.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Soovitused

**Soovitatud** toiming toob osalist otsingut sisendi soovitusi. Tavaliselt kasutatakse seda otsinguväljale, kui kasutajad on otsinguterminid sisestada ettetippimise soovituste andmiseks.

Päringusoovituste taotlusi eesmärk kujutav target dokumendid, et soovitatud teksti võib korrata, kui mitu testversiooni dokumentide match sama otsida sisestatud. Saate kasutada `$select` tuua muude dokumendi väljad (sh dokumendi võti) nii, et saate teada, milline dokument on iga päringusoovituste allikas.

**Soovitatud** toiming on välja toomine või POSTITUSSE päringuga.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Millal kasutada postituse asemel hankimine**

Kui kasutate helistamiseks **soovituste** API HTTP GET, peate võtke arvesse, et taotluse URL-i pikkus ei tohi ületada 8 KB. See on tavaliselt piisavalt enamiku rakenduste puhul. Siiski mõned rakendused, andes väga suurte päringud, spetsiaalselt OData filtriavaldiste. Nende rakenduste abil HTTP POST on parem valik kuna see lubab suurem kui GET filtrid. POSTITUSE, on klauslitest filtri arvu piiramine tegur, ei mahu töötlemata filter stringi taotluse postituse mahupiirang on umbes 16 MB.

> [AZURE.NOTE] POSTITUSE taotluse mahupiirangu on väga suurte, ei saa meelevaldselt keerukate filtriavaldiste. Lisateavet [OData avaldise süntaksisse](https://msdn.microsoft.com/library/dn798921.aspx) filter keerukuse piirangute kohta.

**Taotlemine**

HTTPS on vaja hooldustaotlused. **Soovitused** taotluse saate ehitatakse meetoditega toomine või POSTITUSSE.

Taotluse URI määrab registri päringu nime. Parameetrite osaliselt Sisestuskeel otsingutermin, näiteks on ära toodud päringustringi puhul Saada kutsed ja taotluse taotleb keha postituse puhul.

Hea tava Saada kutsed loomisel, ärge unustage [URL-i kodeerida](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) konkreetse päringu parameetrite otse REST API helistades. **Soovitused** toimingute, see hõlmab:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-i kodeerimine on soovitatav ainult päringu ülaltoodud parameetritel. Kui te tahtmatult URL-i kodeerida kogu päringustringi (kõik pärast selle?), kutsed kuvatakse leheküljepiiri.

Lisaks URL-i kodeeringus on vajalik ainult helistades otse kasutades GET REST API-ga. URL-i kodeerimine on vaja helistamisel **soovituste** abil postituse või kasutades [.net-i kliendi teek](https://msdn.microsoft.com/library/dn951165.aspx), mis tegeleb URL-i kodeerimine teie eest.

**Päringu parameetrite**

**Soovitused** aktsepteerib Päringukriteeriumide anda ja määrata ka otsing käitumine mitmed parameetrid. Esitate järgmiste parameetrite URL-i päringu stringi helistamisel **soovituste** hankimine kaudu ja JSON atribuutidena koosolekukutse kehasse helistamisel **soovituste** postiga. Mõned parameetrid süntaks on pisut erinev hankimine ja POSTITA. Erinevused andmeteks rakendatav all:

`search=[string]`-Otsitav tekst soovitamine päringute abil. Peab olema vähemalt 1 märk ja maksimaalselt 100 märki.

`highlightPreTag=[string]`(valikuline) - stringi sildistamine, et otsida tabamust prepends. Peab olema seatud koos `highlightPostTag`.

> [AZURE.NOTE] Kui helistades **soovituste** abil toomine, URL-i reserveeritud märkide tuleb protsendi-kodeerida (nt asemel # % 23).

`highlightPostTag=[string]`(valikuline) - stringi sildistamine, mis lisab tabamust otsimiseks. Peab olema seatud koos `highlightPreTag`.

> [AZURE.NOTE] Kui helistades **soovituste** abil toomine, URL-i reserveeritud märkide tuleb protsendi-kodeerida (nt asemel # % 23).

`suggesterName=[string]`-suggester määratletud nimi on `suggesters` saidikogumis, mis on osa registri määratlemine. A `suggester` määratleb, milliseid välju kontrollitakse soovitatud päringutingimused. Üksikasjad leiate [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(valikuline, vaikimisi = false) – Kui seatud väärtusele tõene see API otsib soovituste isegi juhul, kui Otsitav tekst on asendatud või puuduvate märkide. Kuigi see mugavamaks mõnel juhul tegemist jõudluse maksumus udune päringusoovituste otsingud on aeglasem ning rohkem ressursse.

`searchFields=[string]`(valikuline) - määratud teksti välja komaga eraldatud nimede loendi. Sihtrakenduse väljad peab olema lubatud soovitusi.

`$top=#`(valikuline, vaikimisi = 5)-soovituste toomiseks arv. Peab olema arv 1 ja 100 vahel.

> [AZURE.NOTE] **Soovituste** abil postituse helistamisel see parameeter on nimega `top` asemel `$top`.

`$filter=[string]`(valikuline) - avaldis, mis filtreerib dokumentide kaaluda soovitusi.

> [AZURE.NOTE] **Soovituste** abil postituse helistamisel see parameeter on nimega `filter` asemel `$filter`.

`$orderby=[string]`(valikuline) - komaga eraldatud avaldiste tulemite järgi sortimiseks loendit. Iga avaldise võib olla välja nime või kõne on `geo.distance()` funktsioon. Iga avaldise saate järgneb `asc` soovite tõusvas järjestuses, nagu on näidatud ja `desc` tähistamiseks laskuvas järjestuses. Vaikimisi on tõusvas järjestuses. Piiratud 32-klauslitest jaoks `$orderby`.

> [AZURE.NOTE] **Soovituste** abil postituse helistamisel see parameeter on nimega `orderby` asemel `$orderby`.

`$select=[string]`(valikuline) – toomiseks komaga eraldatud väljade loend. Kui määramata, tagastatakse ainult dokumendi võti ja soovituste teksti. Saate otseselt taotleda kõik väljad, parameetri `*`.

> [AZURE.NOTE] **Soovituste** abil postituse helistamisel see parameeter on nimega `select` asemel `$select`.

`minimumCoverage`(valikuline, 80 vaikesätted) – arv vahemikus 0 – 100 indeks, mis peab olema hõlmatud soovitused selleks, et päring olema staatusega õnnestumise päringu protsentides. Vaikimisi peab olema vähemalt 80% register saadaval või `Suggest` tagastab HTTP olekukoodi 503. Kui seate `minimumCoverage` ja `Suggest` õnnestub, see tagastada HTTP 200 ja kaasata on `@search.coverage` indeks, mis lisati päringu protsentides vastus väärtus.

> [AZURE.NOTE] Parameetri väärtus väiksem kui 100 võib olla kasu otsingu kättesaadavus isegi ainult üks koopia teenuste tagamiseks. Siiski kõiki vastavaid soovitusi on tagatud tulemuste esitus. Kui tagasivõtmist olulisem rakenduse kättesaadavus, siis on parem vähendage `minimumCoverage` all 80 vaikeväärtus.

`api-version=[string]`(nõutav). Eelvaateversiooni on `api-version=2015-02-28-Preview`. Vaadake [Otsingu teenuse versioonimise](http://msdn.microsoft.com/library/azure/dn864560.aspx) üksikasjad ja alternatiivne versioonid.

Märkus: selle toimingu jaoks soovitud `api-version` on määratud Päringuparameetri URL-i sõltumata sellest, kas helistate **soovituste** hankimine või postitus.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised

- `api-key`: Mida `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenuse URL-i stringiväärtus. **Soovitused** taotluse saate määrata administraator klahv või päringu klahvi soovitud `api-key`.

Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua Azure portaali kaudu. Teemast [Azure otsinguteenuse portaali loomine](search-create-service-portal.md) aitab lehe navigeerimiseks.

**Koosolekukutse kehasse.**

Saada: pole.

Post:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Kui suvand projektsiooni kasutatakse toomiseks massiivi iga element on need sisalduvad väljad:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Näide**

Saate tuua 5 soovitusi, kus osalist otsingut on "lux"

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
