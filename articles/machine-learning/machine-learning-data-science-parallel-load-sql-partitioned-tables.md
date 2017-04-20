<properties 
    pageTitle="Paralleelselt lahtise andmeid importida kasutades SQL partitsiooni tabelid | Microsoft Azure" 
    description="Paralleelselt lahtise andmeid importida kasutades SQL partitsiooni tabelid" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Paralleelselt lahtise andmeid importida kasutades SQL partitsiooni tabelid

Käesolev dokument kirjeldab kuidas ehitada sektsioonitud tabelid kiiresti paralleelselt lahtiselt importiva andmeid SQL Serveri andmebaasiga. Suur laadimise/andmeedastus SQL andmebaasi, andmete importimine SQL DB ja päringutes parandada kasutades _piiretega tabelid ja vaated_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Saate luua uue andmebaasi ja filegroups komplekt

- [Loo uus andmebaas](https://technet.microsoft.com/library/ms176061.aspx) (kui seda pole)
- Lisa andmebaasi filegroups andmebaasi, mis sektsioonitud füüsilised failid

  Märkus: Seda saab teha kui uut [Luua andmebaasi](https://technet.microsoft.com/library/ms176061.aspx) või [Muuda andmebaasi](https://msdn.microsoft.com/library/bb522682.aspx) kui andmebaasi on juba olemas

- Iga andmebaasi failirühma lisada ühe või mitu faili (vajadusel)

 > [AZURE.NOTE] Saate määrata eesmärgi failirühma, mis hoiab selle sektsiooni andmed ja füüsilise andmebaasi faili nimesid failirühma andmete talletamiseks.
 
Järgmine näide loob uue andmebaasi kolme peale esmase filegroups ja Logi rühma, mis sisaldab ühe füüsilise faili iga. Andmebaasi failid on loodud SQL serveri andmete vaikekausta konfigureeritud SQL serveri eksemplaris. Vt lisateavet faili vaikeasukohad [Asukohad vaikimisi ja nimega eksemplari SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Sektsioonitud tabeli loomine

Luua sektsioonitud tabeleid eelmises etapis loodud andmebaasi filegroups vastendatud andmed skeemi kohaselt. Kui andmed on lahtiselt imporditud sektsioonitud tabeleid, jaotatakse kirjete vahel filegroups sektsiooni kava vastavalt alltoodud viisil.

**Partitsiooni tabeli loomiseks pead:**

- [Loo partition funktsioon](https://msdn.microsoft.com/library/ms187802.aspx) , mis määratleb erinevaid väärtusi/piirid lisada iga üksiku partitsiooni tabeli, nt selleks, et piirata sektsioonid kuude kaupa (mõned\_datetime\_välja) aastal 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Loo sektsiooni kava](https://msdn.microsoft.com/library/ms179854.aspx) , mis vastendab iga partitsiooni partition funktsioon vahemiku füüsilise failirühma, nt:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Vihje: Vahemike tegelikult iga sektsiooni funktsiooni/süsteemi kohaselt kontrollida järgmise päringu käivitamiseks:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Loo sektsioonitud tabeli](https://msdn.microsoft.com/library/ms174979.aspx) s ostumeetod andmete skeemi, ja määrata partitsiooni tabeli, nt kasutatud partitsiooni kava ja piirangu välja:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Lisateabe saamiseks vt [luua piiretega tabelid ja indeksid](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Enamik iga üksiku partitsiooni tabeli andmete importimiseks

- Võite kasutada BCP, lahtiselt lisada või muid meetodeid, näiteks [SQL Server ränne viisard](http://sqlazuremw.codeplex.com/). Antud näites kasutab BCP meetodit.

- [Muuta andmebaasi](https://msdn.microsoft.com/library/bb522682.aspx) muuta kannete logimise kava BULK_LOGGED vähendada üldkulusid metsaraie, nt:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Et kiirendada andmete laadimist, käivitada samal ajal lahtiselt tegevus. Vt näpunäiteid kiirendada lahtiselt importiva andmeid SQL serveri andmebaasidega, [laadida alla 1 tunni 1 TB](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Järgmine PowerShelli skript on paralleelselt andmete laadimist, kasutades BCP näiteks.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Loo indeksid optimeerida ühendused ja töö

- Kui sa väljavõte mitmest tabelist andmete modelleerimine, Loo indeksid liitumise võtmed liitumise mõju parandamiseks.

- [Loo indeksid](https://technet.microsoft.com/library/ms188783.aspx) (rühmitatud või -rühmitatud) suunatud sama failirühma kõigi sektsioonide jaoks nt:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
või,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Võite luua enne andmete importimise lahtiselt indeksid. Registri loomine enne importimist lahtiselt aeglustab andmete laadimist.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Täiustatud Analytics protsessi ja tehnoloogia meetmeid näite

Otsast-otsani läbikäiguks näiteks avaliku DataSetti Cortana Analytics protsesside abil, vt [Cortana Analytics protsessi tegevuses: SQL serveri](machine-learning-data-science-process-sql-walkthrough.md).
 
