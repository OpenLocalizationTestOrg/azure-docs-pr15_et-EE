<properties
    pageTitle="Koosta tüübid ja mudeli kvaliteet: soovitusi API | Microsoft Azure'i"
    description="Azure'i masina õ soovitused--Lühijuhend"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Koosta tüübid ja mudeli kvaliteet #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Toetatud koostamine tüübid ##

Soovitused API toetab praegu Koosta kahte tüüpi: *soovitus* ja *FBT*. On erinevate tugevuste ja iga ehitatakse abil erinevate algoritmide kohta. Selles dokumendis kirjeldatakse iga need järgud ja võtteid loodud mudelid kvaliteedi võrdlus.

Kui te pole teinud juba, soovitame lõpuleviimist [Lühijuhend juhend](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Soovitus koostada tüüp ###

Soovitus koostamine tüüp kasutab maatriksi Faktoriseerimine soovitusi. See loob [peidetud funktsiooni](https://en.wikipedia.org/wiki/Latent_variable) vektorite põhjal tehinguid kirjeldamiseks iga üksuse ja võrdlemiseks üksused, mis sarnanevad kasutab need peidetud vektorite.

Kui koolitada mudeli vastavalt teie elektroonika poes ostude ja sisestage Lumia 650 telefoni mudeli sisendina, mudeli tagastab üksused, mis on tavaliselt inimesed, kes võivad osta Lumia 650 telefoni osta kogumist. Üksusi ei pruugi komplementaarse. Selles näites on võimalik mudeli tulemuseks muudele numbritele, kuna inimestele, kes soovivad Lumia 650 nagu muudele numbritele.

Soovitus koostamine on kaks võimalusi, mis muudavad kena ülevaate.

* *Soovituse koostamine toetab *külma üksuse* paigutuse **

Üksused, mis on oluline kasutus nimetatakse külma üksused. Näiteks kui saate telefoni olete kunagi müünud enne saatmist, süsteemi ei saa tuletab selle toote tehingute eraldi soovitused. See tähendab, et õppida süsteemi toote enda kohta teabe.

Kui soovite kasutada külma üksuse paigutuse, peate funktsioonid teavet iga teie kataloogis olevad üksused. Järgmine on esimesed read oma kataloogi võib-olla näha (Märkus klahv = väärtus vorming funktsioonide jaoks).

>6CX-00001, pinna Pro2, Surface, tippige = riistvara salvestusruumi = 128 GB mälu = 4G, tootja = Microsoft

>73H-00013, pärast Xbox 360, mängimine,, tüüp = tarkvara keel inglise, hindamine = = küps

>WAH-0F05, Minecraft Xbox 360, mängimine, * tüüp = tarkvara, Language = Hispaania, hindamine = noorte

Samuti peate määrama koostamine järgmisi:

| Parameetri koostamine         | Märkmete
|------------------     |-----------
|*useFeaturesInModel*     | Määrake väärtuseks **true**.  Näitab, kui funktsioone saab kasutada soovitus andmemudeli täiustamiseks.
|*allowColdItemPlacement*   | Määrake väärtuseks **true**. Näitab, kui soovitust peaks ka push külma üksuste funktsioon sarnased kaudu.
| *modelingFeatureList*   | Funktsioon nimede kasutatava soovitus koostamine täiustamiseks soovitust komaga eraldatud loend. Näiteks "keel, salvestusruumi" eelnev näide.

**Soovituse koostamine toetab kasutaja soovitusi**

Soovitus koostamine toetab [kasutaja soovitusi](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). See tähendab, et tagavad isikupärastatud soovitused kasutajad vastavalt oma tehingu ajalugu. Kasutaja soovitusi, saate luua kasutaja ID-d või tehingud selle kasutaja tehtud ajalugu.

Üks klassikaline näide, kus võiksite rakendada kasutaja soovitused on lehel Tere tulemast sisselogimine. Seal saate reklaamida sisu, mis kehtib teatud kasutaja.

Samuti võite rakendada soovitused Koosta tüüp, kui kasutaja on välja. Sel hetkel teil on kasutajal on osta üksuste loendit, ja saate sisestada praeguse market korv soovitusi.

#### <a name="recommendations-build-parameters"></a>Soovitused koostada parameetrid

| Nimi  |   Kirjeldus |    Tüüp <br>  lubatud väärtusi, <br> (vaikeväärtus)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Mudeli sooritab iteratsiooni arv kajastub Arvuta aega ja mudeli täpsuse. Suurem arv, seda täpsem mudelit, kuid Arvuta aega võtab rohkem aega.  |   Täisarv, <br>  10 – 50 <br>(40)
| *NumberOfModelDimensions* |   Dimensioonide arvu, mis on seotud mudeli proovib leida oma andmete funktsioonide arvu. Arvu mõõtmed võimaldab paremini peenhäälestus tulemuste väiksemate kogumite sisse. Siiski liiga palju mõõtmed takistada mudeli leidmist üksuste vahel. |  Täisarv, <br> 10 kuni 40 <br>(20). |
| *ItemCutOffLowerBound* |  Määratleb kasutus punktide üksuse peaks olema seda lugeda näidise väikseima arvu. |        Täisarv, <br> 2 või rohkem <br> (2) |
| *ItemCutOffUpperBound* |  Määratleb üksuse peaks olema seda loetakse osa mudeli kasutamine punktide arvu ülempiir. |  Täisarv, <br>2 või rohkem<br> (2147483647) |
|*UserCutOffLowerBound* |   Määratleb kasutaja peab olema tehtud loetakse näidise väikseima arvu. | Täisarv, <br> 2 või rohkem <br> (2)
| *UserCutOffUpperBound* |  Määratleb kasutaja peab olema tehtud käsitletakse näidise maksimaalne arv. | Täisarv, <br>2 või rohkem <br> (2147483647)|
| *UseFeaturesInModel* |    Näitab, kui funktsioone saab kasutada soovitus andmemudeli täiustamiseks. |     Kahendmuutuja<br> Vaikimisi: True
|*ModelingFeatureList* |    Funktsioon nimede kasutatava soovitus koostamine täiustamiseks soovitust komaga eraldatud loend. See sõltub funktsioone, mis on oluline. |    String, kuni 512 märki
| *AllowColdItemPlacement* |    Näitab, kui soovitust peaks ka push külma üksuste funktsioon sarnased kaudu. | Kahendmuutuja <br> Vaikimisi: False
| *EnableFeatureCorrelation*    | Näitab, kui funktsioone saab kasutada arutluskäigus. | Kahendmuutuja <br> Vaikimisi: False
| *ReasoningFeatureList* |  Funktsioon nimede kasutatakse arutluskäiku laused, nt soovitus selgitused komaga eraldatud loend. See sõltub funktsioonid, mis on oluline, et kliendid. | String, kuni 512 märki
| *EnableU2I* | Luba isikupärastatud soovitusi, nimetatakse ka kasutajal üksuse (U2I) soovitusi. | Kahendmuutuja <br>Vaikimisi: True
|*EnableModelingInsights* | Määratleb, kas ühenduseta hindamiseks tuleb teha koguda modelleerimine ülevaateid (ehk teisisõnu öeldes täpsuse ja mitmekesisuse mõõdikute). Kui see on seatud tõene, andmete alamhulga kasutatakse koolitus, kuna see tuleb reserveeritud testimiseks mudel. Lugege lisateavet [ühenduseta hinnangud](#OfflineEvaluation). | Kahendmuutuja <br> Vaikimisi: False
| *SplitterStrategy* | Kui luba modelleerimine ülevaateid on seatud väärtusele *tõene*, on see, kuidas andmed tuleks jagada hindamiseks.  | Tekstistring, *RandomSplitter* või *LastEventSplitter* <br>Vaikimisi: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>FBT koostamine tüüp ###

Sageli ostetud koos (FBT) koostamine ei analüüsi, mis loendab, mitu korda kahte või kolme eri toodete ühis-esineda koos. See sorditakse komplekti, mis on väga sarnased funktsiooni (**kaasautorluse juhud**, **Jaccard**, **tõstke**) alusel.

Võrrelda **Jaccard** ja **tõstke** normaliseerimine kaasautorluse juhud viisid.  See tähendab, et tagastatakse üksused ainult siis, kui nad kui ostnud koos seemne üksus.

Meie Lumia 650 telefoni näites tagastatakse telefoni X ainult siis, kui X-telefon on ostetud sama seansi nimega soovitud Lumia 650 telefoni. Kuna see võib olla tõenäoline, saame oodata üksuste täiendavad Lumia 650 tagastatakse; näiteks ekraan muutub või power adapterit Lumia 650 jaoks.

Praegu kaks üksused on eeldatakse, et sama seansi ilmnemisel toimingu samas kasutaja ID ja ajatempli osta.

FBT järgud ei toeta külma üksusi, kuna määratluse oodatud kaks sama tehingu ostetavate kaupade. Kuigi FBT järgud võib tagastada failihulgale (kolmest), ei toeta isikupärastatud soovitused Kuna aktsepteerimist ühe seemne üksus on sisendina.


#### <a name="fbt-build-parameters"></a>FBT koostada parameetrid

| Nimi  |   Kirjeldus |       Tüüp  <br> lubatud väärtusi, <br> (vaikeväärtus)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Kuidas konservatiivne mudel on. Tuleb arvestada modelleerimiseks kaasautorluse esinemiskordade arvu. |  Täisarv, <br> 3-50 <br> (6)
| *FbtMaxItemSetSize* | Piire sagedased kogumis olevate üksuste arvu.| Täisarv  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Minimaalne Keskmine sagedased kogum peaks olema tagastatud tulemite kaasamiseks. Mida suurem on parem. | Kahekordne <br> 0 või rohkem <br> (0)
| *FbtSimilarityFunction* | Määratleb koostamine kasutavad funktsiooni sarnased. **Tõstke** eelistab serendipity, soodustab **koostööd esinemiskord** prognoositavuse ja **Jaccard** on kaks kompromiss. | Stringi  <br>  <i>cooccurrence, lifti jaccard</i><br> Vaikimisi: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Hindamis- ja koostamine ##

Juhised võivad aidata teil otsustada, kas peaksite kasutama soovitused koostamine või mõne FBT koostamine, kuid see ei ole juhul, kui võite kasutada ühte neist kindla vastuse. Lisaks isegi juhul, kui teate, et soovite kasutada mõnda FBT Koosta tüüp, endiselt võite valida **Jaccard** või **tõstke** sarnased funktsiooni.

Parim viis valida kahe eri järgud on katse neid tõeline (online hindamine) ja jälgida jaoks eri järgud ulatuses. Teisendamine määr võib sõltuvalt soovitus hiireklõpsuga soovitused arv tegelik oste, või isegi tegeliku ostu summad mõõta eri soovitusi olid juhul, kui. Võite valida teisendamise määr meetermõõdustik vastavalt teie ettevõtte eesmärk.

Mõnel juhul soovite hinnata mudeli ühenduseta enne selle valmistamisel. Ajal ühenduseta hindamiseks ei asenda online hindamiseks, võib olla mõõdiku.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Ühenduseta hindamine  ##

Ühenduseta hindamise eesmärk on prognoosida arvutustäpsuse (kasutajad, kes ostavad ühte soovitatud üksuste arv) ja mitmekesisuse soovituste (arv üksused, mis on soovitatav).
Osana täpsuse ja mitmekesisuse mõõdikute väärtustamise süsteemi leiab valimi kasutajate ja jagab kahe rühma tehingud nende kasutajate jaoks: koolitus andmekomplekti ja testi andmekomplekti.

> [AZURE.NOTE]Ühenduseta mõõdikute kasutamiseks peab teil olema teie kasutamine andmete ajatemplid.
> Aja andmed on vaja tükeldamiseks kasutus õigesti koolitus ja testi andmekomplektide vahel.

> Ka ühenduseta hindamise võib-olla ei anna tulemuste kasutamine väikesed failid. Väärtustamise olema põhjalik, peaks olema vähemalt 1000 kasutus punktide testi andmekomplekti.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Arvutustäpsuse juures k ###
Järgmises tabelis tähistab arvutustäpsuse juures k ühenduseta hindamise väljund.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Protsent |   13,75 | 18.04   | 21 |  24.31 | 26.61
|Kasutajate test |    10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Kasutajate lugeda | 10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Kasutajad ei loeta | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
Eelmise tabeli *k* tähistab soovituste näidatud kliendile. Tabeli sisu on järgmine: "Kui katse ajal on kuvatud ainult ühe soovituse klientidele, ainult 13,75 kasutajate oleks ostnud soovituse." See lause põhinevad eeldusel, et mudel on saanud väljaõppe ostu andmetega. Teine võimalus, et see on, et arvutustäpsuse 1 on 13,75.

Märkate, et nagu kliendile rohkem üksusi, on soovitatav üksuse ostmist kliendi tõenäosuse läheb üles. Eelmise katse jaoks tõenäosuse kahekordistab 26.61 protsenti kui 5 üksused on soovitatav.

#### <a name="percentage"></a>Protsent
Kuvatud on juba vähemalt üks *k* soovitusi kasutajatelt protsent. Protsent jagatakse lugeda kasutajate arv, mis juba vähemalt ühe soovituse kasutajate arv. Vaadake teemat kasutajate, lisateavet lugeda.

#### <a name="users-in-test"></a>Kasutajate test
Selle rea andmed esindavad testi andmekomplekti kasutajate arv.

#### <a name="users-considered"></a>Kasutajate lugeda
Kasutaja peetakse ainult siis, kui süsteemi on soovitatav vähemalt *k* üksuste põhjal loodud kasutamise koolitus andmekomplekti mudel.

#### <a name="users-not-considered"></a>Kasutajad ei loeta
Selle rea andmed esindavad kõik kasutajad ei loeta. Kasutajad ei saanud vähemalt *k* Soovitatavad üksused.

Kasutaja ei loeta = kasutajate test – kasutajad lugeda

<a name="Diversity"></a>
### <a name="diversity"></a>Mitmekesisuse ###
Mitmekesisuse mõõdikute Mõõtke tüüpi üksused on soovitatav. Järgmises tabelis tähistab mitmekesisuse ühenduseta hindamise väljund.

|Protsentiili kopp |    0-90|  90 – 99| 99-100
|------------------|--------|-------|---------
|Protsent        | 34.258 | 55.127| 10.615


Üksused, mis on soovitatav kokku: 100 000

Kordumatud üksused, mis on soovitatav: 954

#### <a name="percentile-buckets"></a>Protsentiili ämbrid
Iga protsentiili ämber esindab on vahemik (miinimum- ja väärtused, mis on vahemikus 0 – 100). 100 lähedale üksused on kõige populaarsemad üksused ja üksuste lähedale 0 on kõige populaarsemate. Näiteks kui protsent kopp 99-100 protsentiili väärtus 10.6, tähendab see, 10.6 protsenti soovitusi tagastatakse ainult ülemine ühe protsenti kõige populaarsemad üksused. Protsentiili kopp vähim väärtus on kaasa arvatud ja maksimumväärtus on välja arvatud, välja arvatud 100.
#### <a name="unique-items-recommended"></a>Soovitatav kordumatut üksust
Kordumatud üksused soovitatav meetermõõdustik kuvatakse erinevate üksuste hindamise tagastatud arv.
#### <a name="total-items-recommended"></a>Soovitatav üksusi kokku
Kokku üksused on soovitatav meetermõõdustik kuvatakse soovitatav üksuste arvu. Mõned duplikaadid võivad olla.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Ühenduseta hindamise mõõdikud ###
Täpsuse ja mitmekesisuse ühenduseta mõõdikute võib olla kasulik, kui valite millised koostamine kasutada. Koosta ajal vastav FBT või soovitus koostamine parameetrid:

-   Seadke parameetri *enableModelingInsights* koostamine väärtuseks **true**.
-   Soovi korral valige *splitterStrategy* (kas *RandomSplitter* või *LastEventSplitter*).
*RandomSplitter* jagab kasutusandmete rongi ja testi määrab antud *randomSplitterParameters* testi protsendi alusel ja juhusliku seeme väärtused.
*LastEventSplitter* jagab kasutusandmete rongi ja testida määrab põhineva viimase tehingu iga kasutaja jaoks.

See käivitab järk, mis kasutab andmete alamhulk koolitus ja ülejäänud andmed kasutab arvutamiseks hindamiseks mõõdikute.  Kui koostamine on lõpule jõudnud, saada väljund hindamise, peate [toomine Koosta mõõdikute API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), läbides vastav *modelId* ja *buildId*helistamiseks.

 Järgmine on näide hindamise JSON.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
