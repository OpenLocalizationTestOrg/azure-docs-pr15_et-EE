<properties 
    pageTitle="Luua ja hallata elastne andmebaasi PowerShelli abil" 
    description="PowerShelli abil saate hallata kaustu Azure SQL-andmebaas" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Saate luua ja hallata SQL-andmebaasi elastne andmebaasi töö abil PowerShelli (eelvaade)

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShelli](sql-database-elastic-jobs-powershell.md)



PowerShelli API **elastne andmebaasi töö** (eelvaade) võimaldavad teil andmebaase, mille skriptide käivitatakse rühma määramiseks. Selles artiklis kirjeldatakse, kuidas luua ja hallata **elastne andmebaasi töö** PowerShelli cmdlet-käskude abil. Vt [elastne töö ülevaade](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Eeltingimused
* Azure'i tellimuse. Tasuta prooviversiooni leiate [ühe kuu tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
* Elastne Andmebaasiriistad loodud andmebaasidele kogum. Vt [Alustamine elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md).
* Azure'i PowerShelli. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).
* **Elastne andmebaasi tööde haldamine** PowerShelli paketi: [installimist elastne andmebaasi projektide](sql-database-elastic-jobs-service-installation.md) kuvamiseks

### <a name="select-your-azure-subscription"></a>Valige Azure tellimuse

Valige tellimus, peate oma tellimuse Id (**-SubscriptionId**) või tellimuse nime (**-SubscriptionName**). Kui teil on mitu tellimust saate käivitage cmdlet-käsk **Get-AzureRmSubscription** ja kopeerige soovitud tellimuse teave tulemite hulk. Kui olete oma tellimuse andmed, käivitage järgmised abil selle tellimuse seada vaikeprinteriks ehk sihtrakenduse loomiseks ja haldamiseks tööde haldamine.

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) on soovitatav kasutus ja käivitada PowerShelli skriptide elastne andmebaasi töö suhtes.

## <a name="elastic-database-jobs-objects"></a>Elastne andmebaasiobjektide tööde haldamine

Järgmises tabelis on loetletud kõik objekti tüübid **elastne andmebaasi töö** koos selle kirjeldust ja oluline PowerShelli API-d.

<table style="width:100%">
  <tr>
    <th>Objekti tüüp</th>
    <th>Kirjeldus</th>
    <th>Seotud PowerShelli API-d.</th>
  </tr>
  <tr>
    <td>Mandaadi</td>
    <td>Kasutajanime ja parooli andmebaaside täitmiseks skriptide või DACPACs kohaldamine ühenduse loomisel kasutada. <p>Enne saatmist ning andmebaasis elastne andmebaasi töö säilitamiseks on krüptitud parool.  Parool on dekrüptida elastne andmebaasi töö teenuse identimisteavet, luua ja üles laadida installi skripti kaudu.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Uue AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skripti</td>
    <td>Transact-SQL-i skripti kasutatakse täitmise üle andmebaasid.  Skripti peaks Autor olevat idempotent, kuna teenuse proovib täitmise skripti tõrgete korral.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Uue AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Rakendus </a> paketi rakendamist kõikjal andmebaasid.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Uue AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Andmebaasi Target (sihtkoht)</td>
    <td>Andmebaasi ja serveri nimi, mis osutab Azure'i SQL-andmebaasi.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Uue AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Kildu kaardi Target (sihtkoht)</td>
    <td>Andmebaasi eesmärki ja salvestatud elastne andmebaasi Kildu vastenduse määratlemiseks kasutatakse mandaati kombinatsioon.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Uue AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Kohandatud saidikogumi Target (sihtkoht)</td>
    <td>Määratletud rühma andmebaaside, et täitmise ühiselt kasutada.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Uue AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Kohandatud saidikogumi tütarüksuste Target (sihtkoht)</td>
    <td>Andmebaasi target kohandatud kollektsioonist viidatud.</td>
    <td>
    <p>Lisage AzureSqlJobChildTarget</p>
    <p>Eemalda – AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Töö</td>
    <td>
    <p>Töö, mida saab kasutada, et täitmise käivitamine või ajakava täita parameetrite määratlemine.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Uue AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Töö täitmise</td>
    <td>
    <p>Container ülesanded, mida on vaja, et täitmise skripti või tõrkeid ja andmebaasi ühenduste mandaadi abil eesmärgiks on DACPAC rakendamine täita käsitleja vastavalt täitmise poliitika.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Algus-AzureSqlJobExecution</p>
    <p>Lõpeta – AzureSqlJobExecution</p>
    <p>Oodake-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Töö ülesande täitmine</td>
    <td>
    <p>Ühe üksuse töö töö täita.</p>
    <p>Kui töö tööülesande ei saa edukalt käivitada, logitakse tulemuseks erandi teade ja vastavaid töö uue ülesande luuakse ja vastavalt määratud täitmise korraga täidetud.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Algus-AzureSqlJobExecution</p>
    <p>Lõpeta – AzureSqlJobExecution</p>
    <p>Oodake-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Töö täitmise poliitika</td>
    <td>
    <p>Juhtelementide töökohtade täitmise ajalõpud, proovige uuesti piirangud ja intervallide korduste vahel.</p>
    <p>Elastne andmebaasi töö sisaldab vaikimisi töö täitmise poliitika, mis sisuliselt lõpmatu korduskatsed töö tööülesande tõrkeid põhjustada eksponentsiaalse backoff vahemiku vahel proovi uuesti.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Uue AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Ajakava</td>
    <td>
    <p>Kellaaja alusel määratlus reoccurring intervall või ühe korraga liikumisel teostatava toimingu täitmiseks.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Uue AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Töö käivitab</td>
    <td>
    <p>Vastenduse töö ja graafik päästik töö täitmise skeemi järgi.</p>
    </td>
    <td>
    <p>Uue AzureSqlJobTrigger</p>
    <p>Eemalda – AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Toetatud elastne andmebaasi töö rühmitamine tüübid
Töö aktiveeritakse Transact-SQL-i (T-SQL-i) skriptide või DACPACs rakendamine kogu rühmale andmebaasid. Kui töö käivitatakse üle rühma andmebaaside, töö "paisub" on tütarüksuste tööle, kus iga teeb taotletud täitmise ühe andmebaasi jaotises. 
 
On rühmad, mida saate luua kahte tüüpi: 

* [Kildu kaardi](sql-database-elastic-scale-shard-map-management.md) rühma: töö esitamisel suunata Kildu kaardi töö päringute Kildu kaardi määrata oma praeguse komplekti shards, ja seejärel loob lapse tööd iga Kildu Kildu kaart.
* Kohandatud saidikogumi rühma: kohandatud määratletud andmebaase. Kui töö suunatud kohandatud saidikogumi, loob see lapse töö iga andmebaasi jaoks praegu kohandatud saidikogumi.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Töö määramiseks elastne andmebaasi ühendus

Ühenduse peab olema seatud töökohtade *juhtelemendi andmebaasi* enne kasutades tööd API-d. Cmdlet-käsu käitamine käivitab mandaati akna hüpikaknas paluda kasutajanimi ja parool, mis on loodud elastne andmebaasi töö installimisel. Kõik selles teemas jooksul näited Oletame, et see esimene samm on juba tehtud.

Avage elastne andmebaasi töö-ühendus.

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Krüptitud mandaadid elastne andmebaasi tööde haldamine

Andmebaasi mandaadi saate lisada töökohtade *juhtelemendi andmebaasi* oma parooliga krüptitud. See on vaja mandaat lubamiseks töö hiljem, käivitatakse (abil töö ajakava).
 
Krüptimise töötab kaudu installimise skripti osana loodud sert. Installi skripti loob ja lisatud serdi Azure'i pilveteenuses talletatud krüptitud paroole dekrüptimine jaoks sisse. Azure'i pilveteenuses talletatud hiljem avalik võti töökohtade *juhtelemendi andmebaasi* , mis võimaldab PowerShelli API või Azure klassikaline portaali kasutajaliidese krüptimiseks esitatud parooli ei ole vaja serdi installimiseks kohalik.
 
Mandaadi paroolid on krüptitud ja turvaline kasutajatele lugemisõiguse elastne andmebaasiobjektide tööde haldamine. Kuid on võimalik, et lugemis-kirjutuspääs elastne andmebaasi töö objektidele pahatahtlik kasutaja parooli ekstrakti. Mandaat on mõeldud töö täitmised taaskasutada. Mandaadi edasi target andmebaaside määramisel ühendused. Praegu kasutatavad iga mandaadi target andmebaasid piiranguid, pahatahtlik kasutaja võib lisada andmebaasi target andmebaasi pahatahtlik kasutaja kontrolli all. Kasutaja võib hiljem alustada tööd suunamise selle andmebaasi saada soovitud mandaat parool.

Turvalisus head tavad elastne andmebaasi töö järgmised.

* Piirake API-de usaldusväärsete üksikisikutele kasutamist.
* Mandaadi peaks olema vähemalt töö toimingu sooritamiseks vajalikud õigused.  Lisateavet näha selle artikli SQL serveri MSDN-i [autoriseerimine ja õigused](https://msdn.microsoft.com/library/bb669084.aspx) .

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Kogu andmebaasid on krüptitud mandaadi töö täitmise loomiseks

Luua uue krüptitud identimisteavet, [**cmdlet-käsk Get-mandaati**](https://technet.microsoft.com/library/hh849815.aspx) palub sisestada kasutajanimi ja parool, mida saab edastada [**cmdlet-käsu New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Mandaadi värskendamine

Kui parooli muutmiseks kasutage [**cmdlet-käsu Set-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) ja **CredentialName** parameeter.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Elastne andmebaasi Kildu kaarti target määratlemine

Käivitada töö vastu kõik andmebaasid Kildu kogumi (loodud [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md)), kasutage andmebaasi target Kildu kaarti. Selles näites nõuab sharded rakendus, mis on loodud, kasutades elastne andmebaasi kliendi teek. Vaadake, [Alustamine elastne andmebaasi tööriistad valimi](sql-database-elastic-scale-get-started.md).

Kildu kaardi Manageri andmebaasi peab olema seatud andmebaasi sihtfail nimega ja seejärel teatud Kildu kaardi peab olema määratud siht.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>T-SQL-i skripti täitmise üle andmebaaside loomine

T-SQL-i skriptide täitmiseks loomisel on väga soovitatav koostamiseks, et need oleksid [idempotent](https://en.wikipedia.org/wiki/Idempotence) ja olles vastu ebaõnnestumist. Iga kord, kui täitmise tekib tõrge, olenemata selle liigitamine, proovib elastne andmebaasi töö täitmise skripti.

[**Cmdlet-käsu New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) abil saate luua ja salvestada, et täitmise skripti ja seadmine **– ContentName** ja **- CommandText** parameetrid.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Luua uue skripti lisamine failist
Kui T-SQL-skript on määratletud failis, kasutage seda importimiseks skripti:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>T-SQL-i skripti täitmise üle andmebaaside värskendamiseks  

See PowerShelli skripti värskendatakse mõne olemasoleva skripti T-SQL-i käsk tekst.

Järgmiste muutujate kajastamiseks soovitud skripti määratluse olema seatud seadmiseks tehke järgmist.

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Kui soovite mõne olemasoleva skripti definitsiooni värskendamiseks

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Töö, mida soovite käivitada skripti üle Kildu kaardi loomine

See PowerShelli skripti alustab tööd skripti täitmise üle iga Kildu vastenduse elastne skaala Kildu sisse.

Järgmiste muutujate soovitud skripti ja target seadmiseks tehke järgmist.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Töö käivitada 

See PowerShelli skripti käivitab mõne olemasoleva töö:

Värskendage järgmised muutuja kajastamiseks soovitud töö nime on täidetud.

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Üksiku täitmise toomiseks

Kasutage [**cmdlet-käsk Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) ja seadke parameetri **JobExecutionId** töö täitmise kuvamiseks.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Kasutada sama **Get-AzureSqlJobExecution** cmdlet koos parameetriga **IncludeChildren** lapse töö täitmised, ehk kindla riigi iga töö täitmise iga andmebaasist suunatud töö oleku vaatamine.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Vaadata üle mitme töö täitmised olek

[**Cmdlet-käsk Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) on mitu valikuliste parameetrite, mida saab kasutada mitut töö täitmised, filtreeritud kaudu esitatud parameetrid kuvamiseks. Järgmine näitab mõningaid Get-AzureSqlJobExecution kasutamise võimalust:

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

## <a name="to-retrieve-failures-within-job-task-executions"></a>Tuua tõrkeid töö tööülesande täitmised sees

**JobTaskExecution objekt** sisaldab atribuudi elutsükli tööülesande koos sõnumi atribuut. Kui töö ülesande täitmine nurjus, elutsükli atribuudi väärtuseks *nurjunud* ja sõnumi atribuudi väärtuseks tulemuseks erandi teade ja selle virnas. Kui tööd ei õnnestunud, on oluline, tööülesandeid, mis ei õnnestunud antud töö üksikasju soovite vaadata.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Töö täitmise lõpuleviimiseks ootama

Oodake, kuni tööülesandega lõpuleviimiseks saab kasutada järgmist PowerShelli skripti:

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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

1. Loobu praegu täidetavad tööülesanded: tühistamise tuvastamisel töötamise tööülesande praegu tühistamise proovitakse praegu täidesaatva proportsioonid tööülesande sees.  Näide: kui kaua töötab päringu pooleli kui tühistamise proovitakse kohale toimetada, on katse Tühista päring.
2. Tööülesande korduskatsed tühistamata: tühistamise tuvastamisel enne tööülesande käivitatakse täitmiseks juhtelemendi jutulõnga juhtelemendi teemat vältida toimingu käivitamine ja deklareerida taotluse, nagu on tühistatud.

Töö tühistamise soovi ema töö tühistamise taotluse kehtima vanema töö ja kõik selle lapse tööde haldamine.
 
Loobumise taotluse esitamiseks kasutada [**cmdlet-käsk Peata-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) ja seadke parameetri **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Töö kustutamine ja ametinimetuse asünkroonselt ajalugu

Elastne andmebaasi töö toetab asünkroonne kustutamine tööde haldamine. Töö saab märkida kustutamiseks ja süsteem kustutab töö ja oma töö ajalugu pärast töö kõik täitmised on töö lõpetanud. Süsteemi automaatselt ei Tühista aktiivse töö täitmised.  

Autonoomsest [**Peata-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) tühistada täitmised aktiivse töö.

Töö kustutamise käivitamiseks kasutada i [**cmdlet-käsk Eemalda-AzureSqlJob**](https://msdn.microsoft.com/library/mt346083.aspx) ja seadke **JobName** parameeter.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Kohandatud andmebaasi sihtrakenduse loomine

Saate määratleda kohandatud andmebaasi sihtkohtade otsese täitmise või lisamine kohandatud rühma. Näiteks, kuna **elastne andmebaasi kaustu** pole veel otse toetatud abil PowerShelli API-d, saate luua kohandatud andmebaasi target ja kohandatud andmebaasi saidikogumi target, mis hõlmab kõik andmebaasid pool.

Järgmiste muutujate vastavalt soovile andmebaasi teave seadmiseks tehke järgmist.

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Kohandatud saidikogumi sihtrakenduse loomine

[**Uus-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) cmdlet-käsu abil saate määratleda kohandatud andmebaasi saidikogumi target üle mitme määratletud andmebaasi sihtkohtade käivitamise lubamiseks. Pärast andmebaasi rühma loomist saab seostatud kohandatud saidikogumi target andmebaasid.

Järgmiste muutujate kajastamiseks soovitud kohandatud saidikogumi target konfiguratsiooni seadmiseks tehke järgmist.

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Kohandatud saidikogumi target andmebaaside lisamiseks

Teatud kohandatud saidikogumi andmebaasi lisamiseks kasutage [**Lisa-AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) cmdlet-käsk.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Vaadake üle andmebaasi kohandatud saidikogumi target andmebaasidele

[**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) cmdlet-käsu abil saate tuua kohandatud andmebaasi saidikogumi eesmärki tütarüksuste andmebaasidele. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Töö, mida soovite käivitada skripti üle kohandatud andmebaasi saidikogumi sihtrakenduse loomine

[**Uus-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) cmdlet-käsu abil saate luua töö andmebaaside määratletud kohandatud andmebaasi saidikogumi target rühma vastu. Elastne andmebaasi töö on töö laieneda mitme lapse töö iga vastav andmebaasi seostatud kohandatud andmebaasi saidikogumi target ja veenduge, et käivitatakse skripti iga andmebaasist. Uuesti, on oluline, et skripte, idempotent olevat, et korduskatsed.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Kogu andmebaasi andmete kogumine

Saate päringut üle rühma andmebaasid ja saata tulemid kindla tabeli tööd. Pärast iga andmebaasist päringu tulemuste vaatamiseks saate kasutataks tabel. See pakub asünkroonne sisestusmeetod päringu üle paljud andmebaasid. Nurjunud katsete käsitletakse automaatselt korduskatsed kaudu.

Määratud sihttabel luuakse automaatselt, kui see pole veel olemas. Uue tabeli vastab skeemi tagastatud tulemite hulk. Kui skript tagastab tulemi mitmele kriteeriumikogumile, saata elastne andmebaasi töö esimene ainult sihttabel.

Järgmist PowerShelli skripti käivitab skripti ja kogub tulemuste määratud tabelisse. See skript eeldab, et T-SQL-i skripti on loodud mis väljundid ühe tulemite hulk ja kohandatud andmebaasi saidikogumi eesmärki loodud.

See skript kasutab [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) cmdlet-käsk. Skripti, mandaadi ja täitmise target parameetrid seadmiseks tehke järgmist.

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Loomine ja andmete kogumise stsenaariumid töö alustamine

See skript kasutab [**Algus-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) cmdlet-käsk.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Töö täitmise päästik ajastamine

Luua korduva ajakava saab kasutada järgmist PowerShelli skripti. See skript kasutab minute intervall, kuid [**New-AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) toetab ka - DayInterval, - HourInterval, - MonthInterval, ja - WeekInterval parameetrid. Ajakava, et käivitada ainult üks kord saab luua näiva läbides - ühekordselt.

Looge uus ajakava.

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Töö ajakava täide käivitamiseks

Töö päästik saate määratleda olema täidetud vastavalt ajakava tööd. Töö päästik loomiseks saab kasutada järgmist PowerShelli skripti.

Kasutage [New-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) ja määrake järgmised muutujad vastaksid soovitud töö ja ajakava.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Ajastatud seos lõpetada töö käivitamisel ajakavas eemaldamine

Lõpetada reoccurring töö täitmise kaudu töö päästik, töö päästik saab eemaldada. Eemaldage töö päästik täitmist skeemi järgi [**Eemalda-AzureSqlJobTrigger cmdlet-käsu**](https://msdn.microsoft.com/library/mt346070.aspx)abil tööd lõpetada.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Saate tuua töö päästikute seotud loend

Järgmist PowerShelli skripti saab hankida ja töö päästikute, mis on registreeritud kindla ajakava kuvada.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Töö päästikute toomiseks seotud tööd 

[Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) abil saate hankida ja kuvada ajakavade, mis sisaldab registreeritud töö.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Soovitud rakendus (DACPAC) täitmiseks üle andmebaaside loomine

Mõne DACPAC loomiseks vaadake teemat [andmete taseme rakendused](https://msdn.microsoft.com/library/ee210546.aspx). Mõne DACPAC juurutamiseks kasutada [cmdlet-käsu New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx). Funktsiooni DACPAC peab olema teenusega. Soovitatav on loodud DACPAC üleslaadimine Azure Storage ja jaoks soovitud DACPAC [Ühiskasutusse Accessi allkirja](../storage/storage-dotnet-shared-access-signature-part-1.md) loomine.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Soovitud rakendus (DACPAC) täitmiseks üle andmebaaside värskendamine

Olemasoleva DACPACs registreeritud elastne andmebaasi töö saab värskendada, et käsk Uus URI-d. Kasutage värskendamiseks klõpsake mõne olemasoleva registreeritud DACPAC DACPAC URI [**Set-AzureSqlJobContentDefinition cmdlet-käsk**](https://msdn.microsoft.com/library/mt346074.aspx) :

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Töö, mida soovite rakendada soovitud rakendus (DACPAC) üle andmebaasi loomine

Pärast soovitud DACPAC loomist sees elastne andmebaasi töö, töö saab luua rakendamine soovitud DACPAC andmebaaside rühma. Kohandatud kogum andmebaaside DACPAC töö loomiseks saate kasutada järgmist PowerShelli skripti:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
