<properties
    pageTitle="Andur analüüside taru ja Hadoopi abil | Microsoft Azure'i"
    description="Saate teada, kuidas analüüsida andurite andmeid kasutades taru päringu konsooli Hdinsightiga (Hadoopi) ja seejärel PowerView Microsoft Exceli andmete visualiseerimiseks."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Taru päringu konsooli kasutamine rakenduses Hdinsightiga Hadoopi andur andmete analüüsiks

Saate teada, kuidas analüüsida andurite andmeid kasutades taru päringu konsooli Hdinsightiga (Hadoopi) ja seejärel Microsoft Excelis andmete visualiseerimine Power View abil.

> [AZURE.NOTE] Juhised selles dokumendis töötavad ainult Windowsi-põhiste Hdinsightiga kogumite.

Selles näites saate kasutada taru Küte, ventilatsioon ja air kliimaseadmed süsteemide ajalooliste andmete töötlemise süsteemid, mida ei saa usaldusväärselt määramine temperatuuri tuvastamiseks. Saate teada, kuidas:

- Päringu andmete komaeraldusega väärtuste (CSV) faili talletatud TARU tabelite loomine.
- Andmete analüüsimiseks TARU päringute loomine.
- Microsoft Exceli abil saate ühendada Hdinsightiga (kasutades avatud andmebaasipöördus (ODBC) analüüsitud andmete toomiseks.
- Andmete visualiseerimine Power View abil.

![Lahendus arhitektuur skeem](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Eeltingimused

* Mõne Hdinsightiga (Hadoopi) kobar: kohta leiate teavet [Hdinsightiga sisse säte Hadoopi kogumite](hdinsight-provision-clusters.md) luua klaster.

* Microsoft Excel 2013

    > [AZURE.NOTE] Microsoft Exceli kasutatakse andmete visualiseerimine [Power](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US)View.

* [Microsoft taru ODBC-draiver](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Valimi käivitamiseks

1. Liikuge järgmine URL oma veebibrauseri kaudu. Asendage `<clustername>` klaster Hdinsightiga nimi.

        https://<clustername>.azurehdinsight.net

    Vastava viiba kuvamisel autentida, kasutades administraatori kasutajanime ja parooliga, mida kasutasite, kui see ettevalmistamise.

2. Avatakse veebileht, klõpsake vahekaarti **Kasutamise alustamine Galerii** ja klõpsake kategoorias **lahenduste näidisandmetega** **Andur Andmeanalüüs** valimi.

    ![Töö alustamine Galerii](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Järgige juhiseid veebilehel valimi lõpuleviimiseks.
