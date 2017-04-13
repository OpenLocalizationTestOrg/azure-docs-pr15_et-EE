<properties 
    pageTitle="Lineaarse regressioonisirge kasutamine arvuti õ | Microsoft Azure'i" 
    description="Lineaarse regressioonisirge mudelite Excelis ja Azure seadme õ Studios võrdlus" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Azure'i masina õ lineaarse regressioonisirge kasutamine

> *Kate Baroni* ja *Ben paadimees* on ettevõtte lahenduse arhitektiõppega Microsofti andmete ülevaateid tipptasemel keskele. Selles artiklis kirjeldatakse need oma kogemusi migreerimine mõne olemasoleva regressiooni analüüsi komplekti kasutades Azure seadme õ pilvepõhist lahenduse.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Eesmärk

Meie projekti kaks eesmärke silmas pidades alustamiseks tehke järgmist.  

1. Meie ettevõtte igakuine tulu prognooside täpsuse suurendamiseks ennustav abil  
2. Azure'i ML abil saate kinnitada, optimeerimine, kiirus, suurendada ja mastaabi meie tulemused.  

Paljud ettevõtted, nt läbib meie ettevõtte igakuine tulude prognoosi koostamiseks protsess. Meie väike meeskond ärianalüütikud ülesanne oli arvutisse õ abil toetada ja parandada prognoosi koostamise täpsust.  Meeskonna kulunud mitu kuud mitmest allikast andmete kogumine ja andmete atribuute läbiv statistilise analüüsi tuvastamise tootenumbri atribuutide oluline teenuste müügi prognoosi koostamiseks.  Järgmised toimingud on alustamiseks prototüüpimine statistika regressiooni mudelites Exceli andmed.  Mõne nädala jooksul oli meil Exceli regressiooni mudelit, mis on outperforming praeguselt väljalt ja protsesside prognoosimise finance. See sai võrdlusalus ennustamine tulemi.  


Me siis võttis järgmise juhise juurde meie ennustav üle liikumisel Azure'i ml teada, kuidas Azure'i ML võib sõnastikupõhise jõudlust parandada.


## <a name="achieving-predictive-performance-parity"></a>Sõnastikupõhise jõudluse võrdse saavutamiseks

Meie esimene prioriteet oli võrdse Azure'i ML ja Exceli regressiooni mudelite vahel.  Anda täpselt samad andmed ja sama tükeldatud koolitus ja testimine me soovivad saavutada sõnastikupõhise jõudluse võrdse Exceli ja Azure ML andmed.   Algselt me ei saanud. Exceli andmemudeli edestas Azure'i ML mudel.   Selle põhjuseks on mõistmise alus tööriista säte Azure'i ml puudumine. Pärast Azure ML toote meeskonnatöö sünkroonida, saadud säte meie andmekogumi jaoks vajaliku alus paremini mõista, ja saavutada võrdse kahe mudeli vahel.  

### <a name="create-regression-model-in-excel"></a>Regressioonisirge andmemudeli loomine Excelis
Meie Exceli regressiooni kasutada Exceli analüüsi tööriistapakett leitud standard lineaarse regressioonisirge mudeli. 

Me arvutatud *keskmine absoluutne % tõrge* ja kasutada seda mudeli jõudluse mõõt.  Kulus 3 kuud jõuda mudeli töötamine Exceli abil.  Oleme toonud palju õ Azure'i ML katse, mis oli lõpuks kasulik mõistmine nõuded sisse.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Azure'i masina õ sarnastes katse loomine  
Me jälgitavad loomiseks meie katse Azure'i ml järgmiselt:  

1.  Üles laaditud andmekomplekti Azure'i ml (väga väike-faili) csv-failina
2.  Luuakse uus katse ja kasutatakse [Andmekomplekti veergude valimine] [ select-columns] mooduli valimine Excelis kasutatavad funktsioonid samad andmed   
3.  [Tükeldatud andmetega] [ split] moodul ( *Suhteline avaldise* režiim) koos täpselt sama rongi komplekti andmeid jagada, nagu on tehtud Exceli  
4.  [Lineaarse regressioonisirge] proovinud[ linear-regression] moodul (vaikimisi valik ainult), dokumenteerida ja võrrelda tulemusi meie Exceli regressiooni mudeli kasutuselevõtt

### <a name="review-initial-results"></a>Algne tulemuste läbivaatus
Esialgu Exceli andmemudeli selgelt edestas Azure'i ML mudeli:  

|   |Exceli|Azure'i ML|
|---|:---:|:---:|
|Jõudluse|   |  |
|<ul style="list-style-type: none;"><li>Kohandatud R-ruudu</li></ul>| 0,96 |N/A|
|<ul style="list-style-type: none;"><li>Ruudu. <br />Määramine</li></ul>|N/A|   0,78<br />(madal täpsus)|
|Absoluutne tõrge tähendab |  $9. 5M|  $ 19,4 M|
|Tähendab absoluutne tõrge (%)|   6,03%|  12,2%

Kui soovime parandusfunktsiooni meie protsessi ja tulemuste arendajatele ja andmeteadlaste Azure ML meeskond, nad kiiresti andnud kasulikke näpunäiteid.  

* Kui kasutate [Lineaarse regressioonisirge] [ linear-regression] mooduli Azure'i ml, pakutakse kaks võimalust:
    *  Veebis astmik päritolu: Sobida paremini suuremas ulatuses probleemid
    *  Tavaline vähimruutude: See on enamik inimesi mõtlema kui nad kuule lineaarse regressioonisirge meetod. For small andmekomplektide, saab tavaliste vähimruutude optimaalne valik.
*  Kaaluge tutistamine L2 reguleerimiseks pärast paksus parameetri jõudluse parandamiseks. See on seatud 0,001 vaikimisi ja meie väike andmehulga, saame määraks 0,005 jõudluse parandamiseks.    

### <a name="mystery-solved"></a>Salajase lahendatud!
Kui oleme rakendanud soovitusi, me saavutada sama võrdlusalus jõudlus Azure'i ml nimega Exceliga:   

|| Exceli|Azure'i ML (esialgne)|Azure'i ML w / vähimruutude|
|---|:---:|:---:|:---:|
|Siltidega väärtuse  |Tegelikud (arv)|sama|sama|
|Õppurite  |Exceli -> andmete analüüsi -> regressiooni|Lineaarse regressioonisirge.|Lineaarregressioon|
|Õppurite suvandid|N/A|Vaikesätteid.|tavaline vähimruutude<br />L2 = 0,005|
|Andmehulgas.|26 ridu, 3 funktsioone, 1 silt.   Kõik arv.|sama|sama|
|Tükeldatud: Train|Exceli koolitada esmalt 18 ridadel katsetada viimati 8 rida.|sama|sama|
|Tükeldatud: Test|Exceli regressiooni valemi viimati 8 ridadele rakendatud|sama|sama|
|**Jõudluse**||||
|Kohandatud R-ruudu|0,96|N/A||
|Kompleksarvu määramine|N/A|0,78|0.952049|
|Absoluutne tõrge tähendab |$9. 5M|$ 19,4 M|$9. 5M|
|Tähendab absoluutne tõrge (%)|<span style="background-color: 00FF00;">6,03%</span>|12,2%|<span style="background-color: 00FF00;">6,03%</span>|

Lisaks võrreldes kordajad Excel ka funktsiooni kaalu Azure koolitatud mudeli:

||Exceli kordajad|Azure'i funktsiooni kaalu|
|---|:---:|:---:|
|Intercept/hälvete|19470209.88|19328500|
|Funktsioon A|0.832653063|0.834156|
|Funktsioon B|11071967.08|11007300|
|Funktsioon C|25383318.09|25140800|

## <a name="next-steps"></a>Järgmised sammud

Me kalendriüksusi kasutamine Azure ML veebiteenuse Excelist.  Meie ärianalüütikud toetuvad Exceli ja viis kõne Azure'i ML veebiteenuse Exceli andmeid sisaldav rida ja see tagastada Exceli tulevane väärtus oleks vaja.   

Me ka kalendriüksusi meie andmemudeli optimeerida suvandid ja algoritmide kohta saadaval Azure ML abil.

### <a name="integration-with-excel"></a>Exceli integreerimine
Meie lahendus oli kiireks meie Azure'i ML regressiooni mudel, luues veebiteenuse koolitatud mudel.  Paari minutiga veebiteenuse on loodud ja saanud kutsume otse Excelist prognoositud tulu väärtuse.    

*Web Services armatuurlaua* jaotis sisaldab allalaaditavad Exceli töövihik.  Töövihik sisaldab eelvormindatud web teenuse API- ja -skeemifailid teave manustatud.   *Allalaadimine Exceli töövihiku*klõpsamisel avatakse see ja saate seda kohalikku arvutisse salvestada.    

![][1]
 
Avage töövihik, kus kopeerige oma eelmääratletud parameetrite Sinine parameeter jaotises nagu allpool näidatud.  Kui parameetrid on sisestatud, Exceli nõuab tähelepanu AzureML veebiteenusele ja selle prognoositud viskas kuvatakse roheline ennustatud väärtuste jaotises Sildid.  Töövihik on jätkuvalt prognoose parameetrite põhjal koolitatud mudelisse sisestatud parameetritega kõigi rea üksuste loomine.   Selle funktsiooni kasutamise kohta leiate lisateavet teemast [tarbimine Azure seadme õ Web teenuse lisamine Exceli failist](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimeerimine ja täpsemaks katsed
Nüüd, kui meil oleks võrdlusalus meie Exceli mudel, oleme liikunud edasi optimeerida meie Azure'i ML lineaarse regressioonisirge mudel.  Kasutasime mooduli [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] parandada lähteandmete valik elemente ja see aitas saavutada jõudluse parandamiseks 4,6% absoluutne tõrge tähendab.   Tulevaste projektide jaoks kasutame seda funktsiooni, mis võib salvestada meile nädalat iterating andmete atribuutide kogum, funktsioonide kasutamiseks modelleerimine leidmiseks.  

Järgmine plaanis lisada täiendavad algoritmide nagu [Bayesi] [ bayesian-linear-regression] või [Võimendada otsust puude] [ boosted-decision-tree-regression] meie katse võrdlemiseks jõudlust.    

Kui soovite katsetada regressiooni, hea andmekomplekti proovida on kujutatud tõhusust regressiooni valimi andmekomplekti, mis on palju arvväärtuste atribuute. Andmekomplekti on esitatud valimi andmekomplektide ML Studios osana.  Saate mitmesuguste Õppekeskuse moodulid prognoosida Küte laadi või jahutus laadi.  Diagrammi allpool on jõudluse võrdlus erinevate regressiooni õpib kujutatud tõhusust andmekomplekti prognoosimine target muutuja jahutus laadi suhtes. 

|Mudel|Absoluutne tõrge tähendab|Root keskmise ruudu tõrge|Suhteline absoluutne tõrge|Suhteline ruudu tõrge|Kompleksarvu määramine
|---|---|---|---|---|---
|Võimendatud Otsusepuu kuvamine|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineaarse regressioonisirge (astmik päritolu)|2.035693|2.98006|0.233414|0.094881|0.905119
|Närvivõrgus regressiooni|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineaarse regressioonisirge (tavaline vähimruutude)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Võtme Takeaways 

Oleme palju õppinud töötava Exceli regressiooni ja Azure seadme õ katsete paralleelselt. Excelis võrdlusalus mudeli loomise ja võrrelda seda mudelite abil Azure'i ML [Lineaarse regressioonisirge] [ linear-regression] aitas meile siit Azure'i ML ja me avastanud võimalusi andmete valiku ja mudeli jõudluse parandamiseks.         

Leidsime ka, on soovitatav kasutada [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] ennustamine tulevaste projektide kiirendamiseks.  Rakendades funktsioonide valik andmete, saate luua mudelit, mis on täiustatud Azure'i ml koos parema üldise jõudluse. 

Võimalus edastamine sõnastikupõhise analüütiline prognoosimine Azure'i ml Exceli süsteemselt võimaldab olulist kasvu võimalus edukalt otsingutulemusi laialdane business kasutaja publikule.     


## <a name="resources"></a>Ressursid
Saate töötada regressiooni aidates on loetletud mõned ressursid:  

* Regressioonisirge Excelis.  Kui olete proovinud kunagi Exceli regressiooni, selles õpetuses on lihtne: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regressioonisirge vs prognoosi koostamiseks.  Tyler Chessman kirjutas ajaveebi artikkel selgitab, kuidas aega sarja Excelis, mis sisaldab hea algajale kirjeldus lineaarse regressioonisirge prognoosi koostamiseks. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Tavaline vähimruutude lineaarse regressioonisirge: Vigu, probleemide ja probleeme.  Sissejuhatus ja regressiooni arutelu: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
