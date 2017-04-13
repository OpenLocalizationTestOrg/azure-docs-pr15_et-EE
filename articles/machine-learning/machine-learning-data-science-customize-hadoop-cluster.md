<properties 
    pageTitle="Kohandada Hadoopi kogumite protsessi meeskonnatöö andmete teadus | Microsoft Azure'i" 
    description="Populaarsed Pythoni moodulid teha kättesaadavaks kohandatud Azure Hdinsightiga Hadoopi rühmades."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Andmete meeskonna teadus protsessi Azure Hdinsightiga Hadoopi kogumite kohandamine 

Selles artiklis kirjeldatakse kohandamiseks on Hdinsightiga Hadoopi kobar installida 64-bitine Anaconda (Python 2.7) iga sõlme kui klaster on ette valmistatud Hdinsightiga teenust. Samuti näitab, kuidas pääseda juurde headnode esitada kohandatud töid klaster. See kohandamine muudab paljud populaarsed Pythoni moodulid kaasatud Anaconda mugavalt kasutada kasutaja määratletud funktsioonid (UDF), mis on mõeldud töötlemine klaster taru kirjete jaoks saadaval. Juhised toiminguid, mida kasutatakse selle stsenaariumi, vaadake, [Kuidas esitada taru päringud](machine-learning-data-science-move-hive-tables.md#submit).

Menüü all eri andmete teadus keskkonnas kasutatavaid [Meeskonnatöö andmete teadus protsess (TDSP)](data-science-process-overview.md)häälestamise kirjeldavate teemade lingid.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Azure Hdinsightiga Hadoopi kobar kohandamine

Luua kohandatud Hdinsightiga Hadoopi kobar, kasutajad peavad [**Klassikaline Azure portaali**](https://manage.windowsazure.com/)sisse logida, klõpsake nuppu **Uus** vasakus allnurgas ja seejärel valige DATA SERVICES -> HDINSIGHTIGA -> **Loo kohandatud** avab akna **Kobar üksikasjad** . 

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Sisestusmeetodi kobar lehel konfiguratsioon 1 luua nime ja muude väljade väärtusi. Klõpsake noolt konfiguratsiooni järgmisele lehele liikumiseks. 

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Konfiguratsiooni lehel 2 Sisestage **Andmed sõlmed**number, valige **Piirkond/VIRTUAL võrgu**ja valige **Pea sõlm** ja **Andmete sõlm**suurused. Klõpsake noolt konfiguratsiooni järgmisele lehele liikumiseks.

>[AZURE.NOTE] **Piirkond/VIRTUAL NETWORK** peab olema sama mis salvestusruumi konto, mida saab kasutada HDInsight Hadoopi kobar piirkond. Muul juhul neljas konfiguratsiooni lehel salvestusruumi konto, mille soovite kasutajate ei kuvata rippmenüü loendis **MEILIKONTO nime**.

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Lehel konfiguratsioon 3, sisestage kasutajanimi ja parool Hdinsightiga Hadoopi kobar. **Ärge** valige _Enter taru/Oozie Metastore_. Klõpsake noolt konfiguratsiooni järgmisele lehele liikumiseks. 

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Määrake lehel konfiguratsioon 4 salvestusruumi konto nime, vaikimisi container Hdinsightiga Hadoopi klaster. Kui kasutajad märkige _loomine vaikimisi container_ **Vaikimisi CONTAINER** ripploendis, luuakse mille klaster sama nimi. Klõpsake noolt viimasele konfiguratsiooni lehele.

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Viimasel lehel **Script toimingud** konfiguratsiooni, klõpsake nuppu **skripti toimingu lisamine** ja teksti väljade täitmine järgmised väärtused.
 
* **Nimi** – toimingut skripti nimeks suvalist märgistringi. 
* **Sõlm tüüp** – valige **kõik sõlmed**. 
* **SKRIPTI URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* on avalik container salvestusruumi konto 
    * *getgoing* kasutame kasutajate töö Azure hõlbustamiseks PowerShelli skripti faile ühiskasutusse anda. 
* **Parameetrite** - (jätke see väli tühjaks)

Lõpuks klõpsake kohandatud Hdinsightiga Hadoopi kobar loomise alustamiseks märke. 

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Juurdepääs pea sõlme Hadoopi kobar

Kasutajate tuleb lubada Kaugpöördusteenuse Hadoopi klaster Azure Hadoopi kobar pea sõlme RDP kaudu juurdepääsu. 

1. [**Klassikaline Azure portaali**](https://manage.windowsazure.com/)sisse logida, **Hdinsightiga** valige vasakul, loendist klaster Hadoopi kogumite, klõpsake vahekaarti **KONFIGUREERIMINE** ja klõpsake lehe allservas **Lubada REMOTE** ikooni.
    
    ![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. Klõpsake aknas **Konfigureerimine kaugtöölaua** sisestage väljad kasutajanimi ja parool ja valige Kaugpöördusteenuse aegumiskuupäeva. Klõpsake sisse märgi Hadoop kobar pea sõlme remote juurdepääsu lubamiseks.

    ![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] Kasutajanime ja parooli remote Access ei ole kasutajanimi ja parool, mida kasutate Hadoopi kobar loomisel. Need on eraldi mandaadikomplekt. Lisaks remote Accessi aegumiskuupäeva peab olema tänasest kuupäevast 7 päeva jooksul.

Pärast Kaugpöördusteenuse on lubatud, klõpsake **ühenduse loomine** remote lehe allosas pea sõlme sisse. Saate sisse logida Hadoopi kobar varem määratud kasutajale Kaugpöördusteenuse mandaadi sisestamise teel pea sõlme.

![Tööruumi loomine](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Järgmiste juhiste juurde täiustatud analüüsi käigus vastendatakse [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja võib sisaldada toiminguid, mis andmete viimine Hdinsightiga, töötlemine ja kuulake seal õ andmete Azure seadme õppe ettevalmistamiseks.

Vaadake, [Kuidas esitada taru päringute](machine-learning-data-science-move-hive-tables.md#submit) juhised juurdepääsu Pythoni moodulid, mis sisalduvad Anaconda kobar sisse kasutaja määratletud funktsioonid (UDF), mida kasutatakse taru kirjed, mis on talletatud klaster pea sõlme.

 
