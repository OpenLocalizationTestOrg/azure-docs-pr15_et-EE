<properties 
    pageTitle="Silumine mudelisse Azure seadme õ | Microsoft Azure'i" 
    description="Selgitatakse, kuidas kuidas silumine mudelisse Azure seadme õ." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Mudelisse Azure seadme õ silumine

Selles artiklis selgitatakse, kuidas oma andmemudelites Microsoft Azure seadme õ silumine. See hõlmab konkreetselt, võimalikku põhjust, miks kas järgmised kaks tõrge juhtudel võib tekkida, kui mudeli töötab:

* [Rongi mudeli] [ train-model] mooduli põhjustab tõrge 
* [Keskmine mudeli] [ score-model] mooduli tagastab vale tulemi 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Rongi mudeli mooduli põhjustab tõrge

![image1](./media/machine-learning-debug-models/train_model-1.png)

[Rongi mudeli] [ train-model] mooduli eeldab, et järgmised 2 sisendeid:

1. Esitatud Azure seadme õ mudelite kogumisest liigitamine/regressiooni mudel tüüp
2. Koolitus andmed määratud veeru silt. Veeru silt määrab prognoosida muutujana. Ülejäänud veerud kaasata on eeldatakse, et funktsioone.

Selle mooduli põhjustab tõrge järgmistel juhtudel:

1. Veeru silt on määratud valesti, kuna rohkem kui üks veerg on valitud sildi või vale veeru indeks on valitud. Näiteks kehtib juhul, kui 30 veeruindeksi kasutati Sisestuskeel andmekomplekti, mis oli ainult 25 veergude.

2. Andmekomplekti ei sisalda ühtegi funktsiooni veergu. Näiteks kui Sisestuskeel andmekomplekti on ainult 1 veerg, mis on märgitud veerus silt, ei oleks ühtegi funktsiooni, mille abil koostada mudel. Käesolevas näites [Rongi mudeli] [ train-model] mooduli võib põhjustada tõrke.

3. Sisestuskeel andmekomplekti (funktsioone või sildi) sisaldavad väärtusena lõpmatuseni.


## <a name="score-model-module-does-not-produce-correct-results"></a>Keskmine mudeli mooduli ei tooda õigeid tulemeid

![image2](./media/machine-learning-debug-models/train_test-2.png)

Tüüpilised koolitus/testimine Graphi jaoks kontrollitud Õppekeskuse, [Tükeldatud andmetega] [ split] mooduli jagab algse andmekomplekti kaks osa: osa, mida kasutatakse mudeli koolitada ja reserveeritud Keskmine, kui hästi koolitatud mudeli teeb andmete põhjal see osa ei koolitada kohta. Seejärel kasutatakse koolitatud mudeli Keskmine, pärast mida tulemusi hinnatakse mudeli täpsuse määratlemiseks testi andmed.

[Keskmine mudeli] [ score-model] mooduli nõuab kahte sisendina:

1. Koolitatud mudeli väljundi [Rongi] mudel[ train-model] mooduli
2. Hinded andmekomplekti ei ole, mis mudeli oli ei kohta

Võib juhtuda, et isegi juhul, kui katse õnnestub, [Keskmine mudeli] [ score-model] mooduli tagastab vale tulemi. Mitme stsenaariumid võib põhjustada see juhtub:

1. Kui määratud silt on kategooriline ja regressiooni mudeli õpetatakse andmeid, on vale väljund toodetud [Keskmine mudeli] [ score-model] mooduli. Selle põhjuseks on regressiooni nõuab pidev vastuse muutujana. Sel juhul tuleks kasutada liigitamine mudeli sobivam. 
2. Samamoodi kui mudeli liigitamine õpetatakse andmekogumis on ujuv punkti arvud veerus silt, võivad selle anda soovimatuid tulemusi. See on liigitamine nõuab eraldi vastuse muutuja, mis võimaldab ainult väärtuste vahemiku tunnid piiratud ja tavaliselt veidi väike kogumi.
3. Kui hinded andmekomplekti kõik funktsioonid koolitada mudeli, [Keskmine mudel] ei sisalda[ score-model] toodaks tõrge.
4. [Keskmine mudeli] [ score-model] ei anna mis tahes väljundi vastav hinded andmekomplekti puuduv väärtus või mis tahes funktsiooni lõpmatu väärtus sisaldav rida.
5. [Keskmine mudeli] [ score-model] võib põhjustada identse väljundeid hinded andmekomplekti kõik read. See võib juhtuda näiteks aeg proovite liigitamine abil otsust metsade, kui valitakse väikseima arvu proovi lehtede sõlm kohta rohkem kui arvu koolitus näited saadaval.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
