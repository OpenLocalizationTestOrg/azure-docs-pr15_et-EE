<properties 
    pageTitle="Veebisaidi logi analüüs Hadoopi taru abil | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada taru Hdinsightiga veebisaidi logid analüüsimiseks. Saate logifaili saab kasutada sisendina Hdinsightiga tabelisse ja päringu andmete HiveQL abil." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Logide veebisaitidel analüüsimiseks Hdinsightiga taru kasutamine

Saate teada, kuidas kasutada HiveQL Hdinsightiga analüüsimiseks logid veebisaidilt. Veebisaidi logi analüüs saab segmendi publikule tegevuse alusel, liigitada, demograafia saidi külastajatele ja välja selgitada sisu need vaade, nad on pärit veebisaitide ja jne.

Selles näites kasutate mõnda Hdinsightiga kobar sagedus külastamine veebisaidile suunavate päev ülevaate saamiseks veebilehel logifailide analüüsimiseks. Kuvatakse genereerida ka kokkuvõtte veebisaidi kasutajate kogemusi tõrkeid. Saate teada, kuidas:

- Ühenduse Azure'i bloobimälu, mis sisaldab veebisaidi logifailid.
- Päringu need logid TARU tabelite loomine.
- Andmete analüüsimiseks TARU päringute loomine.
- Microsoft Exceli abil saate ühendada Hdinsightiga (abil analüüsitud andmete toomiseks avatud andmebaasipöördus (ODBC).

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Eeltingimused

- Teil peab on ette valmistatud Hadoopi kobar Windows Azure Hdinsightiga kohta. Juhised leiate teemast [Sätte Hdinsightiga kogumite][hdinsight-provision]. 
- Peab teil olema Microsoft Excel 2013 või Excel 2010 installitud.
- [Microsoft taru ODBC-draiver](http://www.microsoft.com/download/details.aspx?id=40886) andmed Excelisse importida taru peab teil olema.


##<a name="to-run-the-sample"></a>Valimi käivitamiseks

1. [Azure portaali](https://portal.azure.com/)kaudu Startboard (kui see on kinnitatud kobar seal), klõpsake kohta, kuhu soovite käivitada valimi kobar paani.

2. Keelest kobar jaotises **Kiirlingid**valige **Kobar armatuurlaud**, ja klõpsake **Hdinsightiga kobar armatuurlaua**keelest **Kobar armatuurlaua** . Teise võimalusena saate otse avada armatuurlaual järgmist URL-i abil.

        https://<clustername>.azurehdinsight.net
    
    Vastava viiba kuvamisel autentida, kasutades administraatori kasutajanime ja parooliga, mida kasutasite kui ettevalmistamise klaster.
  
2. Avatakse veebileht, klõpsake vahekaarti **Kasutamise alustamine Galerii** ja klõpsake kategoorias **lahenduste näidisandmetega** **veebisaidi Log** proovi.

3. Järgige juhiseid veebilehel valimi lõpuleviimiseks.

##<a name="next-steps"></a>Järgmised sammud
Proovige järgmises näites: [analüüsitakse andurite andmeid taru koos Hdinsightiga abil](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
