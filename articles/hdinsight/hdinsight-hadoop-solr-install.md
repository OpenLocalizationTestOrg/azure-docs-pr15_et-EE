<properties
    pageTitle="Skripti toimingu abil saate installida Solri Hadoopi kobar | Microsoft Azure'i"
    description="Saate teada, kuidas kohandada Hdinsightiga kobar Solri skripti toimingu abil."
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
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installimine ja kasutamine Solri Hdinsightiga Hadoopi kogumite

Saate teada, kuidas kohandada Windowsi vastavalt Hdinsightiga kobar Solri skripti toimingu abil ning Solri abil saate andmeid otsida. Linux-põhine kobar Solri kasutamise kohta leiate teemast [installimine ja kasutamine Solri Hdinsightiga Hadoopi kogumite (Linux) kohta](hdinsight-hadoop-solr-install-linux.md).
 
Klõpsake mis tahes tüüpi kobar (Hadoopi, torm, HBase, säde) Solri saate installida Windows Azure Hdinsightiga *Skripti toimingu*abil. Proovi skripti Solri installimiseks on Hdinsightiga kobar on kirjutuskaitstud salvestusruumi Azure'i bloobimälu [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)veebisaidil saadaval. 

Skripti näide töötab ainult Hdinsightiga kobar versioon 3,1. Hdinsightiga kobar versioonide kohta leiate lisateavet teemast [Hdinsightiga kobar versioonid](hdinsight-component-versioning.md).

Selles teemas kasutatud skripti näide loob Windowsi-põhiste Solri kobar konkreetse konfiguratsiooni. Kui soovite konfigureerida Solri kobar erinevate saidikogumid, shards skeemid, koopiad jne, peate muutma skripte ja Solri kahendfaile vastavalt sellele.

**Seotud artiklid**

- [Installimine ja kasutamine Solri Hdinsightiga Hadoopi kogumite (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Rakenduses Hdinsightiga loomine Hadoopi kogumite](hdinsight-provision-clusters.md): üldist teavet Hdinsightiga kogumite loomise kohta.
- [Kohandada Hdinsightiga kobar skripti toimingu abil][hdinsight-cluster-customize]: Üldteave kohandamiseks Hdinsightiga kogumite skripti toimingu abil.
- [Arendamise skripti toimingu skriptid Hdinsightiga](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Mis on Solri?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solri</a> on ettevõtte otsing platvorm, mis võimaldab võimsaid otsingu andmete põhjal. Kuigi Hadoopi võimaldab talletamine ja suure hulga andmete haldamine, pakub Apache Solri otsinguvõimalusi andmete toomiseks. 

## <a name="install-solr-using-portal"></a>Installige Solri portaalis

1. Käivitage luua klaster **Luua kohandatud** suvandi abil, nagu on kirjeldatud aadressil [loomine Hadoopi kogumite Hdinsightiga sisse](hdinsight-provision-clusters.md#portal).
2. Viisardi lehel **Script toimingud** klõpsake **skripti toimingu lisamine** üksikasjad skripti kohta, nagu allpool näidatud:

    ![Kasutage skripti toimingu kohandamiseks on kobar] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Kasutage skripti toimingu kohandamiseks on kobar")

    <table border='1'>
        <tr><th>Atribuut</th><th>Väärtus</th></tr>
        <tr><td>Nimi</td>
            <td>Määrake skripti toimingu nimi. Näiteks <b>Installida Solri</b>.</td></tr>
        <tr><td>Skripti URI</td>
            <td>Määrake soovitud ühtse identifikaator (URL) skripti, mis on vaja järgida klaster kohandamiseks. Näiteks <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Sõlme tüüp</td>
            <td>Määrake sõlmi, mille kohandamine skript käivitatakse. Saate valida <b>kõik sõlmed</b>, <b>ainult pea sõlmed</b>või <b>ainult töötaja sõlmed</b>.
        <tr><td>Parameetrid</td>
            <td>Määrake soovitud parameetrid skripti vajadusel. Skripti Solri installimiseks ei nõua kõik parameetrid nii, et võite seda tühjaks jätta.</td></tr>
    </table>

    Saate lisada rohkem kui ühe skripti toimingu klaster mitme komponentide installimine. Kui olete lisanud skriptide, klõpsake klaster loomise alustamiseks märke.


## <a name="use-solr"></a>Kasutage Solri

Peab algama indekseerimine Solri mõned andmefailid. Seejärel saate Solri käivitamiseks otsingupäringuid indekseeritud andmete põhjal. Järgmiste toimingute Solri kasutamine on Hdinsightiga kobar:

1. **Kasutage Kaugtöölaua protokolli (RDP) kaugtöölaua Hdinsightiga kobar abil installitud Solri sisse**. Azure'i portaalis lubamine üheks klaster Solri installitud ja seejärel remote loodud klaster Kaugtöölaud. Juhised leiate teemast [Hdinsightiga kogumite abil RDP ühenduse loomine](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Registri Solri üleslaadimisel andmefailid**. Kui soovite indekseerida Solri, Pange dokumendid see, mida soovite otsida. Registrisse Solri, kasutage RDP kaugtöölaua klaster, liikuge töölauale, avage Hadoopi käsurea ja liikuge **C:\apps\dist\solr-4.7.2\example\exampledocs**. Käivitage järgmine käsk:

        java -jar post.jar solr.xml monitor.xml

    Näete järgmist väljundi konsooli:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Post.jar kasuliku indeksite Solri kahe valimi dokumente, **solr.xml** ja **monitor.xml**. Post.jar kasuliku ja valimi dokumendid on saadaval Solri installiga.

3. **Kasutage Solri armatuurlaual otsing indekseeritud dokumendid**. RDP-seansi Hdinsightiga klaster, avage Internet Explorer ja käivitage Solri armatuurlaua veebisaidil **http://headnodehost:8983/Solri / #/**. Klõpsake vasakpoolsel paanil **Core Vaateselektori** ripploendit valige **collection1**ja sees, klõpsake nuppu **päring**. Näiteks valige ja tagastada kõik dokumendid Solri, sisestage järgmised väärtused.

    * Sisestage väljale **k** ** \*:**\*. See naaseb kõik dokumendid, mis on indekseeritud Solri. Kui soovite otsida stringi teatud dokumendid, saate sisestada see string siin.
    
    * Valige väljal **wt** teksti vorming. Vaikimisi on **json**. Klõpsake **päringu**.

    ![Kasutage skripti toimingu kohandamiseks on kobar] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Päringu Solri armatuurlaual")
    
    Väljund tagastab kahe dokumendid, mis kasutasime indekseerimise Solri. Väljund meenutab järgmist:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Soovitatav: varundamine indekseeritud andmed Solri Azure'i bloobimälu Hdinsightiga kobar-ga seostatud**. Hea tava varundada tuleks indekseeritud andmete Solri kobar üksuste sõlmi Azure'i bloobimälu kaudu. Tehke seda teha järgmist:

    1. RDP-seansi, avage Internet Explorer ja valige käsk järgmine URL:

            http://localhost:8983/solr/replication?command=backup

        Peaksite nägema vastuse umbes järgmine:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Liikuge Kaugseanss, {SOLR_HOME}\{saidikogumi} \data. Klaster loodud valimi skripti kaudu, tuleb see **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Selles asukohas peaksite nägema hetktõmmise kausta nime, mis on sarnane *loodud *hetktõmmis.* ajatempli***.

    3. ZIP hetktõmmise kaust ja laadige see Azure'i bloobimälu. Hadoopi käsureal liikuge hetktõmmise kausta asukoht ning abil järgmine käsk:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        See käsk kopeerib hetktõmmis /example/data/jaotises container salvestusruumi kontoga seostatud klaster vaikimisi sees.

## <a name="install-solr-using-aure-powershell"></a>Installige Solri Aure PowerShelli abil

Lugege teemat [kohandamine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Valimi näitab, kuidas installida Azure PowerShelli kaudu säde. Peate kohandada skripti [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)kasutama.

## <a name="install-solr-using-net-sdk"></a>Kasutades .NET SDK Solri installimine

Lugege teemat [kohandamine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Valimi näitab, kuidas installida säde .NET SDK abil. Peate kohandada skripti [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)kasutama.



## <a name="see-also"></a>Vt ka

- [Installimine ja kasutamine Solri Hdinsightiga Hadoopi kogumite (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Rakenduses Hdinsightiga loomine Hadoopi kogumite](hdinsight-provision-clusters.md): üldist teavet Hdinsightiga kogumite loomise kohta.
- [Kohandada Hdinsightiga kobar skripti toimingu abil][hdinsight-cluster-customize]: üldist teavet kohandamiseks Hdinsightiga kogumite skripti toimingu abil.
- [Arendamise skripti toimingu skriptid Hdinsightiga](hdinsight-hadoop-script-actions.md).
- [Installimine ja kasutamine säde Hdinsightiga kogumite][hdinsight-install-spark]: skripti toimingu valimi säde installimise kohta.
- [R installimine Hdinsightiga kogumite][hdinsight-install-r]: skripti toimingu valimi R. installimise kohta
- [Installige Giraph Hdinsightiga kogumite kohta](hdinsight-hadoop-giraph-install.md): skripti toimingu valimi Giraph installimise kohta.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
