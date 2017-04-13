<properties
    pageTitle="Azure'i SQL-andmebaasi loomiseks PowerShelli abil BACPAC faili importimine | Microsoft Azure'i"
    description="Azure'i SQL-andmebaasi loomiseks PowerShelli abil BACPAC faili importimine"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Azure'i SQL-andmebaasi loomiseks PowerShelli abil BACPAC faili importimine

**Ühe andmebaasi**

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-import.md)
- [PowerShelli](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Sellest artiklist leiate Azure'i SQL-andmebaasi importimisega [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili PowerShelliga loomise juhiseid.

Andmebaas on loodud mõne salvestusruumi Azure'i bloobimälu container imporditud BACPAC faili (.bacpac). Kui teil pole Azure Storage BACPAC faili, vt [arhiivi Azure'i SQL-andmebaasiga BACPAC faili PowerShelli abil](sql-database-export-powershell.md). Kui teil on juba BACPAC faili, mis pole Azure Storage, [Kasutage AzCopy lihtsalt üles laadida neid oma Azure Storage konto](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure'i SQL-andmebaasi automaatselt loob ja säilitab iga kasutaja andmebaasi, mille saate taastada varukoopiad. Lisateavet leiate teemast [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md).


SQL-andmebaasi importida, vajate järgmist:

- Azure'i tellimuse. Kui teil on vaja Azure tellimuse lihtsalt **Tasuta prooviversioon** selle lehe ülaosas nuppu ja siis naaske artiklit lõpuleviimiseks.
- Andmebaasi BACPAC faili, mida soovite importida. Funktsiooni BACPAC peab olema [Azure Storage konto](../storage/storage-create-storage-account.md) bloobimälu ümbrises.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Muutujate keskkonna häälestamine

On paar muutujaid, kuhu soovite andmebaasi ja oma konto salvestusruumi teatud väärtused näide väärtuste asendamine.

Serveri nimi peab olema server, mis eelmises juhises valitud tellimuse praegu on olemas. Soovite luua andmebaasi server peaks olema. Andmebaasi importida otse kohapeal on elastne ei toetata. Kuid saate importida esmalt ühe andmebaasi ja teisaldage andmebaas on.

Andmebaasi nimi on nimi, mida soovite uue andmebaasi.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Järgmised muutujad on salvestusruumi konto kaudu, kus asub teie BACPAC. Sirvige [Azure portaali](https://portal.azure.com)konto salvestusruumi saada need väärtused. Esmane kiirklahv võite leida, klõpsates nuppu **Kõik sätted** ja seejärel **klahve** blade salvestusruumi konto kaudu.

Bloobimälu nimi on BACPAC olemasolev fail, mida soovite luua andmebaasi nimi. Tuleb kaasata .bacpac laiendamine.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Töötab [Get-mandaati] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) cmdlet-käsk avab akna küsiks oma kasutajanimi ja parool. Sisestage administraatori kasutajanime ja parooli SQL-andmebaasi server ($ServerName erineb ülalolevast), mitte kasutajanimi ja Azure'i konto parooli.

    $credential = Get-Credential


## <a name="import-the-database"></a>Andmebaasi importimine

See käsk esitab taotluse importimine andmebaasi teenusega. Olenevalt teie andmebaasi mahtu, imporditoimingu võib võtta aega.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Selle toimingu edenemise jälgimine

Pärast opsüsteemi [uus AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), saate kutse olekut käivitades [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL-i andmebaasi PowerShelli impordi skripti


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Järgmised sammud

- Ühenduse ja päringu imporditud SQL-andmebaasi leiate teemast [ühenduse SQL-andmebaasi SQL Server Management Studio ja teha valimi T-SQL-päringu](sql-database-connect-query-ssms.md)
