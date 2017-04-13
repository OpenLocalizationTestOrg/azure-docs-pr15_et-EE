<properties
    pageTitle="Soovitused, kasutades Mahout ja Windowsi-põhiste Hdinsighti luua | Microsoft Azure'i"
    description="Saate teada, kuidas Apache Mahout masina Õppekeskuse teegi abil saate luua filmi soovitused koos Windowsi-põhiste Hdinsightiga (Hadoopi)."
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
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Luua filmi soovitused kasutades Apache Mahout Hadoopi sisse Hdinsightiga

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Saate teada, kuidas õppimine raamatukogu koos Windows Azure Hdinsightiga [Apache Mahout](http://mahout.apache.org) seadme abil saate luua filmi soovitused.

> [AZURE.NOTE] Juhised selle dokumendi jaoks on vaja Windows klient ja Windowsi-põhiste Hdinsightiga kobar. Linux-põhine Hdinsightiga kobar Mahout Linux, OS X või Unix klient, kasutades leiate teemast [Genereeri filmi soovitused Apache Mahout koos Linux-põhine Hadoopi rakenduses Hdinsightiga abil](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Mida on õppida

Mahout on [masina Õppekeskuse] [ ml] Apache Hadoop teek. Mahout sisaldab algoritmide kohta andmete filtreerimine, liigitamine, nt rühmitamise töötlemiseks. Selles artiklis on soovitus Mootor abil luua filmi soovitused, mis põhinevad Filmid sõpradele näinud. Samuti saate teada, kuidas teha, mis otsust mets. See õpetab järgmist:

* Kuidas käivitada Mahout tööde haldamine Windows PowerShelli abil

* Kuidas käivitada Mahout töö Hadoopi käsurea kaudu

* Kuidas installida Mahout Hdinsightiga 3.0 ja Hdinsightiga 2.0 kogumite

    > [AZURE.NOTE] Mahout on esitatud Hdinsightiga 3,1 versiooni rühmad. Kui kasutate Hdinsightiga varasemas versioonis, lugege teemat [Mahout installida](#install) enne jätkata.

##<a name="prerequisites"></a>eeltingimused

- **An Windowsi-põhiste Hadoopi kobar Hdinsightiga sisse**. Üks loomise kohta leiate artiklist [rakenduses Hdinsightiga Hadoopi kasutamise alustamine][getstarted]
- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Soovitused luua Windows PowerShelli abil

> [AZURE.NOTE] Kuigi töö kasutada selles jaotises töötab Windows PowerShelli abil, paljude tunnid, mis on varustatud Mahout praegu ei tööta Windows PowerShelli ja need tuleb käitada Hadoopi käsurea kaudu. Tunnid, mis ei toimi Windows PowerShelli abil loendi leiate jaotisest [tõrkeotsing](#troubleshooting) .
>
> Näiteks Hadoopi käsurea kaudu käivitada Mahout tööd, vt [liigitab andmete Hadoopi käsurea kaudu](#classify).

Üks funktsioonid, mis on esitatud Mahout on soovitus mootor. See mootor aktsepteerib andmete vorming `userID`, `itemId`, ja `prefValue` (kasutajad eelistus üksus). Mahout saate teha koostööd sündmust analüüsi määratlemiseks: _kasutajad, kes on üksuse eelistust on ka muud nendeks eelistust_. Mahout seejärel määrab kasutajatele meeldib üksusega eelistused, mida saate kasutada soovitusi.

Järgnev on väga lihtne näide, mis kasutab filmid:

* __Kaasautorluse sündmust__: Joe, Alice ja Bob kõigi meeldinud _tähesõdade_, _Empire Strikes Back_ja _tagastada Jedi_. Mahout määratleb, et kasutajad, kes meeldib mõni järgmised Filmid ka nagu teised kaks.

* __Kaasautorluse sündmust__: Bob ja Alice meeldinud _Phantom Menace_, _rünnaku kloonide_ja _kätte maksta Sith_. Mahout määratleb, et kasutajad, kes meeldis eelmise kolm filmi samuti nagu need kolm.

* __Väga sarnased soovitus__: Kuna Joe meeldinud esimesed kolm Filmid Mahout suunatud filmid, et teised sarnane eelistused meeldinud, kuid Joe ei leidnud (meeldinud/hinnatud). Sel juhul soovitab Mahout _Phantom Menace_, _rünnaku kloonide_ja _kätte maksta Sith_.

###<a name="understanding-the-data"></a>Andmete mõistmine

Mugavalt, [GroupLens uurimistöö] [ movielens] annab Filmid vormingus, mis ühildub Mahout reiting andmeid. Andmed on saadaval teie kobar vaikimisi salvestusruum `/HdiSamples/MahoutMovieData`.

On kaks faili, `moviedb.txt` (filmid, teavet) ja `user-ratings.txt`. Kasutaja-ratings.txt faili kasutatakse analüüsimisel, samal ajal moviedb.txt kasutatakse kasutajasõbralik teksti juubelipidustuste analüüsi tulemuste kuvamisel.

Kasutaja-ratings.txt sisalduvate andmete on struktuur `userID`, `movieID`, `userRating`, ja `timestamp`, mis ütleb, kuidas iga kasutaja saanud filmi. Siin on näide andmeid:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Töö

Järgmine Windows PowerShelli skripti abil saate käivitada töö, mis kasutab Mahout soovitus Mootor filmi andmetega:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout töö Eemalda ajutised andmed, mis on loodud ajal töö. Funktsiooni `--tempDir` parameeter on määratud näide töö eristamiseks teatud kataloogi ajutised failid.

Töö Mahout ei tagasta väljund standardväljundit. Selle asemel see talletab selle väljundi määratud kataloogi __osa-r-00000__nimega. Skripti laadib __output.txt__ praegust kausta sisse oma töökoha seda faili.

Järgmises näites selle faili sisu:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Esimene veerg on soovitud `userID`. Olevaid väärtusi "[" ja "]" on `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Vaate väljund

Kuigi loodud väljund võib olla OK rakenduse kasutamiseks, pole väga loetav. Funktsiooni `moviedb.txt` serverist saab kasutada lahendamiseks soovitud `movieId` filmi nime, kuid te peate esmalt serverist alla laadida selle ja hinnangute faili järgmise skripti abil:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Kui failid on alla laaditud, kasutage järgmist PowerShelli skripti filmi nimedega soovituste kuvamiseks:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Järgmine on näide skripti:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Väljund peaks kuvatama umbes järgmine:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Andmete liigitada Hadoopi käsurea kaudu

Saadaval Mahout meetodite liigitamine on koostamiseks [juhusliku mets][forest]. See on mitu järgmist toimingut, mis hõlmab otsust puude, mida kasutatakse seejärel liigitada andmete loomiseks koolitus andmete kasutamine. See kasutab __org.apache.mahout.classifier.df.tools.Describe__ klassi Mahout järgi. See praegu tuleb käivitada Hadoopi käsurea kaudu.

###<a name="load-the-data"></a>Andmete laadimine

1. Järgmised failid alla laadida [The NSL-KDD andmehulgas](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. Hädaabiteabe](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): faili koolitus

  * [KDDTest +. Hädaabiteabe](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): testi andmed

2. Iga faili avada ja eemaldada jooned ülaosas, mis algavad '@', ja seejärel salvestage failid. Kui need on eemaldatud, saate tõrketeated Mahout andmed kasutamisel.

2. Laadige failid __üles/näidisandmetega__. Saate seda teha järgmist skripti abil. Asendage __CLUSTERNAME__ Hdinsightiga kobar nime. Asendage nimi FAILINIME küsiks faili üles laadida.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Töö

1. See töö nõuab Hadoopi käsurea. Luba kaugtöölaud Hdinsightiga kobar, siis selle ühenduse juures [Hdinsightiga kogumite abil RDP ühenduse](hdinsight-administer-use-management-portal.md#rdp)juhiste järgi.

3. Pärast ühenduse loomist Hadoopi käsurea avamiseks kasutada __Hadoopi käsurea__ ikoon:

    ![Hadoopi cli][hadoopcli]

3. Järgmise käsu abil saate luua faili deskriptori (__KDDTrain + .info__), mis kasutab Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    Funktsiooni `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` kirjeldab andmete faili atribuute. Näiteks L näitab silt.

4. Koostada mets otsust puude abil järgmine käsk:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Selle toimingu väljund on talletatud kataloogis __nsl – mets__ , mis asub veebisaidil __ wasbs://user/ klaster Hdinsightiga hoidmiseks&lt;kasutajanimi > / nsl mets/nsl forest.seq. Funktsiooni &lt;kasutajanimi > on kasutaja nimi, mida kasutasite kaugtöölaua seansi jaoks. See fail pole loetav inimestele.

5. Testige mets teel liigitamine __KDDTest + .arff__ andmekomplekti. Kasutage järgmine käsk:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    See käsk tagastab kokkuvõtva teabe liigitamine protsessi kohta järgmine:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Selle töö toodab ka failid, mis asuvad __wasbs:///example/data/predictions/KDDTest+.arff.out__. See fail pole loetav inimestele.

> [AZURE.NOTE] Mahout töökohtade ei kirjuta. Kui soovite need tööd uuesti käivitada, siis kustutage failid, mis on loodud eelmise töö.

##<a name="troubleshooting"></a>Tõrkeotsing

###<a name="install"></a>Installige Mahout

Mahout on installitud Hdinsightiga 3,1 kogumite ja seda saab installida käsitsi Hdinsightiga 3.0 või Hdinsightiga 2.1 kogumite, kasutades järgmist:

1. Mahout kasutada versiooni sõltub klaster Hdinsightiga versioon. Kobar versiooni leiate Azure'i portaalis klaster atribuutide vaatamine.

  * __Hdinsightiga 2.1__, saate alla laadida Java Archive (JAR) faili, mis sisaldab [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __Hdinsightiga 3.0__, tuleb [koostada Mahout lähtekoha] [ build] ja määrake Hadoopi versioon, mis on esitatud Hdinsightiga. Eeltingimused, mis on loetletud lehel Koosta installida, allikas ja Mahout jar failide loomine järgmise käsu abil:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Mahout 1.0 väljaandmisel peaks saama kasutada Varemkoostatud pakettide Hdinsightiga 3.0.

2. __Näide/purgid__ jaoks klaster laos vaikimisi jar faili üleslaadimiseks. Asendage CLUSTERNAME järgmist skripti klaster Hdinsightiga nime, ja faili nimi __mahout coure-0,9 job.jar__ faili tee.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Faile ei saa üle kirjutada

Mahout töö ära Puhasta ajutised failid, mis on loodud töötlemise ajal. Lisaks ei üle projektid olemasoleva väljundi faili.

Mahout tööde tõrgete vältimiseks kustutage ajutised ja väljund käivitatakse vahel või kasutada kordumatud väljund ja ajutine kaustanimed.

###<a name="cannot-find-the-jar-file"></a>Te ei leia JAR-faili

Hdinsightiga 3,1 kogumite kaasata Mahout. Tee ja faili nimi kaasata Mahout, kuhu on installitud klaster versiooninumber. Windows PowerShelli skripti näide selles õpetuses kasutab tee, mis on kehtiv alates November 2015, kuid versiooninumber muutub tulevikus värskenduste Hdinsightiga. Praeguse klaster Mahout JAR faili tee määratlemiseks kasutada Windows PowerShelli käsk ja seejärel muuta skripti viidata faili tee, mis tagastatakse:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Tunnid, mis ei toimi Windows PowerShelli abil

Mahout töö, mida kasutada järgmiste tagastavad mitmesuguseid tõrketeateid, kasutatakse Windows PowerShelli kaudu.

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Need tunnid kasutavate käivitamiseks ühenduse Hdinsightiga kobar ja käitamist Hadoopi käsurea kaudu. Vaadake [liigitab andmete Hadoopi käsurea](#classify) näide.

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas kasutada Mahout, vaadake muid võimalusi Hdinsightiga andmetega töötamine:

* [Taru koos Hdinsightiga](hdinsight-use-hive.md)
* [Siga Hdinsightiga](hdinsight-use-pig.md)
* [MapReduce koos Hdinsightiga](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
