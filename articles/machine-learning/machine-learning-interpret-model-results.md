<properties
    pageTitle="Mudeli tulemusi kohapeal õ tõlgendamine | Microsoft Azure'i"
    description="Kuidas valida optimaalse parameetri algoritmi abil ja visualiseerimine Keskmine mudeli väljundeid seadmine."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Azure'i masina õ mudeli tulemuste tõlgendamine

See teema selgitab, kuidas visualiseerimine ja ennustamine tulemuste Azure seadme õ Studios tõlgendamine. Pärast väljaõppe mudeli ja valmis prognoose peal ("viskas mudeli"), peate ennustamine tulemi küsimustes.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

On neli põhilist liiki seadme õppe mudelid Azure seadme õppida.

* Liigitamine
* Rühmitamise
* Regressioonisirge
* Soovitaja süsteemid

Kasutatakse ennustamine peal need mudelid moodulid on:

* [Keskmine mudel] [ score-model] mooduli liigitamiseks ja regressiooni
* [Kogumite määramine] [ assign-to-clusters] mooduli rühmitamiseks
* [Keskmine tikutoos soovitaja] [ score-matchbox-recommender] soovitus süsteemid

Selles dokumendis selgitatakse, kuidas tõlgendada ennustamine tulemused kõik need moodulid. Ülevaate nende moodulid, vaadake, [kuidas optimeerida oma algoritmide Azure seadme õppe parameetrite valimiseks](machine-learning-algorithm-parameters-optimize.md).

Selles teemas käsitletakse ennustamine tõlgendamise, kuid mitte mudeli hindamise. Lisateavet selle kohta, kuidas hinnata mudeli kohta leiate teemast [hindamaks, kui mudeli jõudluse Azure seadme õ](machine-learning-evaluate-model-performance.md).

Kui olete uue Azure seadme õ ning kas vajate abi luua lihtsa katse alustada, lugege teemat [loomine lihtsa katse Azure seadme õ Studios](machine-learning-create-experiment.md) Azure seadme õ Studios.

## <a name="classification"></a>Liigitamine ##
On kaks alamkategooriate liigitamine probleeme.

* Probleemid ainult kaks klassi (kaks klassi või kahendarvu liigitus)
* Rohkem kui kaks klassi (mitme klassi liigitamine) probleemid

Azure'i masina õ on moodulist tegelema iga seda tüüpi liigitamine, kuid meetodid tõlgendamisel nende ennustamine on sarnased.

### <a name="two-class-classification"></a>Kaks klassi liigitamine###
**Näide katse**

Näiteks kahe-klassi liigitamine probleem on iris lilledest liigitus. Tööülesanne on iris lilledest nende funktsioonide alusel liigitada. Iris andmehulga esitatud Azure seadme õppe on populaarsed [Iris andmehulga](http://en.wikipedia.org/wiki/Iris_flower_data_set) alamhulk, mis sisaldab eksemplari ainult kaks Lille liiki (tunnid 0 ja 1). On neli funktsioonid iga lill (õietupp pikkus, õietupp laiuse, kroonleht pikkus ja kroonleht laius).

![Kuvatõmmis iris katse](./media/machine-learning-interpret-model-results/1.png)

Joonis 1. Iris kaks klassi liigitamine probleemi katse

Katse on tehtud probleemi lahendamiseks, nagu on näidatud joonisel 1. Kaks klassi võimendatud otsust puu mudeli on väljaõppe ja viskas. Nüüd saate visualiseerida, [Keskmine mudeli] ennustamine tulemusi[ score-model] mooduli väljundi pordi [Keskmine mudelit] klõpsates[ score-model] mooduli ja klõpsates siis käsku **Visualiseeri**.

![Keskmine mudeli mooduli](./media/machine-learning-interpret-model-results/1_1.png)

Kuvatakse tabeliveergude hinded tulemused, nagu on näidatud joonisel 2.

![Iris kaks klassi liigitamine katse tulemused](./media/machine-learning-interpret-model-results/2.png)

Joonis 2. Visualiseerimine Keskmine mudeli tulemuseks kaks klassi liigitamine

**Tulemuse tõlgendamine**

Tulemuste tabelis on kuus veergu. Vasakul neli veergu on neli funktsioonid. Parem kaks veergu, viskas sildid ja viskas tõenäosus, on ennustamine tulemused. Viskas tõenäosus veerus kuvatakse tõenäosuse lill kuuluva positiivne ainekursuse (1 tund). Esimene veerg (0.028571) tähendab, et number on näiteks 0.028571 tõenäosuse, et esimene lill kuulub 1 tund. Siltide viskas veerus kuvatakse prognoositud tunni jaoks iga lill. See on tõenäosus viskas veeru alusel. Kui lill poolitusjoonega tõenäosus on suurem kui 0,5, on ennustatud 1 tund. Muul juhul on ennustatud klassi 0.

**Veebis teenuse avaldamine**

Pärast ennustamine tulemused mõistma ja hinnatakse heli, saate katse nimega veebiteenuse avaldada nii, et saate juurutada erinevates rakendustes ja kõne selle saada klassi prognoose mis tahes uus iris lill. Saate teada, kuidas muuta koolitus katse hinded katse ja avaldada mis veebiteenuse, lugege teemat [Avalda Azure seadme õ veebiteenus](machine-learning-walkthrough-5-publish-web-service.md). Selle toimingu pakub teile hinded katse, nagu on näidatud joonisel 3.

![Kuvatõmmis hinded katse](./media/machine-learning-interpret-model-results/3.png)

Joonis 3. Hinded iris kaks klassi liigitamine probleemi katse

Nüüd peate määrama sisestus- ja väljundi veebiteenuse. On õige Sisestuskeel pordi [Keskmine]mudeli[score-model], mis on Iris lill funktsioonid sisestatud. Väljund valik sõltub sellest, kas olete huvitatud prognoositud ainekursuse (poolitusjoonega sildi), poolitusjoonega tõenäosuse või mõlemad. Selles näites eeldatakse, et olete huvitatud nii. Soovitud väljund veergude valimiseks kasutage [Andmehulga veerud valige] [ select-columns] mooduli. Klõpsake nuppu [Vali veerud andmehulga][select-columns], klõpsake nuppu **Käivita veeru Vaateselektori**ja valige **Viskas sildid** ja **Viskas tõenäosus**. Pärast seadmine [Valige] veergude andmehulga pordi väljundi[ select-columns] ja uuesti käivitada, peaksite olema avaldada hinded katse nimega veebiteenuse, klõpsates nuppu **AVALDA VEEBITEENUS**. Lõplik katse näeb joonis 4.

![Iris kaks klassi liigitamine katse](./media/machine-learning-interpret-model-results/4.png)

Joonis 4. Lõplik hinded katse, et probleem on kaks klassi liigitamine iris

Kui käivitate veebiteenuse ja sisestage mõne funktsiooni väärtuste kontrollimiseks, tagastab tulemi kahe arvu. Esimene number on poolitusjoonega silt ja teine on poolitusjoonega tõenäosus. See lill on ennustatud 0.9655 tõenäosus on 1 tund.

![Testige suulise tõlke Keskmine mudel](./media/machine-learning-interpret-model-results/4_1.png)

![Hinded testi tulemused](./media/machine-learning-interpret-model-results/5.png)

Joonis 5. Web teenuse tulemus iris kaks klassi liigitamine

### <a name="multi-class-classification"></a>Mitme klassi liigitamine
**Näide katse**

See katse sooritamist tähe-tuvastus tööülesande multiclass liigitamine näide. Klassifitseerijale püüab prognoosida teatud tähe (klass) mõned ekstraktimist käsitsi kirjutatud pildid käsitsi kirjutatud atribuudi väärtuste põhjal.

![Täht tuvastus näide](./media/machine-learning-interpret-model-results/5_1.png)

Koolitus andmeid, on 16 funktsioonid ekstraktimist käsitsi kirjutatud kirja pildid. 26 tähed vormi meie 26 tunnid. Joonis 6 kuvatakse mis koolitada multiclass liigitamine mudeli tähe tunnustamiseks ja prognoosida katse sama funktsioon pannud testi andmehulgas.

![Täht tuvastus multiclass liigitamine katse](./media/machine-learning-interpret-model-results/6.png)

Joonis 6. Täht tuvastus multiclass liigitamine probleemi katse

[Keskmine mudeli] tulemusi visualiseerimine[ score-model] mooduli, klõpsates nuppu väljund porti [Keskmine] mudeli[ score-model] mooduli ja klõpsates siis käsku **Visualiseeri**, kuvatakse sisu, nagu on näidatud joonisel 7.

![Keskmine mudeli tulemused](./media/machine-learning-interpret-model-results/7.png)

Joonis 7. Visualiseerimine Keskmine mudeli tulemusi mitme klassi liigitamine

**Tulemuse tõlgendamine**

Vasakul 16 veerud esindavad funktsiooni väärtuste testi määramine. Veergude nimed nagu viskas tõenäosust Class "XX" on lihtsalt nagu kaks klassi juhul veeru viskas tõenäosus. Sektordiagrammis kuvatakse tõenäosuse vastava kirje vahemikku teatud klassi. Näiteks esimese kirje on 0.003571 tõenäosuse, et see on "A," 0.000451 tõenäosuse, et see on "B" ja jne. Viimane veerg (viskas sildid) on sama siltidena viskas kaks klassi juhul. Klassi suurim poolitusjoonega tõenäosus on prognoositud klassi vastav kirje valib. Näiteks esimese kirje poolitusjoonega silt on "F" Kuna see on suurim tõenäosust, et "F" (0.916995).

**Veebis teenuse avaldamine**

Samuti saate poolitusjoonega sildi iga kirje ja poolitusjoonega sildi tõenäosuse. Elementaarne loogika on leida suurima tõenäosuse vahel poolitusjoonega tõenäosus. Selle tegemiseks peate kasutama [Käivitada R skripti] [ execute-r-script] mooduli. R kood on näidatud joonisel 8 ja katse tulemus on näidatud joonisel 9.

![R koodi näide](./media/machine-learning-interpret-model-results/8.png)

8-kujund. R koodi ekstraktimiseks viskas sildid ja siltide seotud tõenäosus.

![Katse tulemi](./media/machine-learning-interpret-model-results/9.png)

Joonis 9. Lõplik hinded katse probleemi tähe-tuvastus multiclass liigitamine

Pärast avaldamine ja käivitate veebiteenuse ja sisestage mõned Sisestuskeel funktsiooni väärtused tagastatud tulemus näeb joonis 10. Selle käsitsi kirjutatud kirja oma ekstraktitud 16 funktsioone, on ennustatud olema "T" 0.9715 tõenäosus on.

![Suulise tõlke Keskmine mooduli testimine](./media/machine-learning-interpret-model-results/9_1.png)

![Testi tulemus](./media/machine-learning-interpret-model-results/10.png)

Joonis 10. Web teenuse tulemus multiclass liigitamine

## <a name="regression"></a>Regressioonisirge

Regressioonisirge probleeme erinevad liigitamine probleeme. Liigitamine probleem, mida proovite prognoosida eraldi tunnid, nt mis kuulub klassi on iris lill. Kuid nagu näha Järgnevas näites regressiooni probleem, mida proovite prognoosida pidev muutuja, nt hind auto.

**Näide katse**

Kasutada oma näitega auto hind ennustamine regressiooni. Üritate prognoosida vastavalt oma funktsioone, sealhulgas muuta, kütusetüübi, keha tüüp ja ketas ratta auto hind. Katse on näidatud joonisel 11.

![Auto hind regressiooni katse](./media/machine-learning-interpret-model-results/11.png)

Joonis 11. Auto hind regressiooni probleemi katse

[Keskmine mudeli] visualiseerimine[ score-model] mooduli, joonis 12 näeb tulem.

![Hinded tulemuste auto hind ennustamine probleem](./media/machine-learning-interpret-model-results/12.png)

Joonis 12. Hinded auto hind ennustamine probleemi tulem

**Tulemuse tõlgendamine**

Poolitusjoonega sildid on tulemus veeru tulem hinded. Arvud on prognoositud hinna iga auto.

**Veebis teenuse avaldamine**

Saate avaldada regressiooni katse veebiteenuse sisse ja kõne auto hind ennustamiseks samamoodi nagu kaks klassi liigitamine kasutamine.

![Hinded katse auto hind regressiooni probleem](./media/machine-learning-interpret-model-results/13.png)

Joonis 13. Hinded katse mõne auto hind regressiooni probleem

Operatsioonisüsteemi veebiteenuse, tagastatud tulem sarnaneb joonis 14. Prognoositud auto hind on $15,085.52.

![Suulise tõlke hinded mooduli testimine](./media/machine-learning-interpret-model-results/13_1.png)

![Hinded mooduli tulemused](./media/machine-learning-interpret-model-results/14.png)

Joonis 14. Web teenuse tulemus on auto hind regressiooni probleem

## <a name="clustering"></a>Rühmitamise

**Näide katse**

Kasutame Iris andmehulga uuesti koostamiseks klastrite katse. Siin saate filtreerida andmehulga klassi sildid, et see ainult on funktsioonid ja seda saab kasutada rühmitamiseks. See iris kasutusmall, saate määrata kogumite olevat kaks käigus koolitus, mis tähendab, et teil oleks klaster ning lilledest liigitada kahte arvu. Katse on näidatud joonisel 15.

![Iris klastrite probleemi katsetamiseks](./media/machine-learning-interpret-model-results/15.png)

Joonis 15. Iris klastrite probleemi katsetamiseks

Rühmitamise erineb liigitamine, et koolitus andmehulk ei sisalda maa-etalonina sildid ise. Rühmitamise sisse erinevate kogumite rühmade koolitus andmehulga eksemplarid. Koolitus käigus mudeli märgib kirjeid õppida nende funktsioonide kasutamise erinevused. Pärast seda, saab koolitatud mudeli täpsemaks liigitada tulevaste kirjed. On kaks osa tulemi oleme huvitatud klastrite probleemi sees. Esimene osa on siltideta koolitus andmehulga ja teine on liigitamine koolitatud mudeliga uue andmehulgas.

Esimene osa tulemi saate visualiseerimise, klõpsates vasakus väljundi pordi [rongi rühmitamise] mudeli[ train-clustering-model] ja klõpsates siis käsku **Visualiseeri**. Visualiseeringu on näidatud joonisel 16.

![Rühmitamise tulem](./media/machine-learning-interpret-model-results/16.png)

Joonis 16. Visualiseerimine rühmitamise tulemi andmehulga koolitus

Teine osa, rühmitamise uued kirjed koolitatud klastrite mudeliga tulemus on näidatud joonisel 17.

![Rühmitamise tulemi visualiseerimine](./media/machine-learning-interpret-model-results/17.png)

Joonis 17. Rühmitamise tulemus uue komplekti andmete visualiseerimine

**Tulemuse tõlgendamine**

Kuigi kaks osa tulemused tulenevad erinevate katse etapid, vaadake sama ja tõlgendatakse samal viisil. Esimesed neli veergu on funktsioonid. Viimane veerg, ülesanded, on tulemus ennustamine. Kirjed, mis on määratud sama palju prognooside olema sama klaster, mis on neil sarnasusi mingil viisil (see katse kasutab vaikimisi Eukleidese kaugus meetermõõdustik). Kuna teie määratud arvu 2 olevat kogumite, ülesanded sissekandeid sildiga kas 0 või 1.

**Veebis teenuse avaldamine**

Saate avaldada klastrite katse veebiteenuse sisse ja pange selle prognoose samamoodi nagu kaks klassi liigitamine kasutusmall rühmitamiseks.

![Hinded katse iris klastrite probleem](./media/machine-learning-interpret-model-results/18.png)

Joonis 18. Hinded katse, et iris klastrite probleem

Pärast veebiteenuse Tagastatud tulem sarnaneb joonis 19. See lill on ennustatud olema kobar 0.

![Testi tõlgendamine hinded mooduli](./media/machine-learning-interpret-model-results/18_1.png)

![Hinded mooduli tulem](./media/machine-learning-interpret-model-results/19.png)

Joonis 19. Web teenuse tulemus iris kaks klassi liigitamine

## <a name="recommender-system"></a>Soovitaja süsteemi
**Näide katse**

Soovitaja süsteemide, saate kasutada restorani soovitus probleemi näide: saate soovitada restorani klientide vastavalt nende hinnangute ajalugu. Sisendandmete koosneb kolmest osast:

* Restorani hinnangute klientidele
* Funktsioon kliendiandmete
* Restorani funktsioonide andmed

On mitmeid asju, mida me saame [Rongi tikutoos soovitaja] [ train-matchbox-recommender] mooduli Azure seadme õ:

- Hinnangute antud kasutaja ja üksuse prognoosida
- Soovitame üksuste antud kasutajale
- Kasutajad, mis on seotud konkreetse kasutaja otsimine
- Antud üksusega seotud üksuste otsimine

Saate valida, mida soovite teha, valides neli võimalust **soovitaja ennustamine tüüpi** menüüs. Siin saate tutvustavad kõik neli stsenaariumi.

![Tikutoos soovitaja](./media/machine-learning-interpret-model-results/19_1.png)

Tüüpilised Azure seadme õ katse soovitaja süsteemi näeb joonisel 20. Nende soovitaja süsteemi moodulid kasutamise kohta leiate teemast [rongi tikutoos soovitaja] [ train-matchbox-recommender] ja [Keskmine tikutoos soovitaja][score-matchbox-recommender].

![Soovitaja süsteemi katse](./media/machine-learning-interpret-model-results/20.png)

Joonis 20. Soovitaja süsteemi katse

**Tulemuse tõlgendamine**

**Hinnangute antud kasutaja ja üksuse prognoosida**

**Hinnangute ennustamine** jaotises **soovitaja ennustamine liiki**valides palute soovitaja süsteemi prognoosida reiting antud kasutaja ja üksuse jaoks. [Keskmine tikutoos soovitaja] visualiseerimine[ score-matchbox-recommender] väljundi näeb joonis 21.

![Tulemuse soovitaja süsteemi--ennustamine hindamine](./media/machine-learning-interpret-model-results/21.png)

Joonis 21. Visualiseerimine keskmine tulemus soovitaja system - ennustamine hindamine

Kahe esimese veeru on esitatud sisendandmete kasutaja üksusega paari. Kolmandas veerus on prognoositud hinnang teatud üksuse kasutaja. Näiteks esimeses reas, kliendi U1048 eeldatakse, et määr restorani 135026 2.

**Soovitame üksuste antud kasutajale**

**Üksuse soovitus** **soovitaja ennustamine liiki**valides küsib soovitaja süsteemi soovitada üksuste antud kasutajale. Selle stsenaariumi valida viimase parameeter on *soovitatav üksuse valimine*. Suvandi **Kaudu hinnatud üksuste (mudeli hindamiseks)** kasutatakse peamiselt mudeli hindamise koolitus käigus. Me praegu ennustamine, valida **Kõik üksused**. [Keskmine tikutoos soovitaja] visualiseerimine[ score-matchbox-recommender] väljund näeb välja umbes 22 joonis.

![Tulemuse soovitaja süsteemi--üksuse soovitus](./media/machine-learning-interpret-model-results/22.png)

Joonis 22. Keskmine tulemus soovitaja süsteemi--üksuse soovitus visualiseerimine

Esimene kuus veergude tähistab antud kasutaja ID-d, üksuste soovitada esitatud sisendandmete. Viis veergu tähistavad üksused, mis on soovitatav kasutaja laskuvas järjestuses. Näiteks esimeses reas, on kõige soovitatav restorani kliendi U1048 134986, millele järgneb 135018, 134975, 135021 ja 132862.

**Kasutajad, mis on seotud konkreetse kasutaja otsimine**

**Seotud kasutajate** **soovitaja ennustamine liiki**valides küsib soovitaja seotud kasutajate antud kasutajale leidmiseks süsteem. Seotud kasutajad on kasutajatele, kellel on sarnane eelistused. Selle stsenaariumi valida viimase parameeter on *seotud kasutaja valiku*. Suvandi **Kaudu kasutajatele, et hinnatud üksuste (mudeli hindamiseks)** kasutatakse peamiselt mudeli hindamise koolitus käigus. Valige **Kõik kasutajad** selle etapi ennustamine. [Keskmine tikutoos soovitaja] visualiseerimine[ score-matchbox-recommender] väljundi näeb Joonis 23.

![Tulemuse soovitaja süsteemi--seotud kasutajad](./media/machine-learning-interpret-model-results/23.png)

Joonis 23. Keskmine tulemuste soovitaja system - seotud kasutajate kujutamine

Esimene kuus veerud kuvatakse antud kasutaja ID-d vaja leida seotud kasutajad, nagu esitatud sisendandmete. Viis veergu talletada prognoositud seotud kasutajate kasutaja laskuvas järjestuses. Kliendi U1048 jaoks kõige asjakohasemad kliendi on näiteks esimeses reas U1051, millele järgneb U1066, U1044, U1017 ja U1072.

**Antud üksusega seotud üksuste otsimine**

**Seostuvad üksused** jaotises **soovitaja ennustamine liiki**valides palute soovitaja seostuvad üksused antud üksuse leidmiseks süsteem. Seostuvad üksused on tõenäoliselt soovinud sama kasutaja üksused. Selle stsenaariumi valida viimase parameeter on *seotud üksuse valimine*. Suvandi **Kaudu hinnatud üksuste (mudeli hindamiseks)** kasutatakse peamiselt mudeli hindamise koolitus käigus. Valisime selle etapi ennustamine **Kõik üksused** . [Keskmine tikutoos soovitaja] visualiseerimine[ score-matchbox-recommender] väljundi näeb joonis 24.

![Tulemuse soovitaja süsteemi--seostuvad üksused](./media/machine-learning-interpret-model-results/24.png)

Joonis 24. Visualiseerimine Keskmine tulemuste soovitaja system - seostuvad üksused

Esimene kuus veergude tähistab antud üksuse ID, mis on vaja leida seostuvad üksused, nagu esitatud sisendandmete. Viis veergu talletada prognoositud seostuvad üksused üksuse laskuvas järjestuses ennast asjakohasuse. Üksuse 135026 jaoks kõige asjakohasemad üksus on näiteks esimeses reas 135074, millele järgneb 135035, 132875, 135055 ja 134992.

**Veebis teenuse avaldamine**

Avaldamiseks nende katsete saada prognoose veebiteenuste toiming sarnaneb iga neli stsenaariumi. Siin võtame näitena teine stsenaarium (antud kasutajale soovitada kirjed). Tehke sama toimingut, mis koos ülejäänud kolm.

Koolitatud mudeli koolitatud soovitaja süsteemi salvestamine ja filtreerimise sisendandmete ühe kasutaja ID veerule soovile, saate ühenduse luua katse, nagu joonisel 25 ja avaldada mis veebiteenusest.

![Hinded katse restorani soovitus probleemi](./media/machine-learning-interpret-model-results/25.png)

Joonis 25. Hinded katse restorani soovitus probleemi

Operatsioonisüsteemi veebiteenuse, tagastatud tulem sarnaneb Joonis 26. Viie soovitatav Restoran kasutaja U1048 on 134986, 135018, 134975, 135021 ja 132862.

![Valimi soovitaja süsteemi teenuse](./media/machine-learning-interpret-model-results/25_1.png)

![Näidistulemused katse](./media/machine-learning-interpret-model-results/26.png)

Joonis 26. Web teenuse tulemi restorani soovitus probleemi.


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
