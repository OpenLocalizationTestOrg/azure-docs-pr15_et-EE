<properties 
    pageTitle="Seadme õ rakendus: normaalne tuvastamise teenus | Microsoft Azure'i" 
    description="Normaalne tuvastamise API on näiteks ehitatud Microsoft Azure'i masina õ tuvastab kõrvalekaldeid aja sarja andmete arvväärtused, mis on ühtlaselt vahedega ajal." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Seadme õ normaalne tuvastamise teenus#

##<a name="overview"></a>Ülevaade

[Normaalne tuvastamise API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) on näiteks ehitatud Azure seadme õ, mis tuvastab kõrvalekaldeid aja sarja andmete arvväärtused, mis on ühtlaselt vahedega ajal. 

See API on tuvastanud järgmist tüüpi Anomaalne mustrite aja sarja andmete:

* **Positiivne ja negatiivne trendide**: näiteks, kui jälgimise mälukasutuse kasvutrendi arvutuste võib huvi nagu see võib olla põhjus,

* **Dünaamilise vahemiku väärtuste muutused**: näiteks visanud pilveteenus erandid kontrollimisel dünaamilise vahemiku väärtuste muudatused võib viidata ebastabiilsuse teenuse seisundi ja

* **Naelu ja Dips**: näiteks login arvu mõnes muus teenuses või arvu kassades pood saidi kontrollimisel diagrammi või dips võib viidata ettenägematut käitumine.

Need seadme õ detektorid selliste muutuste jälitus väärtused üle nende väärtuste aja ja aruande muutuste nimega normaalne hinded. Nad ei nõua adhoc lävi häälestamine ja oma hinded saab kasutada tulemuste määra. Normaalne avastamine API on kasulik mitu stsenaariumid, nagu teenuse jälgimine aja jooksul, KPI-d jälgides kasutuse jälgimise kaudu mõõdikute nagu otsingute arv, arvude hiireklõpsuga, jõudluse jälgimise kaudu hinnale mälu, CPU, nt faili loeb jne aja jooksul.

Normaalne tuvastamise pakkumise on töö alustamiseks kasulikke tööriistu. 

* [Veebirakenduse](http://anomalydetection-aml.azurewebsites.net/) aitab teil hinnata ja visualiseerida tulemuste normaalne tuvastamise API-de oma andmete põhjal. 

* [Proovi kood](http://adresultparser.codeplex.com/) näitab, kuidas programmiliselt juurde API ja sõeluda C# tulemused.

>[AZURE.NOTE]
>Proovige **IT normaalne ülevaateid lahendus** [selle API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) tootja
>
>Saada see lõpuni lahendus juurutatud Azure tellimuse <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **alustage siit >**</a>


##<a name="api-definition"></a>API määratlus

Teenus pakub ülejäänud vastavalt API https, saate tarbitud erinevatel viisidel, sh veebis või mobiilirakenduse, R, Python, Excel, jne.  Selle teenuse kaudu REST API kõne ajal sarja andmete saata, ja see töötab kombinatsiooni kolme normaalne ülalkirjeldatud tüübiga. Teenus töötab Azure seadme õ platvorm, mis teie ettevõtte vajadustele skaala sujuvalt ja pakub SLAs.

###<a name="headers"></a>Päised
Tagada õige päiste kaasamine teie taotlus, mis peaks olema järgmised:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Teie konto teie konto võti leiate [Azure'i Data Marketi](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>Keskmine API

Keskmine API kasutatakse normaalne tuvastamise töötavate-hooajaline kellaajaandmeid sarja. API käivitatakse arv normaalne detektorid andmed ja tagastab nende normaalne hinded. Joonis kujutab näidet kõrvalekaldeid, mis tuvastab keskmine API-ga. Selle sarja aeg on 2 erinevate saiditasemel muudatusi ja 3 diagrammi. Punane punkti Kuva aeg, kus avastatakse ajal musta punkti Kuva tuvastati diagrammi taseme muutmine.

![Keskmine API][1]
    
**URL-I**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Näide taotluse keha**

Allpool taotluse saadame aeg sarja (kuvatud kärbitud) koos andmepunktide: 3/1/2016 kuni 3/10/2016 ja parameetrite kühvli detektorid API tuvastamiseks, kui mõni järgmised andmepunktide on Anomaalne. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Reageerimine on järgmised: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

ScoreWithSeasonality API kasutatakse normaalne tuvastamise töötavate kellaaja sari, mis on hooajalised mudelid. See API on kasulik hälvete hooajaline mustrite tuvastamiseks.  

Järgmisel joonisel on näide kõrvalekaldeid sarja hooajaline aega tuvastada. Sarja aeg on üks kühvli (1st musta punkti), kaks dips (2 musta punkti ja üks lõpus) ja ühe taseme muutmine (punane dot). Pange tähele, et dip keskel aja sari ja taseme muutmine ainult tajutavatest pärast hooajaline eemaldatakse sarja.

![Hooajaliste API][2]

**URL-I**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Näide taotluse keha**

Allpool taotluse saadame API tuvastamiseks, kui mõni järgmised andmepunktide on Anomaalne aja sarja (kuvatud kärbitud) koos andmepunktide: 3/1/2016 kuni 3/10/2016 ja parameetrid. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Reageerimine on järgmised: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Detektorid

Normaalne tuvastamise API toetab detektorid 3 ülesandekategooriat. Teatud sisendparameetrite ja iga detektor väljundeid üksikasjad leiate järgmises tabelis.

|Detektor kategooria|Detektor|Kirjeldus|Parameetrid|Väljundid
|---|---|---|---|---|
|Kühvli detektorid|TSpike detektor|Diagrammi ja palju väärtuste põhjal dips on esimene ja kolmas Kvartiile tuvastamine|*tspikedetector.sensitivity:* võtab täisarv vahemikus 1 – 10, vaikimisi: 3; Suuremad väärtused on veel äärmiselt väärtusi, mistõttu on vähem tundlik jaole juba|TSpike: kahendarvu väärtused – 1, kui kühvli/suplust tuvastatakse, 0 muul viisil|
||ZSpike detektor|Diagrammi ja kui palju on andmepunktid on keskväärtuse põhjal dips tuvastamine|*zspikedetector.sensitivity:* võtta täisarv vahemikus 1 – 10, vaikimisi: 3; Suuremad väärtused on veel äärmiselt väärtusi, mistõttu on vähem tundlik jaole juba|ZSpike: kahendarvu väärtused – 1, kui kühvli/suplust avastatakse, 0 muul viisil|
|Trend detektor aeglane|Trend detektor aeglane|Tuvasta aeglane positiivne trend määramine tundlikkuse kohta|*trenddetector.sensitivity:* piirmäära detektor Keskmine (vaikimisi: 3,25, 3,25 – 5 on mõistlik valik selle kaudu; Mida suurem vähem tundlik)|TScore: ujuv arvu tähistav normaalne Keskmine trend|
|Detektorid taseme muutmine|Detektor Ühesuunalised taseme muutmine|Tuvasta tõusev tase muuta ühe määramine tundlikkuse|*upleveldetector.sensitivity:* piirmäära detektor Keskmine (vaikimisi: 3,25, 3,25 – 5 on mõistlik valik selle kaudu; Mida suurem vähem tundlik)|PScore: ujuv normaalne Keskmine ülespoole kohta tähistav arv tase muutmine|
||Kahesuunaline taseme muutmine detektor|Tuvasta üles- ja allapoole ühe määramine tundlikkuse taseme muutmine|*bileveldetector.sensitivity:* piirmäära detektor Keskmine (vaikimisi: 3,25, 3,25 – 5 on mõistlik valik selle kaudu; Mida suurem vähem tundlik)|RPScore: ujuv arv esindavad normaalne Keskmine ülespoole ja allapoole tasemel muutmine

###<a name="parameters"></a>Parameetrid

Järgmises tabelis on loetletud täpsemat teavet nende parameetrid:

|Parameetrid|Kirjeldus|Vaikesäte|Tüüp|Lubatud vahemikku|Soovitatud vahemikus|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Koondamine intervall sekundites liitmise sisestusmeetodi aja sarja|0 (ei ole liita)|täisarv|0: vahele jätta koondamine > 0 teisiti|5 minutit 1 päeva, kellaaja-sarja sõltuv
|preprocess.aggregationFunc|Funktsiooni kasutatakse andmete liitmise sisse määratud AggregationInterval|keskmise.|loetletud|keskmise, summa, pikkus|N/A|
|preprocess.replaceMissing|Puuduvad andmed soetuse kasutatavaid väärtusi|LKV (teadaolev väärtus)|loetletud|null, lkv, keskmise.|N/A|
|detectors.historyWindow|Ajalugu (# andmepunktide) normaalne Keskmine arvutamiseks kasutada|500|täisarv|10 – 2000|Aja-sarja sõltuv|
|upleveldetector.sensitivity|Detektori tundlikkuse ülespoole taseme muutmine |3,25|kahekordne|Ükski|3,25-5(Lesser values mean more sensitive)|
|bileveldetector.sensitivity|Kahesuunaline taseme tundlikkuse muuta detektor. |3,25|kahekordne|Ükski|3,25-5(Lesser values mean more sensitive)|
|trenddetector.sensitivity|Positiivse trendi detektori tundlikkus. |3,25|kahekordne|Ükski|3,25-5(Lesser values mean more sensitive)|
|tspikedetector.sensitivity |Tundlikkus TSpike detektor|3|täisarv|1-10|3-5(Lesser values mean more sensitive)|
|zspikedetector.sensitivity |Tundlikkus ZSpike detektor|3|täisarv|1-10 |3-5(Lesser values mean more sensitive)|
|seasonality.Enable|Kas hooajalisus analüüsi on teostatav|True|kahendmuutuja|True, false|Aja-sarja sõltuv|
|seasonality.numSeasonality |Maksimumarv perioodiliste tsüklit tuvastamine|1|täisarv|1, 2|1-2|
|seasonality.Transform |Kas hooajaline (ja) trendi komponendid kõrvaldatakse enne rakendamist normaalne automaattuvastus|deseason|loetletud|ükski, deseason deseasontrend|N/A|
|postprocess.tailRows |Säilitatakse väljundi tulemuste uusima andmepunktide arv|0|täisarv|0 (kõik andmepunktide jätta), või määrata tulemuste hoida punktide arv|N/A|

###<a name="output"></a>Väljund
API käivitatakse kõik andurid aja sarja andmete ja tagastab normaalne hinded ja kahendarvu kühvli näidikud iga aeg. Järgmises tabelis on loetletud väljundeid API. 

|Väljundid|Kirjeldus|
|---|---|
|Aeg|Ajatemplid toorandmetega või liidetud (või) arvestuslike andmete kui koondamine (või) puuduvad on rakendatud andmete arvestamine|
|OriginalData|Kui toorandmetega või liidetud (või) arvestuslike andmete väärtused koondamine (või) puuduvad rakendatakse andmete arvestamine|
|ProcessedData|Ükskõik kumba järgmistest: <ul><li>Hooajaliste kohandatud aja sarja kui märgatavat hooajalisus on avastatud ja deseason märgituks;</li><li>hooajaliste kohandada ja detrended aja sari, kui märgatavat hooajalisus tuvastatakse ja deseasontrend valitud suvand</li><li>muul juhul see on sama, mis OriginalData</li>|
|TSpike|Binaarsed tähis, mis näitab, kas on kühvli tuvastatakse TSpike detektor|
|ZSpike|Binaarsed tähis, mis näitab, kas on kühvli tuvastatakse ZSpike detektor|
|Pscore|Ujuv normaalne Keskmine ülespoole tasemel tähistav arv muutmine|
|Palert|1/0 väärtusega, mis näitab, on tõusev muutmine normaalne Sisestuskeel tundlikkuse põhjal|
|RPScore|Ujuv arv esindavad normaalne Keskmine kahesuunaline taseme muutmisel|
|RPAlert|1/0 väärtusega, mis näitab, on kahesuunaline taseme muutmine normaalne Sisestuskeel tundlikkuse põhjal|
|TScore|Ujuv arv esindavad normaalne Keskmine positiivne trend|
|TAlert|1/0 väärtus, mis näitab, et on positiivne trend normaalne, mis põhineb Sisestuskeel tundlikkuse|


Selle väljundi saab sõeluda, kasutades [lihtsat parseri](https://adresultparser.codeplex.com/) – see on proovi kood, mis näitab, kuidas ühendada API ja sõeluda väljund. Tuvastatud kõrvalekaldeid saate armatuurlaual visualiseerimise ja/või inimeste ekspertide korrektiivläätsed toimingute jaoks edasi või integreeritud piletimüügi süsteemid.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
