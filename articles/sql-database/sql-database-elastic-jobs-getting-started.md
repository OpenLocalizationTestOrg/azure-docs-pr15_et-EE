<properties
    pageTitle="Elastne andmebaasi töö alustamine"
    description="Kuidas kasutada elastne andmebaasi tööde haldamine"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Elastne andmebaasi töö alustamine

Azure'i SQL-andmebaasi elastne andmebaasi tööd (eelvaade) võimaldab töökindluse käivitada T-SQL-i skriptide ulatuvad mitme andmebaasi ajal automaatselt uuesti proovimist ning tagada lõpliku lõpetamise. Elastne andmebaasi töö funktsioonide kohta leiate lisateavet teemast [funktsioon lehe Ülevaade](sql-database-elastic-jobs-overview.md).

Selles teemas laiendab valimi leitud [elastne Andmebaasiriistad töötamise alustamine](sql-database-elastic-scale-get-started.md). Kui valmis, siis: saate teada, kuidas luua ja hallata tööd, mis on seotud andmebaase rühma haldamiseks. See ei pea selleks, et kasutada eeliseid elastne töökohtade elastne skaala tööriistu saate kasutada.

## <a name="prerequisites"></a>Eeltingimused

Laadige alla ja käivitage [elastne andmebaasi tööriistad valimi töötamise alustamine](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Luua mõne Kildu kaardi manager valimi rakenduse kasutamine

Siin loote Kildu kaardi halduri koos mitme shards, järgneb andmete lisamise shards sisse. Kui teil on juba loodud sharded andmeid neid shards, saate järgmised toimingud vahele jätta ja liikuda järgmise jaotise juurde.

1. Koostamine ja valimi **elastne Andmebaasiriistad alustamine** rakenduse käivitada. Juhis 7 [alla laadida ja käivitage rakendus valimi](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools)jaotises kuni järgige juhiseid. Lõpus juhisega 7, kuvatakse järgmine Käsuviip.

    ![Käsuviip][1]

2.  Käsuviiba aken, tippige "1" ja vajutage sisestusklahvi **Enter**. See loob on Kildu kaardi halduri ja liidab kaks shards serveriga. Tippige "3" ja vajutage sisestusklahvi **Enter**; Korrake seda toimingut neli korda. See lisab teie shards valimi andmeread.

3.  [Azure portaali](https://portal.azure.com) peaks kuva kolme uute andmebaaside v12 serverisse.

    ![Visual Studio kinnituse][2]

    Selles etapis loome kohandatud andmebaasi saidikogumi, mis kajastab Kildu kaardi kõik andmebaasid. See võimaldab meil luua ja käivitada tööd, mis üle shards uude tabelisse lisada.

Siin me tavaliselt looks Kildu kaardi target, cmdlet-käsu **New-AzureSqlJobTarget** abil. Kildu kaardi Manageri andmebaasi peab olema seatud andmebaasi sihtfail nimega ja seejärel teatud Kildu kaardi on määratud siht. Selle asemel me loetlemine server kõik andmebaasid ja lisada uue kohandatud saidikogumi, välja arvatud juhtslaidi andmebaasid.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Loob kohandatud saidikogumi ja lisab kõik andmebaasid server kohandatud saidikogumi target, välja arvatud juhtslaidi.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>T-SQL-i skripti täitmise üle andmebaaside loomine

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Töö käivitada skripti üle kohandatud jaotise andmebaaside loomine

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Töö käivitab 

Mõne olemasoleva töö saab kasutada järgmist PowerShelli skripti:

Värskendage järgmised muutuja kajastamiseks soovitud töö nime on täidetud.

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Üksiku täitmise tuua

Kasutada sama **Get-AzureSqlJobExecution** cmdlet koos parameetriga **IncludeChildren** lapse töö täitmised, ehk kindla riigi iga töö täitmise iga andmebaasist suunatud töö oleku vaatamine.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Vaata üle mitme töö täitmised olek

**Get-AzureSqlJobExecution** cmdlet-käsk on mitu valikuliste parameetrite, mida saab kasutada mitut töö täitmised, filtreeritud kaudu esitatud parameetrid kuvamiseks. Järgmine näitab mõningaid Get-AzureSqlJobExecution kasutamise võimalust:

Aktiivse ülemise taseme töö kõik täitmised tuua:

    Get-AzureSqlJobExecution

Saate tuua kõik täitmised ülemise taseme töö, sealhulgas passiivsed töö:

    Get-AzureSqlJobExecution -IncludeInactive

Saate tuua kõik lapse töö täitmised esitatud töö täitmise ID, sealhulgas passiivsed töö:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Kõik töö täitmised ajakava abil loodud tuua / töö kombinatsioon, sealhulgas passiivsed tööde haldamine:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Saate tuua määratud Kildu kaart, sh passiivne töö suunamise kõiki töid:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Saate tuua määratud kohandatud saidikogumi, sh passiivne töö suunamise kõiki töid:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Saate tuua töö tööülesande täitmised jooksul konkreetse töö täitmise loendit:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Saate tuua töö tööülesande täitmise üksikasjad:

Saab kasutada järgmist PowerShelli skripti töö ülesande täitmine, mis on eriti kasulik, kui silumine täitmise tõrkeid üksikasju soovite vaadata.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Saate tuua tõrkeid töö tööülesande täitmised sees

JobTaskExecution objekt sisaldab atribuut elutsükli tööülesande koos sõnumi atribuut. Kui töö ülesande täitmine nurjus, elutsükli atribuudi väärtuseks *nurjunud* ja sõnumi atribuudi väärtuseks tulemuseks erandi teade ja selle virnas. Kui tööd ei õnnestunud, on oluline, tööülesandeid, mis ei õnnestunud antud töö üksikasju soovite vaadata.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Töö täitmise lõpuleviimiseks ootamine

Oodake, kuni töö tööülesande lõpuleviimiseks saab kasutada järgmist PowerShelli skripti:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Kohandatud täitmise poliitika loomine

Elastne andmebaasi töö toetab luua kohandatud täitmise poliitikat, mida saab rakendada töö käivitamisel.
  
Täitmise poliitika lubamiseks praegu määratlemiseks tehke järgmist.

* Nimi: Identifikaator täitmise poliitika.
* Töö ajalõpp: Kogu aeg enne tööd, tühistatakse elastne andmebaasi töö.
* Algne uuesti intervalli: Intervall oodatakse enne esimest proovi uuesti.
* Suurim lubatud uuesti intervalli: Ots intervallide uuesti kasutada.
* Proovige intervall Backoff ruudu: Järgmise intervalli korduskatsed arvutamisel kasutatavate ruudu.  Kasutatakse järgmist valemit: (algse proovige intervall) * Math.pow ((intervall Backoff ruudu) (arv, korduvad katsed) - 2). 
* Lubatud katsete: Proovi uuesti maksimumarv proovib sees tööd teha.

Täitmise vaikepoliitika kasutab järgmised väärtused:

* Nimi: Vaikepoliitika täitmise
* Töö ajalõpp: 1 nädal
* Algne uuesti intervalli: 100 millisekundit.
* Suurim lubatud uuesti intervalli: 30 minutit.
* Proovige intervall ruudu: 2
* Suurim lubatud katsete: 2,147,483,647

Soovitud täitmise poliitika loomiseks tehke järgmist.

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Värskendage kohandatud täitmise poliitika

Värskendage soovitud täitmise poliitika värskendada.

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Katkestamine

Elastne andmebaasi töö toetab töökohtade tühistamise taotlused.  Loobumise taotluse praegu täidetakse töö tuvastab elastne andmebaasi töö proovib lõpetage töö.

On kaks võimalust, et elastne andmebaasi töö saab teha tühistamise:

1. Tühistamata praegu täidetavad tööülesanded: tühistamise tuvastamisel töötamise tööülesande praegu tühistamise proovitakse praegu täidesaatva proportsioonid tööülesande sees.  Näide: kui kaua töötab päringu pooleli kui tühistamise proovitakse kohale toimetada, on katse Tühista päring.
2. Tühistamine tööülesande korduskatsed: Tühistamise tuvastamisel enne tööülesande käivitatakse täitmiseks juhtelemendi jutulõnga juhtelemendi teemat vältida toimingu käivitamine ja deklareerida taotluse, nagu on tühistatud.

Töö tühistamise soovi ema töö tühistamise taotluse kehtima vanema töö ja kõik selle lapse tööde haldamine.
 
Loobumise taotluse esitamine **Peata-AzureSqlJobExecution** cmdlet-käsu kasutamine ja seadke parameetri **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Töö nimi ja selle töö ajaloo kustutamine

Elastne andmebaasi töö toetab asünkroonne kustutamine tööde haldamine. Töö saab märkida kustutamiseks ja süsteem kustutab töö ja oma töö ajalugu pärast töö kõik täitmised on töö lõpetanud. Süsteemi automaatselt ei Tühista aktiivse töö täitmised.  

Selle asemel tuleb kasutada Peata-AzureSqlJobExecution tühistada täitmised aktiivse töö.

Töö kustutamise käivitamiseks **Eemalda-AzureSqlJob** cmdlet-käsu kasutamine ja seadke parameetri **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Kohandatud andmebaasi sihtrakenduse loomine
Kohandatud andmebaasi sihtkohtade saate määratleda elastne andmebaasi töö, mida saab kasutada otse täitmise või lisamine kohandatud rühma. Kuna **elastne andmebaasi kaustu** pole veel otse toetatud API PowerShelli kaudu, loote lihtsalt kohandatud andmebaasi target ja kohandatud andmebaasi saidikogumi target, mis hõlmab pool kõik andmebaasid.

Järgmiste muutujate vastavalt soovile andmebaasi teave seadmiseks tehke järgmist.

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Kohandatud saidikogumi sihtrakenduse loomine
Saate määratleda kohandatud andmebaasi saidikogumi target üle mitme määratletud andmebaasi sihtkohtade käivitamise lubamiseks. Pärast andmebaasi rühma loomist andmebaaside võib olla seotud kohandatud saidikogumi sihtkohta.

Järgmiste muutujate kajastamiseks soovitud kohandatud saidikogumi target konfiguratsiooni seadmiseks tehke järgmist.

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Andmebaaside lisamine kohandatud andmebaasi saidikogumi sihtkoht

Andmebaasi sihtkohtade võib olla seostatud kohandatud andmebaasi saidikogumi sihtkohtade rühma andmebaaside loomiseks. Iga kord, kui tööd on loodud, et eesmärgid kohandatud andmebaasi saidikogumi target, laiendatakse suunatud andmebaasid seotud rühma täitmise ajal.

Teatud kohandatud saidikogumi soovitud andmebaasi lisamiseks tehke järgmist.

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Vaadake üle andmebaasi kohandatud saidikogumi target andmebaasidele

**Get-AzureSqlJobTarget** cmdlet-käsu abil saate tuua kohandatud andmebaasi saidikogumi target lapse andmebaasidele. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Töö, mida soovite käivitada skripti üle kohandatud andmebaasi saidikogumi sihtrakenduse loomine

**Uus-AzureSqlJob** cmdlet-käsu abil saate luua töö andmebaaside määratletud kohandatud andmebaasi saidikogumi target rühma vastu. Elastne andmebaasi töö on töö laieneda mitme lapse töö iga vastav andmebaasi seostatud kohandatud andmebaasi saidikogumi target ja veenduge, et käivitatakse skripti iga andmebaasist. Uuesti, on oluline, et skripte, idempotent olevat, et korduskatsed.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Kogu andmebaasi andmete kogumine

**Elastne andmebaasi töö** toetab päringu käivitamisel üle rühma andmebaasid ja saadab tulemi määratud andmebaasi tabelisse. Pärast iga andmebaasist päringu tulemuste vaatamiseks saate kasutataks tabel. See pakub päringu üle paljud andmebaasid on asünkroonne süsteem. Tõrge juhtumeid üks andmebaasid, on ajutiselt saadaval käsitletakse automaatselt korduskatsed kaudu.

Määratud sihttabel luuakse automaatselt, kui see pole veel olemas, sobitamine skeemi tagastatud tulemite hulk. Kui skripti täitmist tagastab tulemi mitmele kriteeriumikogumile, saata elastne andmebaasi töö ainult esimene esitatud sihttabel.

Järgmist PowerShelli skripti saab käivitada skripti kogumiseks tulemuste määratud tabelisse. See skript eeldatakse, et T-SQL-i skripti on loodud, mis väljundid ühe tulemite hulk ja kohandatud andmebaasi saidikogumi sihtrakenduse loomist.

Määrake soovitud skripti, mandaadi ja täitmise target järgmist:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Loomine ja andmete kogumise stsenaariumid töö alustamine
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Luua ajakava abil töö päästik töö täitmine

Järgmist PowerShelli skripti saab luua reoccurring ajakava. See skript kasutab ühe minuti järel, kuid New-AzureSqlJobSchedule toetab ka - DayInterval, - HourInterval, - MonthInterval, ja - WeekInterval parameetrid. Ajakava, et käivitada ainult üks kord saab luua mõõt läbides - ühekordselt.

Looge uus ajakava.

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Töö päästik töökohale täide ajakava loomine

Töö päästik saate määratleda olema täidetud vastavalt ajakava tööd. Töö päästik loomiseks saab kasutada järgmist PowerShelli skripti.

Järgmiste muutujate vastaksid soovitud töö ja ajakava seadmiseks tehke järgmist.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Ajastatud seos lõpetada töö käivitamisel ajakavas eemaldamine

Lõpetada reoccurring töö täitmise kaudu töö päästik, töö päästik saab eemaldada. Eemaldage töö päästik täitmist skeemi järgi **Eemalda-AzureSqlJobTrigger** cmdlet-käsu abil tööd lõpetada.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Exceli elastne andmebaasi päringutulemite importimine

 Saate importida Exceli faili päringu tulemid allikast.

1. Käivitage Excel 2013.
2.  **Andmete** lindil liikumine
3.  Nuppu **Muudest allikatest** ja klõpsake käsku **SQL Serverist**.

    ![Exceli importimine muudest allikatest][5]
4.  Tippige **Andmeühendusviisardis** serveri nimi ja sisselogimise identimisteave. Klõpsake nuppu **edasi**.
5.  Valige dialoogiboksis **Valige andmebaas, mis sisaldab andmeid, mida soovite**andmebaasi **ElasticDBQuery** .
6.  Valige tabel **Kliendid** loendivaates, ja klõpsake nuppu **edasi**. Klõpsake nuppu **valmis**.
7.  Vormi **Andmete importimine** jaotises **Valige, kuidas soovite oma töövihikus andmeid**, valige **Tabel** ja klõpsake nuppu **OK**.

Tabel **tellijad** talletatud erinevate shards kõiki ridu asustada Exceli leht.

## <a name="next-steps"></a>Järgmised sammud
Nüüd saate kasutada Exceli andmete funktsioone. BI ja andmete kataloogiintegreerimise tööriistad elastne päringu andmebaasiga ühenduse loomiseks kasutada oma serverinime, andmebaasi nimi ja mandaadi ühendusstring. Veenduge, et teie tööriista andmeallikana on toetatud SQL serveri. Vaadake elastne päringu andmebaas ja välise tabeleid, nagu SQL Serveri andmebaas ja SQL serveri tabelid, mida soovite ühendada oma tööriista.

### <a name="cost"></a>Maksumus
On täiendavaid tasuta elastne andmebaasi päringu funktsioonide kasutamiseks. Sel ajal see funktsioon on saadaval ainult premium andmebaaside nimega lõpp-punkt, kuid shards võib olla mis tahes teenuse kiht.

Täpsemat teavet hinna [SQL-i andmebaasi hinnad üksikasjad](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
