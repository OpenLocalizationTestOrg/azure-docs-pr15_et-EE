<properties
    pageTitle="Hadoopi koos tooni kasutamine Hdinsightiga Linux kogumite | Microsoft Azure'i"
    description="Siit saate teada, kuidas installida ja kasutada tooni Hadoopi kogumite Hdinsightiga Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installimine ja kasutamine tooni Hdinsightiga Hadoopi kogumite

Saate teada, kuidas installida tooni Hdinsightiga Linux kogumite ja kasutada tunneling taotlused marsruutimiseks toon.

## <a name="what-is-hue"></a>Mis on tooni?

Värvitoon on suhelda Hadoopi kobar veebirakenduste komplekt. Tooni abil saate sirvida seostatud Hadoopi kobar (WASB puhul Hdinsightiga kogumite) talletamist, käivitage taru töö ja siga skriptide jne. Järgmised komponendid on saadaval kõige uuemas tooni installe on Hdinsightiga Hadoopi kobar.

* Mesilasvaha taru redaktor
* Siga
* Metastore haldur
* Oozie
* FileBrowser (mille räägib WASB vaikimisi container)
* Töö brauseris

> [AZURE.WARNING] Koos Hdinsightiga kobar komponendid on täielikult toetatud ja Microsoft Support aitab eristada ja nende komponentide seotud probleemide lahendamiseks.
>
> Kohandatud komponendid tugiteenuseid äriliselt mõistlik aidata teil selle probleemi tõrkeotsingu sooritamiseks. See võib põhjustada probleemi lahendamine või palub teil otsimist ja avatud allika tehnoloogiad, kui leitakse sügav teadmised selle tehnoloogia. Näiteks on palju kogukonnafoorumi saite, mida saab kasutada, nt: [MSDN-i Foorum Hdinsightiga](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ka Apache projektide on projekti saitidel [http://apache.org](http://apache.org), näiteks: [Hadoopi](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Installige tooni skripti toimingute kasutamine

Skripti järgmine toiming saab installida tooni Linux-põhine Hdinsightiga kobar.
https://hdiconfigactions.blob.Core.Windows.net/linuxhueconfigactionv02/install-Hue-Uber-v02.sh
    
Sellest jaotisest leiate juhised selle kohta, kuidas kasutada skripti, kui ettevalmistamise klaster Azure'i portaalis. 

> [AZURE.NOTE] Azure'i PowerShelli, Azure'i CLI, Hdinsightiga .NET SDK või Azure ressursihaldur malle saab kasutada skripti toimingute. Saate rakendada ka skripti toimingud juba töötab kogumite. Lisateabe saamiseks vt [kohandamine Hdinsightiga kogumite skripti toimingute](hdinsight-hadoop-customize-cluster-linux.md).

1. Käivitage ettevalmistamise klaster [sätte Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md#portal)rühmades Linux juhiste abil, kuid täitke ettevalmistamise.

    > [AZURE.NOTE] Hdinsightiga kogumite tooni installimiseks soovitatav headnode maht on vähemalt A4 (8 tuuma, 14 GB mälu).

2. **Valikuline konfigureerimine** enne, valige **Skripti toimingud**ja teavet, nagu allpool näidatud:

    ![Sisesta skripti toimingu parameetrid tooni] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Sisesta skripti toimingu parameetrid tooni")

    * __Nimi__: sisestage skripti toimingu sõbralik nimi.
    * __Skripti URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Juht__: märkige see ruut
    * __Töötaja__: See väli tühjaks jätta.
    * __ZOOKEEPER__: See väli tühjaks jätta.
    * __Parameetrite__: See väli tühjaks jätta.

3. **Skripti toimingud**allosas kasutada konfiguratsiooni salvestamiseks **Valige** nupp. Lõpuks nuppu **Valige** **Valikuline konfigureerimine** tera allosas salvestamiseks kasutage valikuline konfiguratsiooniteavet.

4. Jätkake ettevalmistamise klaster, nagu on kirjeldatud [sätte Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md#portal)rühmades Linux.

## <a name="use-hue-with-hdinsight-clusters"></a>Hdinsightiga kogumite tooni kasutamine

SSH Tunneling on ainus viis juurdepääsu tooni klaster, kui see töötab. SSH Tunneling võimaldab liikluse ja minge otse headnode klaster, kus töötab toon. Kui klaster on lõpule jõudnud, ettevalmistamise, siis tehke järgmist tooni kasutamine on Hdinsightiga Linuxi kobar.

1. Teabe kasutamine [kasutada SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, ja muud web UI's juurde pääseda](hdinsight-linux-ambari-ssh-tunnel.md) loomiseks on SSH tunneliga kliendi süsteemist Hdinsightiga klaster ja konfigureerimist kasutama SSH tunneliga proxy veebibrauseris.

2. Kui olete loonud mõne SSH tunneliga ja puhverserveri liikluse läbi teie brauseri konfiguratsioon, leiate esmane pea sõlme hosti nimi. Selleks ühenduse klaster port 22 SSH abil. Näiteks `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` kus __kasutajanimi__ on teie SSH kasutajanimi ja __CLUSTERNAME__ on klaster nimi.

    SSH kasutamise kohta lisateabe saamiseks vaadake järgmisi dokumente:

    * [SSH kasutamine Linux-põhine Hdinsightiga kliendilt Linux, Unix või Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Windows kliendi Linux-põhine Hdinsightiga SSH kasutamine](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Kui ühendus on loodud, kasutage saada esmane headnode täielik domeeninimi järgmine käsk:

        hostname -f

    See toob nimi umbes järgmine:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    See on esmane headnode hostname, kus asub tooni veebisaidi.

2. Avage tooni portaali aadressil http://HOSTNAME:8888 brauseri abil. Asendage nimi hankisite eelmises etapis hostname (HOSTINIMI).

    > [AZURE.NOTE] Kui logite esimest korda, palutakse teil luua konto tooni portaali sisse logida. Siin saate määrata mandaadi piiratakse portaali ja administraatori või määratud ajal sätte klaster SSH kasutajatunnust pole seotud.

    ![Tooni portaali sisselogimine] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Määrake mandaat tooni portaal")

### <a name="run-a-hive-query"></a>Taru päringu käivitamine

1. Portaalist tooni, klõpsake **Päringu redaktorid**ja **taru** taru redaktori avamiseks klõpsake.

    ![Kasutage taru] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Kasutage taru")

2. Klõpsake menüü **abistamine** jaotises **andmebaasi**peaksite nägema **hivesampletable**. See on valimi tabeli, mis saadetakse kõik Hadoopi kogumite Hdinsightiga kohta. Sisestage päringu valimi parempoolsel paanil ja vaadata väljund vahekaardil **tulemuste** paanil all, nagu on näidatud ekraanipildi.

    ![Päringu käivitamine taru] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Päringu käivitamine taru")

    Saate menüü **Diagrammiriistad** kuvamiseks visuaalseks kujutamiseks tulemuse.

### <a name="browse-the-cluster-storage"></a>Sirvige salvestusruumi kobar

1. Tooni portaalis, klõpsake **Faili brauseri** paremas ülanurgas menüüriba.

2. Vaikimisi avaneb brauseri faili **/user/myuser** kataloogi. Klõpsake kasutaja kataloogi avamiseks Azure storage ümbris seostatud klaster juurkaustas tee enne kaldkriips.

    ![Kasutage faili brauseris] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Kasutage faili brauseris")

3. Paremklõpsake faili või kausta kuvamiseks toimingud. Paremas ülanurgas nupp **üleslaadimine** abil praegust kausta failide üleslaadimine. Nupp **Uus** abil saate luua uusi faile või kaustu.

> [AZURE.NOTE] Tooni faili brauseris saate kuvada ainult need vaikimisi ümbris Hdinsightiga kobar seotud sisu. Mis tahes täiendavat salvestusruumi kontod/ümbriste teil võib olla seotud klaster ei pääse faili brauseri kaudu. Täiendavad ümbriste, mis on seotud klaster on alati siiski taru töökohtade jaoks kättesaadavaks. Näiteks kui sisestate käsu `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` taru toimetaja, saate vaadata ka täiendavad ümbriste sisu. See käsk **newcontainer** pole vaikimisi ümbris seostatud klaster.

## <a name="important-considerations"></a>Oluline teave

1. Skripti tooni installimiseks kasutada installib see ainult esmane headnode, klaster.

2. Installimise ajal mitme Hadoopi teenuseid (HDFS, LÕNG, MR2, Oozie) uuesti värskendamise konfiguratsiooni. Pärast skripti installimise tooni, võib kuluda aega muude Hadoopi teenuste alustada. See võib mõjutada algselt tooni 's jõudlust. Pärast kõigi teenuste alustada, saab tooni töökorras.

3.  Tooni mõista Tez töö, mis on praeguse vaikimisi mesilaspere. Kui soovite kasutada mootori taru täitmise MapReduce, värskendada skript kasutamine oma skript järgmine käsk:

        set hive.execution.engine=mr;

4.  Linux kogumite, võib olla stsenaarium, kus teie teenused töötavad esmane headnode ajal ressursihaldur võiks teisese töötab. Sellisel juhul võib põhjustada tõrkeid (vt allpool) kasutamisel tooni klaster töötab töö üksikasjade kuvamiseks. Kui töö on valmis, saate siiski vaadata töö üksikasjad.

    ![Tooni portaali tõrge] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Tooni portaali tõrge")

    Selle põhjuseks teadaolev probleem. Lahenduseks muutmine nii, et aktiivne ressursihaldur töötab ka esmane headnode Ambari.

5.  Tooni mõistab WebHDFS ajal Hdinsightiga kogumite kasutada Azure Storage abil `wasbs://`. Nii, et kasutatud skripti toimingu kohandatud skript installib WebWasb, mis on WebHDFS ühilduv teenus WASB räägi. Jah, kuigi tooni portaali ütleb HDFS kohtades (nt kui viite kursori üle **Faili brauseris**), see tuleks tõlgendada WASB.


## <a name="next-steps"></a>Järgmised sammud

- [Installige Giraph Hdinsightiga kogumite kohta](hdinsight-hadoop-giraph-install-linux.md). Kobar kohandamine abil saate installida Giraph Hdinsightiga Hadoopi kogumite. Giraph võimaldab teil teha Graphi töötlemine Hadoopi abil ja seda saab kasutada Windows Azure Hdinsightiga.

- [Installige Solri Hdinsightiga kogumite kohta](hdinsight-hadoop-solr-install-linux.md). Kobar kohandamise abil saate installida Solri Hdinsightiga Hadoopi kogumite. Solri võimaldab salvestatud andmeid võimas otsing toiminguid.

- [Hdinsightiga kogumite R installida](hdinsight-hadoop-r-scripts-linux.md). R installimine Hdinsightiga Hadoopi kogumite kobar kohandamise abil. R on avatud lähtekoodi keel ja statistika arvutuste jaoks. Pakub sadu sisseehitatud statistikafunktsioonid ja oma programmeerimiskeel, mis ühendab objekti rakendusse ja funktsionaalne programmeerimine aspekte. See sisaldab ka olulisel graafiline võimalusi.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
