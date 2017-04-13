<properties 
    pageTitle="Andmete teisaldamine SQL Server Azure'i virtual arvutis | Azure'i" 
    description="Teisaldage andmed tasapinnalise failidest või asutusesisese SQL Serverist SQL Server Azure'i VM." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Andmete teisaldamine SQL Server Azure'i virtual arvutis

Selles teemas kirjeldatakse võimalusi andmete teisaldamine tasapinnalise failid (CSV- või TSV vormingud) või kohapealne SQL Serverist SQL Server Azure'i virtual arvutis. Järgmised toimingud andmete teisaldamiseks pilveteenuses on andmete meeskonna teadus käigus.

Teema, mis kirjeldab suvandid arvuti õ Azure'i SQL-andmebaasi andmete teisaldamine, leiate [Azure'i masina õppe Azure SQL-andmebaasi andmete teisaldamine](machine-learning-data-science-move-sql-azure.md).

**Menüü** allpool kuidas neelata andmed üheks muude target keskkonnas, kus saate andmeid talletada ja töödelda ajal meeskonnatöö andmete teadus protsess (TDSP) kirjeldavate teemade lingid.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


Järgmises tabelis on andmete teisaldamine SQL Server Azure'i virtual arvutis suvandid.

<b>ALLIKAS</b> |<b>Sihtkoht: SQL Server Azure'i VM</b> |
------------------ |-------------------- |
<b>Lamefailiga</b> |1. <a href="#insert-tables-bcp">käsurea hulgi Kopeeri kasuliku (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">hulgiredigeerimiseks lisa SQL-päringu</a><br> 3. <a href="#sql-builtin-utilities">graafilise sisseehitatud Utiliidid SQL serveris</a>
<b>Asutusesisese SQL serveriga</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy SQL serveri andmebaasi Microsoft Azure VM viisard</a><br> 2. <a href="#export-flat-file">Lamefailiga eksportimine</a><br> 3. <a href="#sql-migration">viisardis SQL-i andmebaasi migreerimine</a> <br> 4. <a href="#sql-backup">andmebaasi tagasi üles ja taastamine</a><br>

Pange tähele, et selle dokumendi eeldab, et SQL-i käskude täidetakse SQL Server Management Studio või Visual Studio andmebaasi Explorer.

> [AZURE.TIP] Teise võimalusena saate [Azure'i andmed Factory](https://azure.microsoft.com/services/data-factory/) loomine ja ajastamine kohaletoimetamisel, mis teisaldab andmed Azure SQL serveri VM. Lisateabe saamiseks lugege teemat [Azure andmete Factory (Kopeeri tegevus) andmete kopeerimine](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Eeltingimused
Selle õpetuse eeldab, et teil on:

* On **Azure tellimust**. Kui olete tellinud, te saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
* **Azure storage konto**. Kasutate Azure storage konto selles õpetuses andmete talletamiseks. Kui teil pole kontot Azure storage, leiate artiklist [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) . Kui olete loonud salvestusruumi konto, peate saada juurdepääsu talletamist kasutatud konto võti. [Vaade, kopeerimine ja uuesti luua salvestusruumi Pääsuklahvide](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)kuvamine
* Ettevalmistatud **SQL Server Azure'i VM**. Juhised leiate teemast [on Azure SQL serveri virtual arvutisse kui serveriks IPython märkmiku täiustatud analüüsi jaoks häälestada](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Installitud ja konfigureeritud **Azure PowerShelli** kohalikult. Juhiste saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Andmete teisaldamine lamefaili allikast SQL serveri on Azure VM

Kui teie andmed on lamefailiga (korraldatud rida/veerg kujul), seda saab teisaldada SQL serveri VM Azure kaudu järgmistest viisidest:

1. [Käsurea hulgi Kopeeri kasuliku (BCP)](#insert-tables-bcp) 
2. [SQL-päringu lisa hulgi](#insert-tables-bulkquery)
3. [Graafilise sisseehitatud Utiliidid SQL serveri (impordi/ekspordi, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Käsurea hulgi koopia kasuliku (BCP)

BCP on installitud versiooniga SQL serveri käsurea kasuliku ja on üks kiireim viise andmete teisaldamiseks. See töötab kõigis kõigi kolme SQL serveri variandid (asutusesisese SQL Server, SQL Azure'i ja SQL Server Azure'i VM). 

> [AZURE.NOTE]**Kui minu andmetel peaks olema jaoks BCP?**  
> Ei ole vaja, võttes faile, mis sisaldavad target SQL Server samas arvutis asuva lähteandmete võimaldab kiirem üleminekuks (võrgu kiiruse vs kohalikule kettale IO kiiruse). Saate teisaldada tasapinnalise faile, mis sisaldavad andmeid masina koht, kus SQL Server on installitud erinevad faili näiteks [AZCopy](../storage/storage-use-azcopy.md)kopeerimise abil, või windows [Azure'i salvestusruumi Exploreri](http://storageexplorer.com/) kopeerige/kleepige Kaugtöölaua protokolli (RDP) kaudu.

1. Veenduge, et target SQL serveri andmebaasi loodud andmebaas ja tabelid. Siin on näide sellest, kuidas selle abil soovitud `Create Database` ja `Create Table` käsud:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Luua vormingus faili, andes käsu käsurea masina järgi skeemi tabeli jaoks kirjeldav kuhu on installitud bcp.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Sisestage andmed andmebaasi käsuga bcp järgmiselt. See peaks töötama käsurea eeldades, et SQL Server on installitud samasse arvutisse:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Lisab BCP optimeerimine** Lugege järgmises artiklis ['Juhised optimeerimine hulgi importimine'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) optimeerida selliste lisab.


### <a name="insert-tables-bulkquery-parallel"></a>Lisab parallelizing kiirem andmete liikumine

Kui teil liiguvad andmed on suur, saate kiirendamiseks täites korraga mitme BCP käsu paralleelselt PowerShelli skripti.

> [AZURE.NOTE]**Big data manustamisest** Suurte ja väga suurte andmekomplektide laadimise andmete kasutamiseks optimeerimise partition loogiline ja füüsiline andmebaasitabelites mitme filegroups ja sektsiooni tabelite abil. Loomine ja andmete partition tabelitele laadimine kohta leiate lisateavet teemast [Paralleelselt laadi SQL Partition tabelist](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Proovi PowerShelli skripti allpool näidata paralleelselt lisab bcp abil:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>SQL-päringu lisa hulgi

Andmete importimine andmebaasi rida/veerg vastavalt failidest ([Ettevalmistamine andmeid hulgi eksportimine või importimine (SQL Server)](https://msdn.microsoft.com/library/ms188609)asuvad toetatud failitüübid) saab [Hulgi lisada SQL-päringu](https://msdn.microsoft.com/library/ms188365) teema. 

Siin on mõned proovi käsud hulgi lisada allpool on:  

1. Andmete analüüsimine ja seadke kohandatud suvandid enne importimise veendumaks, et SQL serveri andmebaasi eeldab, mis tahes teisiti väljadel kuupäevade sama vormingut. Siin on näide sellest, kuidas seadistada kuupäevavormingu kui aasta, kuu ja päeva (kui teie andmed sisaldavad kuupäeva aasta, kuu ja päeva-vormingus).

        SET DATEFORMAT ymd; 
    
2. Andmete importimine hulgi abil importida laused.

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Sisseehitatud Utiliidid SQL serveris

SQL serveri integratsioon (SSIS) abil saate andmed importida lamefailiga Azure SQL serveri VM. SSIS-i on saadaval kahes studio keskkonnas. Lisateavet leiate teemast [Integration Services (SSIS) ja Studio keskkonnas](https://technet.microsoft.com/library/ms140028.aspx):

- SQL Server Data Tools kohta leiate teemast [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Impordi-ja ekspordiviisard kohta täpsema teabe saamiseks vt [SQL serveri impordi- ja ekspordiviisard](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Jooksva SQL Server Azure'i VM asutusesisese SQL serveri andmetega

Saate teha ka järgmist migreerimise strateegiad.

1. [SQL serveri andmebaasi Microsoft Azure VM viisardi juurutamine](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Eksportimine Lamefailiga](#export-flat-file) 
3. [SQL-i andmebaasi migreerimine viisard](#sql-migration)
4. [Andmebaasi tagasi üles ja taastamine](#sql-backup)

Kirjeldame kõik need allpool:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>SQL serveri andmebaasi Microsoft Azure VM viisardi juurutamine

**Deploy SQL serveri andmebaasi Microsoft Azure VM viisard** on lihtne ja Soovitatavad võimalus teisaldada andmeid asutusesisese SQL serveri eksemplar SQL serveri on Azure VM. Üksikasjalikud juhised kui ka muud alternatiivid arutelu, leiate teemast [migreerimine on Azure VM SQL serveri andmebaasi](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Eksportimine Lamefailiga

Erinevad meetodid saab hulgi nagu dokumenteerida teemas [hulgi impordi ja ekspordi, andmete (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) eksportimine andmete asutusesisese SQL serverist. Selle dokumendi hõlmab näiteks hulgi koopia programmi (BCP). Kui andmed on eksporditud lamefailiga, saate selle importida teine SQL serveri abil hulgi impordi. 

1. Andmete eksportimine asutusesisese SQL serveri faili abil bcp kasuliku järgmiselt.

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Luua Azure kasutamise kohta VM SQL Serveri andmebaas ja tabel soovitud `create database` ja `create table` tabeli skeemi eksporditud juhises 1.

3. Tabeli andmed on eksporditud/imporditud skeemi kirjeldamiseks vormingus faili loomine. Vormingus faili andmeid on kirjeldatud [loomine vormingus faili (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Vormindage faili loomisel kui SQL Serveri arvutis töötab BCP 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Vormindage faili loomisel kui töötab BCP eemalt vastu SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Kasutage mõnda kirjeldatud teemas [Faili allika andmete teisaldamine](#filesource_to_sqlonazurevm) tasapinnalise failides andmete teisaldamine SQL serveri.

### <a name="sql-migration"></a>SQL-i andmebaasi migreerimine viisard

[SQL serveri andmebaasi migreerimine viisard](http://sqlazuremw.codeplex.com/) pakub andmete teisaldamiseks ühelt teisele kaks SQL Serveri eksemplari kasutajasõbralikul viisil. See võimaldab kasutajal kaart andmete skeemi andmeallikate ja sihttabelites vahel, valige veerutüübid ja muud erinevaid funktsioone. Seda kasutatakse all hõlmab hulgi Kopeeri (BCP). Kuvatõmmis tervituskuva SQL-i andmebaasi migreerimine viisard on näidatud allpool.  

![SQL serveri migreerimise viisard][2]

### <a name="sql-backup"></a>Andmebaasi tagasi üles ja taastamine

SQL Server toetab. 

1. [Andmebaasi tagasi üles ja funktsioonide taastamine](https://msdn.microsoft.com/library/ms187048.aspx) (nii kohaliku faili või bacpac eksportimine bloobimälu) ja [Andmete taseme rakendused](https://msdn.microsoft.com/library/ee210546.aspx) (abil bacpac). 
2. Võimalus luua otse Azure SQL serveri VMs kopeeritud andmebaasi või olemasoleva SQL Azure'i andmebaasi koopia. Lisateabe saamiseks vt [kasutamine Kopeeri andmebaasi viisard](https://msdn.microsoft.com/library/ms188664.aspx). 

Kuvatõmmis andmebaasi tagasi üles/Taasta SQL Server Management Studio suvandid on näidatud allpool.

![SQL serveri impordi tööriist][1]

## <a name="resources"></a>Ressursid

[SQL Server Azure'i VM andmebaasi migreerimine](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server Azure'i Virtuaalmasinates ülevaade](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
