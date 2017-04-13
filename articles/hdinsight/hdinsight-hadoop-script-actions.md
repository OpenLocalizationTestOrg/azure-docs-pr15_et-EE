<properties
    pageTitle="Skript toimingu arendamiseni Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas kohandada Hadoopi kogumite skripti toiminguga. Skripti toimingu saab töötavate Hadoopi kobar täiendavat tarkvara installimiseks või muuta konfiguratsiooni klaster installitud rakendused."
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

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Skripti toimingu skriptid Hdinsightiga Windowsi-põhiste kogumite töötada

Siit saate teada, kuidas kirjutada skripti toimingu skriptide Hdinsightile. Skripti toimingu skriptide kasutamise kohta leiate teemast [kohandamine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster.md). Linux-põhine Hdinsightiga kogumite kirjutatud sama artikli teemast [arendamise skripti toimingu skriptid Hdinsightile](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Teave selles dokumendis on Windowsi-põhiste Hdinsightiga kogumite. Kasutades Windowsi-põhiste kogumite skripti toimingud leiate teemast [skripti toimingu arendamiseni Hdinsightiga (Linux)](hdinsight-hadoop-script-actions-linux.md).


Skripti toimingu saab töötavate Hadoopi kobar täiendavat tarkvara installimiseks või muuta konfiguratsiooni klaster installitud rakendused. Skripti toimingud on skriptide kobar sõlmed töötavad Hdinsightiga kogumite juurutamisel ja nad on käivitada, kui sõlmed klaster Hdinsightiga konfigureerimise lõpuleviimine. Skripti toiming käivitatakse jaotises süsteem administraatoriõigused konto ja pakub täielikud juurdepääsuõigused kobar sõlmi. Iga kobar saate anda skripti toimingute täitmiseks on määratud järjestuses loend.

> [AZURE.NOTE] Kui teil tekib kuvatakse järgmine tõrketeade:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Sellepärast, et te ei kaasata helper meetodid.  Lugege teemat [kohandatud skriptide Helper meetodid](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Näidisskriptid

Windowsi operatsioonisüsteemi Hdinsightiga kogumite loomist on skripti toimingu Azure PowerShelli skripti. Järgmine on näide skripti konfigureerimine saidi failid:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Skripti võtab neli parameetrit, konfiguratsiooni faili nime, atribuut, mida soovite muuta, on väärtus, mida soovite seada, ja kirjeldus. Näiteks:

    hive-site.xml hive.metastore.client.socket.timeout 90

Järgmiste parameetrite seab hive.metastore.client.socket.timeout väärtus 90 taru-site.xml faili.  Vaikeväärtus on 60 sekundi järel.

See näide skript leiate ka [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

Hdinsightiga pakub mitme skriptide Hdinsightiga kogumite lisakomponentide installida.

Nimi | Skripti
----- | -----
**Installige säde** | https://hdiconfigactions.blob.Core.Windows.net/sparkconfigactionv03/Spark-Installer-V03.ps1. [Installimine ja kasutamine säde Hdinsightiga kogumite kohta]vt[hdinsight-install-spark].
**Installi R** | https://hdiconfigactions.blob.Core.Windows.net/rconfigactionv02/r-Installer-v02.ps1. Vt [installi ja kasutage R Hdinsightiga kogumite][hdinsight-r-scripts].
**Installige Solri** | https://hdiconfigactions.blob.Core.Windows.net/solrconfigactionv01/Solr-Installer-V01.ps1. Vt [installimine ja kasutamine kogumite Solri Hdinsightiga kohta](hdinsight-hadoop-solr-install.md).
- **Installige Giraph** | https://hdiconfigactions.blob.Core.Windows.net/giraphconfigactionv01/giraph-Installer-V01.ps1. Vt [installimine ja kasutamine kogumite Giraph Hdinsightiga kohta](hdinsight-hadoop-giraph-install.md).

Skripti toimingu saab juurutada Azure portaalis Azure PowerShelli või Hdinsightiga .NET SDK abil.  Lisateabe saamiseks lugege teemat [kohandamine Hdinsightiga kogumite skripti toimingu abil][hdinsight-cluster-customize].

> [AZURE.NOTE] Funktsiooni näidisskriptid töötavad ainult versiooniga Hdinsightiga kobar 3,1 või kohale. Hdinsightiga kobar versioonide kohta leiate lisateavet teemast [Hdinsightiga kobar versioonid](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Kohandatud skriptid Helper meetodid

Skripti toimingu helper meetodid on Utiliidid, mida saate kasutada kohandatud skriptide kirjutamise ajal. Need on määratletud [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)ja saab lisada oma skriptide abil järgmist:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Järgnevalt on toodud helper meetodid, mis on esitatud selle skripti:

Helper meetod | Kirjeldus
-------------- | -----------
**Salvesta-HDIFile** | Faili alla laadida, on määratud ühtse identifikaator (URL) kohalikule kettale, mis on seotud Azure VM sõlme klaster määratud asukohta.
**Laiendage HDIZippedFile** | Pakkige see lahti ZIP faili.
**Autonoomsest HDICmdScript** | Käivitage skripti cmd.exe.
**Kirjutage-HDILog** | Kirjutage väljundi skripti toimingu jaoks kasutatava kohandatud käsikirjaga.
**Get-teenused** | Siin on töötab arvutis, kus skripti täidab teenuste loend.
**Get-teenus** | Kindla teenuse nimega sisendina, saada üksikasjalikku teavet kindla teenuse (teenuse nimi, protsessi ID, state jne) arvutisse, kus skripti käivitab.
**Get-HDIServices** | Siin on loend Hdinsightiga teenused töötavad arvutis, kus skripti täidab.
**Get-HDIService** | Kindla teenuse Hdinsightiga nimega sisendina, saada üksikasjalikku teavet kindla teenuse (teenuse nimi, protsessi ID, state jne) arvutisse, kus skripti käivitab.
**Get-ServicesRunning** | Teenused, mis töötavad arvutis, kus skripti täidab loendi hankimine.
**Get-ServiceRunning** | Märkige ruut, kui kindla teenuse (nime järgi) töötab arvutis, kus skripti käivitab.
**Get-HDIServicesRunning** | Siin on loend Hdinsightiga teenused töötavad arvutis, kus skripti täidab.
**Get-HDIServiceRunning** | Märkige ruut, kui Hdinsightiga kindla teenuse (nime järgi) töötab arvutis, kus skripti käivitab.
**Get-HDIHadoopVersion** | Hadoopi, kus skripti täidab arvutisse installitud versiooni hankimine.
**Test-IsHDIHeadNode** | Kontrollige, kas arvuti, kuhu skripti käivitab on pea sõlme.
**Test-IsActiveHDIHeadNode** | Kontrollige, kas arvuti, kuhu skripti käivitab on aktiivse pea sõlme.
**Test-IsHDIDataNode** | Kontrollige, kas arvuti, kuhu skripti käivitab on andmete sõlm.
**Redigeeri-HDIConfigFile** | Redigeeri config failid taru-site.xml core-site.xml hdfs-site.xml, mapred-site.xml või lõng-site.xml.


## <a name="best-practices-for-script-development"></a>Skripti arengu head tavad

Kui teil tekib mõne Hdinsightiga kobar kohandatud skript, on mitu head tavad meeles pidada.

- Hadoopi versiooni kontrollimine

    Hdinsightiga versioon 3,1 (Hadoopi 2.4) ja tugiteenuste skripti toimingu abil saate installida kohandatud komponendid klaster kohal. Oma kohandatud skripti, peate kasutama **Get-HDIHadoopVersion** helper meetodit Hadoopi versiooni enne jätkamist muude ülesannetega skripti.


- Skripti ressursid ühed linke

    Kasutajate veenduge, et kõik skripte ja muud esemeid kasutatud klaster kohandus jäävad klaster eluea ja nende failide versioone ei muuda kestel. Need ressursid on nõutav, kui sõlmed klaster uuesti Imagingi on nõutav. Parim on alla laadida ja arhiivi kõik salvestusruumi konto, mis määrab kasutaja. See võib olla salvestusruumi vaikekonto või mis tahes täiendava salvestusruumi kontod määratud kohandatud kobar kasutamise ajal.
    Säde ja R kohandatud kobar näidised antud dokumentides, näiteks oleme teinud kohaliku eksemplari ressursside selle salvestusruumi konto: https://hdiconfigactions.blob.core.windows.net/.


- Veenduge, et kobar kohandamine script on idempotent

    Te peate oota, et mõne Hdinsightiga kobar sõlmed uuesti imaged kobar elu jooksul. Kobar kohandamine skript käivitatakse iga kord, kui klaster on uuesti imaged. See skript peab olla kujundatud selliselt, et pärast uuesti Imagingi, skripti peaks klaster tagastamise kindlustamiseks sama idempotent kohandatud märkida, et see on ainult siis, kui skripti parandusfunktsiooni klaster algselt loomise esimest korda. Näiteks kui kohandatud skript installitud taotluse D:\AppLocation esimese käivitada, siis iga järgmise jooksma, pärast uuesti Imagingi, tuleb skripti vaadata, kas rakendus on olemas D:\AppLocation asukoht enne jätkamist muud etappide skripti.


- Optimaalse asukoha kohandatud komponentide installimine

    Kui kobar sõlmed on uuesti imaged, C:\ ressursi ketas ja D:\ süsteemi ketas saab uuesti vormindatud, mille tulemuseks on andmed ja rakendused, mis oli installitud need draivid. See võib juhtuda ka siis, kui Azure virtuaalse masina (VM) sõlm, mis on osa klaster läheb alla ja uue sõlme asendatakse. Saate installida komponendid D:\ draivil või klaster C:\apps asukohta. Muudesse asukohtadesse kettadraivi C:\ on kaitstud. Määrake asukoht, kus rakendused või teegid on olema installitud ja kobar kohandamine skripti.


- Tagada kõrge kobar arhitektuur

    Hdinsightiga on aktiivne passiivne arhitektuur kõrge-saadavus, kus üks pea sõlme on aktiivse režiimi (kus Hdinsightiga teenused töötavad) ja pea sõlme on (mis Hdinsightiga sisse teenuste ei tööta) puhkerežiimis. Sõlmed režiimide aktiivne ja passiivne kui Hdinsightiga katkestada. Kui skripti toimingut kasutatakse installitoimingut nii pea sõlmed jaoks kõrge-saadavus, Pange tähele, et Hdinsightiga Tõrkesiirde süsteemi ei saa kasutada automaatselt need kasutaja installitud teenused. Nii kasutaja installitud teenused Hdinsightiga pea sõlmed, mis eeldatakse, et väga kättesaadav peate olema oma Tõrkesiirde süsteem kui režiimis aktiivne passiivne või olema aktiivne aktiivne režiimis.

    Käsu käivitava Hdinsightiga skripti toimingu töötab nii pea sõlmed juht-sõlm rolli määratud *ClusterRoleCollection* parameetri väärtuse. Nii kui loote kohandatud skript, veenduge, et mis skripti andmetel selle häälestus. Ei peaks tekib probleeme, kus sama teenused on installitud ja alustada nii pea sõlmed ja nad lõpuks üksteisega. Arvestage ka, et andmed lähevad kaotsi ajal uuesti Imagingi, nii, et toimingu skripti kaudu installitud tarkvara peab olema olles selliste sündmuste. Rakenduste kavandada levitatakse üle palju sõlmi tugevalt saadaval andmetega töötamiseks. Pange tähele, et nii palju 1/5 sõlmed klaster saab uuesti imaged samal ajal.


- Azure'i bloobimälu kasutada kohandatud komponentide konfigureerimine

    Kohandatud komponendid, mille saate installida kobar sõlmed võib olla vaikimisi konfiguratsioon kasutamiseks Hadoopi jaotatud faili süsteemi (HDFS) salvestusruumi. Teil on vaja muuta konfiguratsioon, kasutage selle asemel Azure'i bloobimälu. Klõpsake soovitud klaster uuesti piltide, HDFS failisüsteemi saab vormindatud ja kaotate andmed, mida talletatakse seal. Azure'i bloobimälu abil hoopis tagab andmete säilitatakse.

## <a name="common-usage-patterns"></a>Levinud mustreid

Selles jaotises antakse juhiseid mõned levinud kasutus mustreid, mis võivad ilmneda kirjutamiseks oma kohandatud skript rakendamise kohta.

### <a name="configure-environment-variables"></a>Keskkonna muutujaid konfigureerimine

Sageli skripti toimingu arendatakse, tunnete tuleb keskkonna muutujad. Näiteks on kõige tõenäolisemad stsenaarium, kui välisel saidil on kahendarvuks allalaadimine, installimine klaster ja lisada asukohta, kuhu see on installitud oma "PATH" muutuja. Järgmised koodilõigu näitab, kuidas määrata kohandatud skript keskkonna muutujate.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

See lause seab keskkonna muutuja **MDS_RUNNER_CUSTOM_CLUSTER** väärtuse "tõene" ja ka määrab muutuja kogu arvutis olevat ulatust. Mõnikord on oluline, et keskkonna muutujate seatakse sobiv ulatus – arvuti või kasutaja. Vaadake [siin] [ 1] keskkonna muutujate seadmise kohta lisateabe saamiseks.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Accessi asukohta, kuhu on talletatud kohandatud skriptid

Kas peab skriptide abil klaster olema klaster salvestusruumi vaikekonto või avaliku kirjutuskaitstud container storage konto. Kui teie script pääseb ressursse, mis asub mujal need peavad olema avalikult juurdepääsetava (vähemalt avaliku kirjutuskaitstud). Näiteks võite faili juurde ja salvestada selle käsu SaveFile-HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

Selles näites peab veenduge, et ümbris 'somecontainer' salvestusruumi konto 'somestorageaccount' on avalikult kättesaadav. Muul juhul skripti põhjustada ei leitud erand ja nurjuda.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Lisa-AzureRmHDInsightScriptAction cmdlet-käsk edasi parameetrid

Mitme parameetrite edastamiseks cmdlet-käsu Lisa-AzureRmHDInsightScriptAction peate stringi väärtuse sisaldavad kõik parameetrid skripti vormindamine. Näiteks:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

või

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Põhjustada erandi juurutamiseks nurjunud kobar

Kui soovite, et saada täpselt teada et kohandamine klaster ei õnnestunud oodatud viisil, on oluline põhjustada erandi ja kobar loomine nurjub. Näiteks võite töötlemine faili, kui see on olemas ja käsitlemiseks veaväärtuse juhul, kui faili ei ole olemas. See oleks veenduge, et skripti suletakse pehmelt ja klaster olek on õigesti teada. Järgmised koodilõigu annab selleks näide:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

See koodilõigu, kui faili ei ole, see oleks viia riigis skripti tegelikult suletakse pehmelt pärast printimist kuvatakse tõrketeade ja klaster jõuab töötava eeldades, et see "" lõpule kobar kohandamise protsessi. Kui soovite, et kohandamine klaster sisuliselt täpselt teavitama ei õnnestunud tõttu kadunud faili oodatud viisil, on vaja põhjustada erandi ja nurjuda kobar kohandamine etappi. Selleks kasutage järgmist valimi koodilõigu hoopis.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Skripti toimingu juurutamine kontroll
Siin on toimingud, me võttis juurutamise nende skriptide ettevalmistamisel.

1. Pange failid, mis sisaldavad kohandatud skriptide kohas, kus on juurdepääsetav kobar sõlmed käigus juurutamist. See võib olla mis tahes vaike- või täiendava salvestusruumi kontod määratud kobar juurutamine või muude avalikult juurdepääsetava salvestusruumi container ajal.
2. Veenduge, et nad käivitada idempotently, nii, et skripti saab teostada mitu korda sama sõlme skriptide kontrolli lisada.
3. **Kirjutamine-väljund** Azure PowerShelli cmdlet-käsu abil saate printida STDOUT kui ka STDERR. Ärge kasutage **Kirjutamine-Host**.
4. Ajutiste failide kausta, nt $env kasutamine: TEMP säilitada kasutatavaid skriptide allalaaditud faili ja seejärel puhastada need pärast skriptide on täidetud.
5. Installige kohandatud tarkvara ainult D:\ või C:\apps. Muude asukohtade c-ketas ei tohi kasutada, kui need on reserveeritud. Pange tähele, et c-ketas väljaspool C:\apps kausta failide installimist võib kaasa tuua häälestuse nurjumine ajal uuesti pilte sõlme.
6. Muudetud OS taseme sätted või Hadoopi teenuse konfiguratsiooni faile, kui soovite taaskäivitage Hdinsightiga teenuseid, et nad saavad kättesaamine mis tahes OS taseme sätteid, näiteks keskkonna muutujate skriptide määramine.

## <a name="debug-custom-scripts"></a>Kohandatud skriptide silumine

Skripti Tõrkelogide talletatakse koos muude väljund kobar selle loomisel määratud vaikekonto salvestusruumi. Tabeli nimi on talletatud logid *u < \cluster-name-fragment >< \time-stamp > setuplog*. Need on liidetud logisid, mis on kõik sõlmed (pea sõlme ja töötaja sõlmed) skripti töötab klaster kirjeid.
Lihtne viis kontrollida logid on kasutada Hdinsightiga tööriistad Visual Studio. Tööriistade installimist vaadake [Visual Studio Hadoopi tööriistad Hdinsightiga kasutamise alustamine](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**Logi Visual Studio abil kontrollida**

1. Avage Visual Studio.
2. Klõpsake menüüd **Vaade**ja seejärel klõpsake nuppu **Server Explorer**.
3. Paremklõpsake "Azure", klõpsake nuppu Loo ühendus **Microsoft Azure'i tellimused**ja seejärel sisestage oma kasutajanimi ja parool.
4. Laiendage **salvestusruumi**, laiendage failisüsteemi kasutada Azure storage konto, laiendamine **tabelid**ja topeltklõpsake tabeli nime.


Saate teha ka remote kobar sõlmed vaatamiseks nii STDOUT ja STDERR jaoks kohandatud skriptid. Iga sõlme logid kehtivad ainult sõlme ja olete sisse logitud rakendusse **C:\HDInsightLogs\DeploymentAgent.log**. Need logifailid kirje kõik väljundid kohandatud käsikirjaga. Mõni näide log koodilõigu säde skripti toimingu näeb välja umbes järgmine:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


See logi on selge, et säde skripti toimingu käivitatud nimega HEADNODE0 VM ja et täitmise ajal visati erandid.

Täitmise tõrge ilmneb juhul, kui ka sisalduma väljundi, mis kirjeldab seda logifail. Need logid teavet peaks olema kasulik silumine skripti probleeme, mis võivad tekkida.


## <a name="see-also"></a>Vt ka

- [Kohandada HDInsight kogumite skripti toimingu abil][hdinsight-cluster-customize]
- [Installimine ja kasutamine säde Hdinsightiga kogumite][hdinsight-install-spark]
- [Installimine ja kasutamine R Hdinsightiga kogumite][hdinsight-r-scripts]
- [Installimine ja kasutamine kogumite Solri Hdinsightiga kohta](hdinsight-hadoop-solr-install.md).
- [Installimine ja kasutamine kogumite Giraph Hdinsightiga kohta](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
