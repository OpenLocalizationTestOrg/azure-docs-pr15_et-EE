<properties
    pageTitle="Kasuta R Hdinsightiga kohandamiseks kogumite | Microsoft Azure'i"
    description="Saate teada, kuidas installida skripti toimingu abil R ja R kasutamine Hdinsightiga kogumite."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installimine ja kasutamine R Hdinsightiga Hadoopi kogumite

Siit saate teada, kuidas Windows kohandada vastavalt Hdinsightiga kobar skripti toimingu abil r ja R kasutamine Hdinsightiga kogumite. [Premium taseme](https://azure.microsoft.com/pricing/details/hdinsight/) Hdinsightiga pakkumine sisaldab R Server klaster Hdinsightiga osana. See võimaldab kasutada MapReduce ja säde jaotatud arvutuste R skriptide. Lisateabe saamiseks vt [R serveris Hdinsightiga kasutamise alustamine](hdinsight-hadoop-r-server-get-started.md). R Linux-põhine kobar kasutamise kohta leiate teemast [installi ja kasutage R Hdinsightiga Hadoopi kogumite (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
R mis tahes tüüpi kobar (Hadoopi, torm, HBase, säde) kohta saate installida Windows Azure Hdinsightiga *Skripti toimingu*abil. Proovi skripti installida R on Hdinsightiga kobar on kirjutuskaitstud salvestusruumi Azure'i bloobimälu [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)veebisaidil saadaval. 

**Seotud artiklid**

- [Installimine ja kasutamine R Hdinsightiga Hadoopi kogumite (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Rakenduses Hdinsightiga loomine Hadoopi kogumite](hdinsight-provision-clusters.md): üldist teavet Hdinsightiga kogumite loomise kohta
- [Kohandada Hdinsightiga kobar skripti toimingu abil][hdinsight-cluster-customize]: Üldteave kohandamiseks Hdinsightiga kogumite skripti toimingu abil
- [Töötada skript toimingu skriptide Hdinsightiga](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Mis on R?

<a href="http://www.r-project.org/" target="_blank">R projekti statistika arvutuste jaoks</a> on avatud allikas keel ja statistika arvutuste jaoks. R leiate sadu sisseehitatud statistikafunktsioonid ja oma programmeerimiskeel, mis ühendab objekti rakendusse ja funktsionaalne programmeerimise aspekte. See sisaldab ka olulisel graafiline võimalusi. R on eelistatud programmeerimise keskkonna kõige professionaalne statistikute ja teadlased erinevaid väljad.

R on ühilduvad koos Azure'i bloobimälu salvestusruumi (WASB) nii, et seal talletatakse andmeid saab töödelda R kasutamine Hdinsightiga.  

## <a name="install-r"></a>Installi R

[Skripti näide](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) installida R on Hdinsightiga kobar on kirjutuskaitstud bloobimälu, Azure Storage saadaval. Sellest jaotisest leiate juhised, kuidas kasutada valimi skripti Azure'i portaalis klaster loomisel.

> [AZURE.NOTE] Skripti näide võeti kasutusele 3,1 Hdinsightiga kobar versiooniga. Hdinsightiga kobar versioonide kohta leiate lisateavet teemast [Hdinsightiga kobar versioonid](hdinsight-component-versioning.md).

1. Portaalist on Hdinsightiga kobar loomisel klõpsake nuppu **Valikuline konfigureerimine**ja klõpsake **Skripti toimingud**.
2. Sisestage lehel **Script toimingud** järgmised väärtused:

    ![Kasutage skripti toimingu kohandamiseks on kobar] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Kasutage skripti toimingu kohandamiseks on kobar")

    <table border='1'>
        <tr><th>Atribuut</th><th>Väärtus</th></tr>
        <tr><td>Nimi</td>
            <td>Määrake skripti toimingut, näiteks <b>Installida R</b>nimi.</td></tr>
        <tr><td>Skripti URI</td>
            <td>Määrake URI skripti, mida järgida kobar, näiteks <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i> kohandamiseks</td></tr>
        <tr><td>Sõlme tüüp</td>
            <td>Määrake sõlmed, mille kohandamine skript käivitatakse. Saate valida <b>Kõik sõlmed</b>, <b>juht sõlmed ainult</b>või <b>töötaja sõlmed</b> ainult.
        <tr><td>Parameetrid</td>
            <td>Määrake soovitud parameetrid skripti vajadusel. Siiski ei nõua skripti R installimiseks soovitud parameetrid nii, et võite seda tühjaks jätta.</td></tr>
    </table>

    Saate lisada rohkem kui ühe skripti toimingu klaster mitme komponentide installimine. Kui olete lisanud skriptide, seejärel klõpsake märkeruutu kastidesse klaster käivitamiseks.

Skripti abil installida R Hdinsightiga Azure PowerShelli või Hdinsightiga .NET SDK abil. Selle artikli on esitatud juhised järgmiste toimingute jaoks.

## <a name="run-r-scripts"></a>R skriptide käitamiseks
Selles jaotises kirjeldatakse, kuidas käivitada mõne R Hadoopi klaster koos Hdinsightiga.

1. **Kaugtöölaua ühendus klaster looma**: portaali kaudu kaugtöölaua lubamine klaster loodud r installitud, ja seejärel ühendada klaster. Juhised leiate teemast [Hdinsightiga kogumite abil RDP ühenduse loomine](hdinsight-administer-use-management-portal.md#rdp).

2. **Avatud R konsooli**: The R installi paneb lingi R konsooli pea sõlme töölaual. Klõpsake seda avamiseks R konsooli.

3. **R skripti**: The R skripti saab käivitada otse R konsooli kleepides see, valige see ja vajutage sisestusklahvi ENTER. Siin on lihtne näide skripti, mis loob arvude 1 kuni 100 ning seejärel korrutab need arvuga 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Kaks esimest rida kõne RHadoop teegid, mis installitakse koos R. Viimase rea prinditakse konsooli tulemid. Väljund peaks välja nägema umbes järgmine:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>R Aure PowerShelli kaudu installida

Lugege teemat [kohandamine Hdinsightiga kogumite skript toimingu abil](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Valimi näitab, kuidas installida Azure PowerShelli kaudu säde. Peate kohandada skripti [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)kasutama.

## <a name="install-r-using-net-sdk"></a>R, kasutades .NET SDK installimine

Lugege teemat [kohandamine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Valimi näitab, kuidas installida säde .NET SDK abil. Peate kohandada skripti [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)kasutama.


## <a name="see-also"></a>Vt ka

- [Installimine ja kasutamine R Hdinsightiga Hadoopi kogumite (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Rakenduses Hdinsightiga loomine Hadoopi kogumite](hdinsight-provision-clusters.md): üldist teavet Hdinsightiga kogumite loomise kohta
- [Kohandada Hdinsightiga kobar skript toimingu abil][hdinsight-cluster-customize]: Üldteave kohandamise Hdinsightiga kogumite skript toimingu abil
- [Töötada skripti toimingu skriptide Hdinsightiga](hdinsight-hadoop-script-actions.md)
- [Installimine ja kasutamine säde Hdinsightiga kogumite][hdinsight-install-spark]: skripti toimingu valimi säde installimise kohta
- [Installige Giraph Hdinsightiga kogumite kohta](hdinsight-hadoop-giraph-install.md): skripti toimingu valimi Giraph installimise kohta
- [Installige Solri Hdinsightiga kogumite kohta](hdinsight-hadoop-solr-install-linux.md): skripti toimingu valimi Solri installimise kohta.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
