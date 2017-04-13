<properties
    pageTitle="Hinded profiilid (Azure Search REST API versioon 2015-02-28-eelvaade) | Microsoft Azure'i | Azure'i otsingu eelvaade API"
    description="Azure'i otsing on majutatud cloud otsingu teenus, mis toetab häälestus hinnatud tulemid, mis põhineb kasutaja määratletud hinded profiilid."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Hinded profiilid (Azure'i Search REST API versioon 2015-02-28-eelvaade)

> [AZURE.NOTE] Selles artiklis kirjeldatakse hinded profiilid [2015 02 28 eelvaates](search-api-2015-02-28-preview.md). Praegu ei erine selle `2015-02-28` [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) -is dokumenteerida versioon ja `2015-02-28-Preview` siin kirjeldatud versioon, kuid pakume selle dokumendi niikuinii üle kogu API dokumendi ulatus pakkumiseks.

## <a name="overview"></a>Ülevaade

Hinded viitab otsing Keskmine iga üksuse otsingutulemite arvutamisel. Hinde on mõni üksus asjakohasuse otsingu praeguse toimingu kontekstis näidikut. Mida rohkem punkte, rohkem asjakohaseid üksus. Otsingutulemites üksused on tellitud kõrge, madal, põhineb otsing Keskmine, mis on arvutatud iga üksuse jaoks asukoht.

Azure'i otsing kasutab vaikimisi hinded arvutada on algse Keskmine, kuid arvutus hinded profiili kaudu saate kohandada. Hinded profiilid teile üksuste järjestuse üle suuremat kontrolli tulemusi. Näiteks võite üksusi vastavalt nende müügitulu võimalikud suurendada, edendada uuem üksuste või ehk suurendada üksused, mis on liiga pikk laoseisu.

Hinded profiili on osa index määratlus, väljad, funktsioonid ja parameetrid.

Järgmises näites on kujutatud teile aimu, milline näeb välja hinded profiili, lihtne profiil, nimega "geo". Sellest sisutüübist suurendab üksused, mis on otsingusõna soovitud `hotelName` välja. Samuti kasutab funktsiooni `distance` funktsiooni kasuks üksused, mis jäävad kümme km praeguse asukoha. Kui keegi otsib Termini "inn" ja 'inn' juhtub tuleb teha nime, kuvatakse dokumendid, mis sisaldavad hotellist Inn-' kõrgema otsingutulemites.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Seda hinded profiili kasutamiseks päring on koostatud profiili määramiseks klõpsake päringu string. Teade allpool päringusse Päringuparameetri, `scoringProfile=geo` taotlusega.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Selle päringu Termini "inn" otsib ja edastab praeguses asukohas. See päring sisaldab muid parameetreid, näiteks märkuse `scoringParameter`. Päringu parameetrite on kirjeldatud [Otsi dokumente (Azure'i otsingu API)](search-api-2015-02-28-preview.md#SearchDocs).

[Näide](#example) üksikasjalikumat näide hinded profiili vaatamiseks klõpsake.

## <a name="what-is-default-scoring"></a>Mis on vaikimisi hinded?

Hinded arvutab otsingu Keskmine rank järjestatud tulemihulga iga üksuse jaoks. Iga üksuse otsingu tulemite hulk on määratud otsingu Keskmine, siis järjestatud kõrgeim väikseimani. Rakenduse tagastatakse üksused, mille kõrgemad hinded. Top 50 tagastatakse vaikimisi, kuid saate kasutada funktsiooni `$top` parameetri üksused (kuni 1000 ühe vastuse) suurema või väiksema arvu tagastamiseks.

Vaikimisi on otsingu keskmine arvutatud statistilisi näitajaid, andmed ja päringu põhjal. Azure'i otsing leiab dokumendid, mis sisaldab otsinguterminid päringu stringi (või kõik, sõltuvalt `searchMode`), soodustades dokumendid, mis sisaldavad otsingusõna paljudel juhtudel. Otsingu Keskmine läheb üles isegi suurem kui tähtaeg on harv üle andmete corpus, kuid levinud dokumendi sees. TF-IDF tuntakse seda lähenemisviisi arvuti asjakohasuse alusel või (Termini sagedus-väärtuse dokumendi sagedus).

Eeldades, et on kohandatud sortimist, tulemused seejärel järjestatud otsingu Keskmine enne tagastatakse funktsiooni helistaja rakendus. Kui `$top` pole määratud, 50 kaubad, millel on kõrgeim otsing, tagastatakse Keskmine.

Otsingu hinde väärtused saate korrata kogu tulemihulga. Näiteks, peate võib-olla 10 üksused, mille on keskmine 1,2, 20 on keskmine 1,0 ja 20 üksused on keskmine 0,5. Kui mitu tabamust on sama otsing Keskmine, poolitusjoonega samad üksused tellimine on määratletud ning ei ole püsiv. Käivita päring uuesti, ja võidakse kuvada üksused shift asukoht. Antud kahe üksused on identne Keskmine, ei ole mingit garantiid, millise neist kuvatakse esimesena.

## <a name="when-to-use-custom-scoring"></a>Millal kasutada kohandatud hinded

Ühe või mitme hinded profiilid loomist peaks vaikimisi järjestamine käitumine ei lähe piisavalt kaugele tekstivormingus koosoleku oma ettevõtte eesmärke. Näiteks võib juhtuda, et otsing tekst peaks kasuks äsja lisatud üksused. Samuti võib olla välja, mis sisaldab kasum (bruto) või mõne muu välja võimalike tulu, mis näitab. Suurendamine tabamust, mis toovad teie ettevõtte kasu võib olla on oluline kasutada hinded profiili otsustades.

Asjakohasus-põhiste tellimine rakendatakse ka kaudu hinded profiilid. Kaaluge võimalust varem, mille abil saate sortida hinna, kuupäeva, hinnangute või asjakohasuse kasutasite otsingutulemuste lehtedel. Azure'i otsinguväljale hinded profiilid juhtida suvandi "tekst". Asjakohasuse määratluse kontrollib teie ettevõtte eesmärkide ja otsingut, mida soovite esitada tüüpi eelduseks.

<a name="example"></a>
## <a name="example"></a>Näide

Nagu, kohandatud hinded rakendatakse kaudu määratletud skeemiga index hinded.

Selles näites on kujutatud kahe hinded profiilid registri skeemi (`boostGenre`, `newAndHighlyRated`). Mis tahes päring selle indeksi, mis sisaldab üks profiil Päringuparameetri kasutada profiili Keskmine tulemite hulk.

[Proovige järgmises näites](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Töövoo

Rakendada kohandatud hinded käitumine, lisada skeem, mis määratleb registri hinded profiili. Saate määrata mitu hinded profiili registri sees, kuid mis tahes päringu ajal saate määrata ainult üks profiil.

Selles teemas antud [malli](#bkmk_template) alustada.

Sisestage nimi. Hinded profiilid on valikuline, kuid kui lisate ühte, nimi on nõutav. Järgige nimereeglid väljade jaoks (algab tähega, hoiab ära erimärgid ja reserveeritud sõnad). Lisateabe saamiseks vaadake [Nime andmise reeglid](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Keha hinded profiil koosneb mitmesugustest kaalutud välju ja funktsioone.

### <a name="weights"></a>Kaalu ###

Funktsiooni `weights` hinded profiili atribuut määrab nime ja väärtuse paarideks, mille suhteline jämeduse määramiseks välja. [Näide](#example)albumTitle, Žanr ja artistName väljad on võimendatud 1,5, 5 ja 2, vastavalt. Miks on Žanr tõsta nii palju suuremad kui teised? Kui otsing toimub üle andmete, mis on mõnevõrra ühtlase (nagu "Žanr" puhul on selle `musicstoreindex`), peate võib-olla suurem dispersioon suhteline kaal. Näiteks on `musicstoreindex`, 'rock' kuvatakse nii Žanr ja ühtemoodi sõnastas Žanr kirjeldused. Kui soovite Žanr üles Žanr kirjeldus, tuleb Žanr välja palju suurema suhtelise kaalu.

### <a name="functions"></a>Funktsioonid ###

Kui täiendavad arvutuste jaoks teatud kontekstides vajalike kasutada funktsioone. Funktsioon lubatud tüübid on `freshness`, `magnitude`, `distance` ja `tag`. Iga funktsiooni on parameetrid, mis esinevad vaid selle.

  - `freshness`Kui soovite suurendada tuleb kasutada, kuidas uue ja vana üksuse on. Seda funktsiooni saab kasutada ainult kuupäeva ja kellaaja väljad (`Edm.DataTimeOffset`). Märkus selle `boostingDuration` atribuuti kasutatakse ainult funktsiooniga värskus.
  - `magnitude`tuleks kasutada, kui soovite suurendada põhjal kuidas kõrge või madala arvulise väärtuse on. Stsenaariumid, mis nõuavad see funktsioon kaasata kasum (bruto), kõrgeim hind, madalaim hind või allalaadimise arvu suurendamine. Saate tühistada vahemikus, kõrge, madal, kui soovite pööratud mustri (nt madalama hinnaga boost üksuste rohkem kui kõrgema hinnaga üksused). 100-eurose vahemiku hindade pöörata $1 saate määrata `boostingRangeStart` 100 ja `boostingRangeEnd` 1 suurendada madalama hinnaga üksused. Seda funktsiooni saab kasutada ainult väljadega kahekordne ja täisarvuni.
  - `distance`tuleks kasutada, kui soovite suurendada lähedus või geograafiline asukoht. Seda funktsiooni saab kasutada ainult `Edm.GeographyPoint` väljad.
  - `tag`tuleks kasutada, kui soovite suurendada siltide ühiselt dokumentide ja otsingupäringuid vahel. Seda funktsiooni saab kasutada ainult `Edm.String` ja `Collection(Edm.String)` väljad.
  
#### <a name="rules-for-using-functions"></a>Reeglid funktsioonide abil ####

  - Funktsioon type (värskus, suurus, kauguse, sildi) peab olema väiketähti.
  - Funktsioonid ei tohi sisaldada nullväärtusega ega tühjad väärtused. Eelkõige sellest, kui kaasate väljanimi, tuleb määramine midagi.
  - Funktsioone saab rakendada ainult filtreeritavad väljad. [Registri loomine](search-api-2015-02-28-preview.md#CreateIndex) vt lisateavet filtreeritavad väljad.
  - Funktsioone saab rakendada ainult väljad, mis on määratletud registri väljade kogum.

Kui indeks on määratletud, koostada üleslaadimisel index skeemiga, dokumendid, millele järgneb register. Järgmiste toimingute juhised teemast [Registri loomine](search-api-2015-02-28-preview.md#CreateIndex) ja [lisamine või värskendamine dokumendid](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Kui indeks on ehitatud, peaks teil olema otstarbekas hinded profiili, mis töötab koos oma andmetest otsimine.

<a name="bkmk_template"></a>
## <a name="template"></a>Mall
Selles jaotises kirjeldatakse süntaks ja malli hinded profiilid. Vaadake [Index atribuudi viide](#bkmk_indexref) järgmises jaotises Atribuudid kirjeldus.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Kasutajaprofiili atribuudi viide hinded

**Märkus** Hinded funktsioon saab rakendada ainult filtreeritavad väljad.

| Atribuut | Kirjeldus |
|----------|-------------|
| `name`   | Nõutav. See on hinded profiili nimi. See järgib sama nimereeglid välja. See peab algama tähega, ei tohi sisaldada punkti, kooloni või @ sümbolid ja ei saa käivitada, mille teemas on fraas "azureSearch" (tõstutundlik). |
| `text` | Sisaldab kaalu atribuuti. |
| `weights` | Valikuline. Nimi väärtus väärtus, mis määrab välja nimi ja suhteline paksus. Suhteline paksus peab olema positiivne täisarv või Ujukomaarv. Saate määrata välja nimi ilma vastavate paksus. Ühe välja teise suhtes tähtsuse kasutatakse kaalu. |
| `functions` | Valikuline. Pange tähele, et hinded funktsioon saab rakendada ainult filtreeritavad väljad. |
| `type` | Nõutav hinded funktsioonide jaoks. Näitab tüüpi funktsiooni kasutamiseks. Lubatud väärtused sisaldavad `magnitude`, `freshness`, `distance` ja `tag`. Saate lisada rohkem kui ühe funktsiooni iga hinded profiili. Funktsiooni nimi peab olema väiketähti. |
| `boost` | Nõutav hinded funktsioonide jaoks. Positiivne arv, mida kasutatakse mitmekordne töötlemata näitaja. Ei saa see olla võrdseks 1. |
| `fieldName` | Nõutav hinded funktsioonide jaoks. Hinded funktsioon saab rakendada ainult väljad, mis on osa registri väljakollektsiooni ja mis on filtreeritavad. Lisaks tutvustab iga funktsioon type (värskus kasutatakse kuupäeva ja kellaaja väljadega ulatuse täisarv või kahekordseks väljadega, vahemaa väljadega asukoht ja silt tekstistring või tekstistringi saidikogumi väljad) täiendavad piirangud. Saate määrata ainult ühe välja funktsioon määratluse kohta. Näiteks kasutada ulatuse kaks korda sama profiili, peaksite kaasata kaks määratlused suurust, üks iga välja jaoks. |
| `interpolation` | Nõutav hinded funktsioonide jaoks. Määratleb tõusu, mille suurendamine suureneb vahemiku algusest lõpuni vahemiku hinde. Lubatud väärtused sisaldavad `linear` (vaikeväärtus) `constant`, `quadratic`, ja `logarithmic`. Üksikasjad leiate [seadmine Vahemärkuste tegemine](#bkmk_interpolation) . |
| `magnitude` | Funktsioon hinded suurus kasutatakse muuta tulemused väärtused arvväärtusega välja lahtrivahemiku põhjal. Mõned kõige levinum kasutamise näited sellest on:<ul><li>Tähehinnangud: hinded väärtuse "Täht hindamine" välja muuta. Kui kaks on oluline, üksuse kõrgema hinne kuvatakse esimesena.</li><li>Veerise jaoks soovitud väärtus: Kui kaks dokumendid on oluline, jaemüüjalt soovi korral dokumendid, mis on suurem marginaalid esmalt suurendada.</li><li>Klõpsake loendab: toimingud toodete või lehtede kaudu, klõpsake linki rakenduste jälgimine, võite kasutada ulatuse suurendamiseks üksustele, et saada kõige liiklus.</li><li>Loendab allalaadimine: rakenduste Jälita allalaaditavad failid ulatuse funktsioon võimaldab teil suurendada üksused, mis on kõige allalaaditavad failid.</li></ul> |
| `magnitude:boostingRangeStart` | Määrab vahemik, mille suurus hinne algväärtus. Väärtus peab olema täis- või Ujukomaarv. See oleks tähtedega 1 – 4, 1. Veeriste rohkem kui 50%, oleks 50. |
| `magnitude:boostingRangeEnd` | Määrab end väärtus vahemikus, mille suurus hinne. Väärtus peab olema täis- või Ujukomaarv. 1 – 4 tähtedega, see oleks 4. |
| `magnitude:constantBoostBeyondRange` | Sobivad väärtused on tõene või väär (vaikesäte). Kui väärtuseks on seatud tõene, täielikku suurendada jätkatakse dokumentidele, mille väärtus on suurem kui vahemiku ülemises otsas sihtvälja. Kui väärtus on false, ei selle funktsiooni mikrofonivõimendus rakendatud dokumendid, väärtust, mis langeb töölehevahemikust välja sihtväärtus. |
| `freshness` | Funktsioon hinded värskus kasutatakse muuta järjestuse hinded üksuste DateTimeOffset välja väärtuste põhjal. Näiteks saate üksuse hilisemaks kõrgem kui vanemate üksuste kohal. (Pange tähele, et samuti on võimalik rank üksuste kalendrisündmuste koos tulevaste kuupäevade nii, et üksuste lähemale kohal saate olla kõrgemal kui üksuste täpsemaks tulevikus.) Teenuse praeguses versioonis, määratakse üks ots vahemiku praeguse kellaaja. Teises otsas on varem põhjal aeg on `boostingDuration`. Suurendamiseks kasutada mitmesuguseid tulevikus negatiivne `boostingDuration`. Mis on suurendamine vahetatakse kuni ja vähim ulatus on määratud soovitud interpoleerimise rakendatud hinded profiilile (vt alltoodud joonist). Rakendatud suurendada tegur tühistamiseks valige boost teguri väiksem kui 1. |
| `freshness:boostingDuration` | Pärast millist suurendamine kuvamisperioodi katkestab kindla dokumendi komplektid. Vt [määramine boostingDuration] [#bkmk_boostdur] kohta järgmisest jaotisest süntaks ja näited. |
| `distance` | Hinded funktsiooni kasutatakse mõjutada kauguse kohta, kuidas põhinevates dokumentides Keskmine sulgemine või palju need on viide geograafilise asukoha suhtes. Viide asukoht on antud parameetri päringu osana (abil soovitud `scoringParameter` päringu parameetrite) nimega lon, Läti argument. |
| `distance:referencePointParameter` | Parameetri tuleb edasi päringute viide asukohta. scoringParameter on Päringuparameetri. Päringu parameetrite kirjeldused leiate [Otsi dokumente](search-api-2015-02-28-preview.md#SearchDocs) . |
| `distance:boostingDistance` | Arv, mis näitab, et viide asukohast, kus suurendada vahemikus lõpeb km kaugus. |
| `tag` | Funktsioon hinded silti kasutatakse mõjutavad Keskmine dokumentide dokumentides ja otsingupäringuid siltide põhjal. Dokumendid, millel on sildid ühist otsingupäringu ei tõsta. Siltide Otsingupäringu jaoks on esitatud iga Otsingutaotluse hinded parameetrina (abil soovitud `scoringParameter` päringu parameetrite). |
| `tag:tagsParameter` | Parameetri päringute määramiseks sildid kindla taotluse vastu. `scoringParameter`Päringuparameetri on. Päringu parameetrite kirjeldused leiate [Otsi dokumente](search-api-2015-02-28-preview.md#SearchDocs) . |
| `functionAggregation` | Valikuline. Kehtib ainult siis, kui funktsioonid on määratud. Lubatud väärtused sisaldavad: `sum` (vaikeväärtus) `average`, `minimum`, `maximum`, ja `firstMatching`. Otsingu Keskmine on üks väärtus, mis on arvutatud mitme muutuja, sh mitmeid funktsioone. Selle atribuute näitab, kuidas suurendab kõik funktsioonid on koondatud ühe liitväärtuse suurendada, mis rakendatakse siis base dokumendi Keskmine. Base Keskmine põhineb tf-idf väärtus arvutada dokumendi ja päring kaudu. |
| `defaultScoringProfile` | Täitmisel Otsingutaotluse, kui ühtegi hinded profiili pole määratud, siis vaikimisi hinded on kasutatud (tf-idf ainult). Hinded profiili nimi vaikimisi saate seada siin Azure'i otsingu selle profiili kasutamiseks, kui ühtegi teatud profiili Otsingutaotluse põhjustada. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Seadmine Vahemärkuste tegemine

Vahemärkuste tegemine abil saate määratleda tõusu mille keskmine, suureneb vahemiku algusest lõpuni vahemiku suurendamine. Saab kasutada järgmist Vahemärkuste tegemine.

  - `Linear`: Jaoks üksused, mis on vahemikus max ja min tehakse suurendada, rakendatakse üksusele pidevalt kahanevas summa. Lineaarne on vaikimisi interpoleerimise hinded profiili.
  - `Constant`: Üksused, mis on käivitamine ja lõpeb vahemikus, seotakse konstandi boost rank tulemused.
  - `Quadratic`: Võrreldes lineaarse interpoleerimise, mis on pidevalt kahanevas suurendada, Ruutvõrrandi väheneb esialgu väiksem tempos ja siis, kui see end vahemikus, see väheneb palju suuremad intervalliga. Suvand interpoleerimise sildi hinded funktsioonid pole lubatud.
  - `Logarithmic`: Võrreldes lineaarse interpoleerimise, mis on pidevalt kahanevas suurendada, logaritmiline väheneb algselt kõrgema tempos ja siis kui see end vahemikus, see väheneb väiksema intervalli. Suvand interpoleerimise sildi hinded funktsioonid pole lubatud.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>BoostingDuration määramine

`boostingDuration`on atribuut värskus funktsiooni. Saate määrata ka aegumise pärast suurendada mis katkestab dokumendi teatud perioodi. Näiteks suurendamiseks tootesarja või kaubamärgi reklaami 10 päeva jooksul, saate määrata 10-päevast perioodi nimega "P10D" nimetatud dokumente. Või suurendamiseks saate määrata järgmise nädala eelseisvaid sündmusi "-P7D".

`boostingDuration`peate vormindatud XSD-"dayTimeDuration" väärtus (piiratud Alamhulk on ISO 8601 kestus väärtus). See muster on: `[-]P[nD][T[nH][nM][nS]]`.

Järgmises tabelis on ära toodud mitu näidet.

| Kestus | boostingDuration |
|----------|------------------|
| 1 päeva | "P1D" |
| 2 päeva ja 12 tunni | "P2DT12H" |
| 15 minutit | "PT15M" |
| 30 päeva, 5 tundi, 10 minutit, ja 6.334 sekundit | "P30DT5H10M6.334S" |

Rohkem näiteid leiate [XML-skeemi: andmetüübid (W3.org veebisait)](http://www.w3.org/TR/xmlschema11-2/).

**Vt ka**
[Azure otsingu teenuse REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) MSDN-is <br/>
MSDN-i [jaoks luuakse register (Azure'i otsing API)](http://msdn.microsoft.com/library/azure/dn798941.aspx)<br/>
[Lisa otsing indeks hinded profiili](http://msdn.microsoft.com/library/azure/dn798928.aspx) MSDN-is<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
