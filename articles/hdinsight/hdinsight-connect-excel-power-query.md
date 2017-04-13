<properties
    pageTitle="Ühenduse loomine Exceli Power Query Hadoopi | Microsoft Azure'i"
    description="Saate teada, kuidas ära business intelligence komponendid ja Exceli Accessi andmed salvestatakse Hadoopi Hdinsightiga Power Query abil."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Ühenduse Hadoopi Exceli Power Query abil

Üks klahv funktsioon Microsoft andmed – suured lahendus on Microsoft business ärianalüüsi (BI) osade koos Hadoopi kogumite rakenduses Windows Azure Hdinsightiga integreerimise. Esmane näide selle integreerimine on võimalus Exceli ühenduse Azure Storage konto, mis sisaldab andmeid, mis on seotud klaster Hadoopi Exceli lisandmooduli Microsoft Power Query abil. Selles artiklis tutvustatakse, kuidas häälestada ja Power Query abil päringu andmete seostatud Hadoopi klaster hallatud Hdinsightile.

> [AZURE.NOTE] Ajal selles artiklis toodud juhiseid saab kasutada Linux või Windowsi-põhiste Hdinsightiga kobar, Windows on vaja klient töökoht.

### <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Hdinsightiga kobar**. Konfigureerimiseks ühte, lugege teemat [alustamine Windows Azure Hdinsightiga][hdinsight-get-started].
- **A töökoha** , kus töötab Windows 7, Windows Server 2008 R2 või uuem operatsioonisüsteem.
- **Office 2013 Professional Plus, Office 365 ProPlus, Office 2010 Professional Plus või Excel 2013 eraldiseisev versioon**.


## <a name="install-power-query"></a>Installige Power Query

Power Query saab kasutada andmete importimine erinevatest allikatest Microsoft Excelile, kus see saate power BI tööriistad nagu Powerpivoti ja Power View. Eelkõige Power Query saate importida andmeid, mis on väljund või mille on loonud Hadoopi töö töötab ka Hdinsightiga kobar.

Laadige Microsoft Power Query for Excel [Microsofti allalaadimiskeskusest] [ powerquery-download] ja installige see.

## <a name="import-hdinsight-data-into-excel"></a>Exceli andmete importimine Hdinsightiga

Power Query lisandmooduli Exceli abil saate importida andmed Excelisse, kus Ärianalüüsi tööriistade nagu Powerpivoti ja Power MAPI saab kontrollida, analüüsida ja selle andmete esitamiseks klaster Hdinsightiga lihtne.

**Saate importida andmed on Hdinsightiga kobar**

1. Avage Excel.

2. Looge uus tühi töövihik.

3. Klõpsake menüüs **Power Query** , klõpsake **: Azure'i**ja klõpsake **Microsoft Azure Hdinsightiga**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Märkus:** Kui te ei näe menüüd **Power Query** , avage **fail** > **Suvandid** > **Lisandmoodulid**ja valige **lehe allosas rippmenüü Halda** **COM-lisandmoodulid** . Valige nupp **Go …** ja veenduge, et Exceli lisandmooduli Power Query ruut on märgitud.

    **Märkus:** Power Query võimaldab teil andmete importimine HDFS, klõpsates nuppu **Muust allikast**.

3. **Konto nimi**, sisestage seotud klaster Azure'i bloobimälu salvestusruumi konto nimi ja seejärel klõpsake nuppu **OK**. See võib olla [vaikekonto salvestusruumi](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) või lingitud salvestusruumi.  Vorming on *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Konto võti**, sisestage bloobimälu salvestusruumi konto võti ja klõpsake siis nuppu **Salvesta**. (Peate selleks ainult pääsete selle poe esimest korda.)

5. Klõpsake akna Päringuredaktor vasakul paanil **navigaator** topeltklõpsake bloobimälu salvestusruumi container nime. Container nimi on vaikimisi kobar nime sama nimi.

6. Otsige üles **HiveSampleData.txt** (kausta tee on **.. veeru **nimi** / taru/ladu/hivesampletable/**), ja seejärel klõpsake käsku **kahendarvu** vasakus servas HiveSampleData.txt. HiveSampleData.txt on kaasas kõigi kobar. Soovi korral saate oma faili.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Kui soovite, võite veerunimede ümber nimetada. Kui olete valmis, klõpsake nuppu **Sule ja laadi**.  Andmete ühendamine oma töövihikuga laadimist:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis soovite õpitut Power Query kasutamine andmete toomiseks Hdinsightiga Excelisse. Samuti saate tuua andmeid hdinsightist Azure SQL-i andmebaasi. Samuti on võimalik üles laadida andmed üheks Hdinsightiga. Lisateabe saamiseks lugege järgmisi artikleid:

* [Ühenduse loomine Exceli hdinsightiga Microsoft taru ODBC-draiver][hdinsight-ODBC]
* [Laadi andmed Hdinsightiga][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
