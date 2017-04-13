<properties
    pageTitle="Samm 2: Andmete laadida seadme õ katse | Microsoft Azure'i"
    description="Töötada lühiülevaade sõnastikupõhise lahenduse etappi 2: üles salvestada avalikest andmetest Azure seadme õ Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Kiirtutvustus samm 2: Laadida olemasolevad andmed Azure seadme õ katse

See on teise etapi kiirtutvustus, [töötada ennustav lahenduse Azure seadme õppe](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Seadme õ tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Laadige üles olemasolevad andmed**
3.  [Looge uus katse](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Koolitada ja hindate mudelid](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Accessi veebiteenuse](machine-learning-walkthrough-6-access-web-service.md)

----------

Arendada sõnastikupõhise näidis krediitkaardiga risk, läheb vaja andmeid, mida saate kasutada koolitada ja testige mudel. See selgituse kasutame "UCI Statlog (saksa krediitkaardi andmed) andmehulga" UCI masina õ hoidla. Leiate siit:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.UCI.edu/ml/Datasets/statlog+(German+Credit+Data)</a>

Kasutame fail nimega **german.data**. Selle faili allalaadimiseks oma arvuti kõvakettale.  

Tuvastatavate sisaldab ridade 20 muutujate 1000 viimase taotleja krediitkaarti. Nende 20 muutujate tähistavad soovitud andmekomplekti funktsiooni vektorkuju, mis pakub kindlaks tunnused iga krediitkaardiga taotleja. Täiendav veerg igal real tähistab taotleja arvutatud krediidi risk, kõrge risk madal krediidi risk ja 300 tuvastatud 700 taotleja abil.

Veebisaidi UCI kirjeldatakse atribuudid funktsiooni vektorkuju, mis sisaldavad finantsteavet, krediidi ajalugu, töö oleku ja isikliku teabe. Iga taotleja kahendarvu hinnang on antud, mis näitab, kas need on väike või suur krediidi risk.  

Kasutame koolitada ennustav mudeli andmed. Kui meil on valmis, meie mudel peaks oskama teavet uute üksikisikutele aktsepteerimiseks ja prognoosida, kas need on kõrge või madal krediitkaardiga riski.  

Siin on üks huvitav. Andmekomplekti kirjeldus selgitab, et misclassifying isiku madal krediidi risk, kui ta on tegelikult kõrge krediidi risk on 5 korda kulukam selle kui misclassifying madal krediidi risk nii kõrge. Üks lihtne viis arvesse meie katse on (5 korda) need kirjed, mis tähistab keegi kõrge krediidi risk dubleerida. Siis, kui mudeli misclassifies kõrge krediidi risk madal, teeme selle klassifikatsioonivead 5 korda, kui iga Dubleeri jaoks. Selle tõrke koolitus tulemuste maksumus suurendada.  

##<a name="convert-the-dataset-format"></a>Andmekomplekti teisendamine
Algne andmekomplekti kasutab tühi eraldatud väärtuste vorming. Seadme õ Studio töötab paremini komaeraldusega (CSV) faili, nii, et teisendame andmekomplekti, asendades komadega tühikuid.  

On palju võimalusi, kuidas teisendada andmed. Üks viis on järgmine Windows PowerShelli käsu abil:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Teine võimalus on Unix sed käsuga:  

    sed 's/ /,/g' german.data > german.csv  

Mõlemal juhul oleme loonud komaga eraldatud versiooni fail nimega kasutame meie katse **german.csv** andmed.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Andmekomplekti üleslaadimine masina õ Studio

Kui andmed on teisendatud CSV-vorming, läheb vaja laadige see arvuti õ Studio sisse. Alustamine seadme õ Studio kohta leiate lisateavet teemast [Microsoft Azure'i masina õ Studio Avaleht](https://studio.azureml.net/).

1.  Avage arvuti õ Studio ([https://studio.azureml.net](https://studio.azureml.net)). Kui teil palutakse sisse logida, kasutage tööruumi omanikuna määratud konto.
1.  Klõpsake akna ülaosas vahekaarti **Studio** .
1.  Valige **+ Uus** akna allosas.
1.  Valige **ANDMEKOMPLEKT**.
1.  Valige **kohalik fail**.
1.  Dialoogiboksis **Uus andmekomplekti üleslaadimine** nuppu **Sirvi** ja leida **german.csv** loodud fail.
1.  Sisestage nimi andmekomplekti. See selgituse me helistan selle "UCI Saksa krediitkaardi andmed".
1.  Andmetüüp, valige **üldise CSV-faili ilma päise (. nh.csv)**.
1.  Kui soovite, lisage kirjeldus.
1.  Klõpsake nuppu **OK**.  

![Andmekomplekti üleslaadimine][1]  


See laadib andmed üheks andmekomplekti moodul, mis kasutame katse.

> [AZURE.TIP] Andmekomplektide Studio üleslaaditud haldamiseks klõpsake vahekaarti **ANDMEKOMPLEKTIDE** Studio akna vasakul.

Erinevat tüüpi andmete importimine katse kohta leiate lisateavet teemast [Azure seadme õ Studio üheks koolitus andmete importimine](machine-learning-data-science-import-data.md).

**Järgmine: [Loo uus katse](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
