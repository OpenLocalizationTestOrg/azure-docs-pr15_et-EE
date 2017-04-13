<properties
    pageTitle="Azure'i SQL-andmebaasiga BACPAC faili arhiivida PowerShelli abil"
    description="Azure'i SQL-andmebaasiga BACPAC faili arhiivida PowerShelli abil"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Azure'i SQL-andmebaasiga BACPAC faili arhiivida PowerShelli abil

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-export.md)
- [PowerShelli](sql-database-export-powershell.md)


Selles artiklis antakse juhiseid SQL Azure'i andmebaasi [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili (salvestatud Azure'i bloobimälu) arhiivimiseks PowerShelli kaudu.

Kui teil on vaja luua Arhiiv Azure'i SQL-andmebaasiga, saate eksportida andmebaasi skeemi ja andmete BACPAC faili. BACPAC faili on lihtsalt ZIP faili laiendiga .bacpac. BACPAC faili saab salvestada hiljem Azure'i bloobimälu või kohaliku salvestusruumi kohapealse asukoht. See saab importida tagasi Azure'i SQL-andmebaasi või SQL Serveri install kohapealse.

**Kaalutlused**

- Ülekandega ühtlustamist arhiivi jaoks peate tagama, et ei kirjuta esineb tegevuse käigus ekspordi või ekspordite [ülekandega ühtsete Kopeeri](sql-database-copy.md) Azure SQL-i andmebaasi.
- Azure'i bloobimälu arhiivitakse BACPAC faili maksimummaht on 200 GB. Kasutage suuremat BACPAC faili kohaliku salvestusruumi arhiivimiseks [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) käsuviiba kasuliku. Selle kasuliku teenusega nii Visual Studio ja SQL serveri. Samuti saate [alla laadida](https://msdn.microsoft.com/library/mt204009.aspx) SQL Server Data Tools saada see kasuliku uusim versioon.
- Arhiivimine Azure premium mäluruumi BACPAC faili abil ei toetata.
- Kui eksporditoimingu ületab 20 tundi, võib see tühistada. Ekspordi ajal jõudluse suurendamiseks saate teha järgmist.
 - Oma teenuse taseme ajutine suurendamine
 - Kõik lugemine ja kirjutamine tegevuse käigus ekspordi lõpetada.
 - Kõik suurte tabelite mittenullväärtusi on [rühmitatud indeksi](https://msdn.microsoft.com/library/ms190457.aspx) abil. Rühmitatud registrid, ilma ekspordi võib nurjuda, kui kulub üle 6 / 12 tunni. See on ekspordi teenuse peab lõpule viima tabeli skannimine ja proovige eksportida kogu tabel. Käivitage **DBCC SHOW_STATISTICS** ja veenduge, et *peale RANGE_HI_KEY suurima* ei ole tühi ja selle väärtus on hea jaotuse on hea viis kindlaks teha, kui tabelitel on optimeeritud ekspordi. Lisateavet leiate teemast [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs ei ole mõeldud kasutada varundamise ja taastamise toimingud. Azure'i SQL-andmebaasi loob automaatselt iga kasutaja andmebaasi varundada. Lisateavet leiate teemast [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md).

Selles artiklis tegemiseks vajate järgmist:

- Azure'i tellimuse.
- Azure'i SQL-andmebaasiga.
- [Azure'i Standard salvestusruumi konto](../storage/storage-create-storage-account.md), bloobimälu container on BACPAC talletamiseks kindlad.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Andmebaasi eksportimine

[Uus AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) cmdlet-käsk esitab taotluse ekspordi andmebaasi teenusega. Olenevalt teie andmebaasi mahtu, eksporditoimingu võib võtta aega.

> [AZURE.IMPORTANT] Selleks, et tagada ülekandega ühtsete BACPAC faili, tuleks [luua andmebaasi koopia](sql-database-copy-powershell.md)esimese ja seejärel eksportige andmebaasi koopia.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Eksporditoimingu edenemise jälgimine

Pärast opsüsteemi [uus AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), saate kutse olekut käivitades [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Töötab see kohe pärast taotluse tavaliselt tagastab **olek: InProgress**. Kui kuvatakse teade **olek: õnnestus** ekspordi lõpuleviimist.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Ekspordi SQL-i andmebaasi näide

Järgmises näites on BACPAC ekspordib olemasoleva SQL-andmebaasi ja seejärel näitab, kuidas eksporditoimingu olekut.

Käesolevas näites käivitamiseks on mõned muutujad peate asendama andmebaas ja salvestusruumi konto jaoks kindlate väärtustega. Sirvige [Azure portaali](https://portal.azure.com)konto salvestusruumi lisamiseks salvestusruumikonto nimi, bloobimälu container nimi ja väärtus. Teie salvestusruumi konto tera **kiirklahvide** klõpsates leiate võti.

Asendage järgmine `VARIABLE-VALUES` väärtustega oma teatud Azure ressursse. Andmebaasi nimi on olemasoleva andmebaasiga, mida soovite eksportida.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Järgmised sammud

- Saate teada, kuidas importida PowerShelli abil Azure'i SQL-andmebaasiga, lugege teemat [importimine on BACPAC PowerShelli kaudu](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Lisaressursid

- [Uue AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)