<properties
    pageTitle="Azure'i SQL-andmebaasi PowerShelliga haldamine | Microsoft Azure'i"
    description="Azure SQL-i andmebaasi haldamine PowerShelli abil."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Azure'i SQL-andmebaasi PowerShelliga haldamine


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShelli](sql-database-manage-powershell.md)

Selles teemas kirjeldatakse teha Azure'i SQL-andmebaasi toiminguid kasutatakse PowerShelli cmdlet-käsud. Täieliku loendi leiate teemast [Azure SQL-i andmebaasi cmdlettide] (https://msdn.microsoft.com/library/mt574084(v=azure.300\).aspx).


## <a name="create-a-resource-group"></a>Ressursi rühma loomine

Ressursi rühma loomine SQL-andmebaasi ja seotud Azure ressursse [New-AzureRmResourceGroup] (https://msdn.microsoft.com/library/azure/mt759837(v=azure.300\).aspx) cmdlet-käsu.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Lisateavet leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).
Proovi skripti, leiate teemast [loomine SQL-andmebaasi PowerShelli skripti](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>SQL-andmebaasi serveri loomine

Luua SQL-andmebaasi server [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) cmdlet-käsu. Asendage *server1* oma serveri nimi. Serverite nimed peavad olema kordumatud kõik Azure'i SQL-andmebaasi serverites. Kui serveri nimi on juba kasutusel, kuvatakse järgmine tõrketeade. See käsk võib kuluda mitu minutit. Ressursirühma peab teie tellimus juba olemas.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Lisateavet leiate teemast [mis on SQL-andmebaasi](sql-database-technical-overview.md). Proovi skripti, leiate teemast [loomine SQL-andmebaasi PowerShelli skripti](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>SQL-andmebaasi serveri tulemüüri reegli loomine

Looge reegel tulemüüri serverit [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) cmdlet-käsu. Käivitage järgmine käsk, asendades kehtivad väärtused algus- ja IP-aadresside oma kliendi jaoks. Ressursirühm ja server peab teie tellimus juba olemas.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Muude Azure'i teenuste juurdepääsu oma serverisse lubamiseks tulemüüri reegli loomine ja määrata nii on `-StartIpAddress` ja `-EndIpAddress` **0.0.0.0**. See reegel teisiti tulemüüri võimaldab kogu Azure liikluse serverit.

Lisateavet leiate teemast [Azure SQL-i andmebaasi tulemüüri](https://msdn.microsoft.com/library/azure/ee621782.aspx). Proovi skripti, leiate teemast [loomine SQL-andmebaasi PowerShelli skripti](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Looge SQL-andmebaasi (tühi)

Andmebaasi loomine koos [New-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) cmdlet-käsu. Ressursirühm ja server peab teie tellimus juba olemas. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Lisateavet leiate teemast [mis on SQL-andmebaasi](sql-database-technical-overview.md). Proovi skripti, leiate teemast [loomine SQL-andmebaasi PowerShelli skripti](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>SQL-andmebaasi jõudluse taseme muutmine

Mastaapimiseks andmebaasi üles või alla [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet-käsu. Ressursirühm, server ja andmebaas peab teie tellimus juba olemas. Määrake soovitud `-RequestedServiceObjectiveName` ühe reasammu lihtsa kiht (nt järgmised koodilõigu). Määrake selleks *S0*, *S1*, *P1*, *P6*jne, nagu eelmises näites tasandi jaoks.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Lisateavet leiate teemast [SQL-andmebaasi suvandid ja jõudluse: mõista, mis on saadaval iga teenuse kiht](sql-database-service-tiers.md). Proovi skripti, leiate teemast [valimi PowerShelli skripti muuta teenuse taseme- ja jõudluse SQL-andmebaasi](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>SQL-andmebaasi kopeerimine sama server

SQL-andmebaasi kopeerimine sama serveri [New-AzureRmSqlDatabaseCopy] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx) cmdlet-käsu. Määrake soovitud `-CopyServerName` ja `-CopyResourceGroupName` sama väärtuste allika andmebaasi serveri ja ressursside rühma nimega.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Lisateavet leiate teemast [kopeerida Azure'i SQL-andmebaasi](sql-database-copy.md). Proovi skripti, leiate teemast [SQL-andmebaasi PowerShelli skripti kopeerida](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>SQL-i andmebaasi kustutamine

Kustutage SQL-andmebaasi [Eemalda-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619368(v=azure.300\).aspx) cmdlet-käsu. Ressursirühm, server ja andmebaas peab teie tellimus juba olemas.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Kustutage SQL-andmebaasi server

Kustutamine serveris [Eemalda-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603488(v=azure.300\).aspx) cmdlet-käsu.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Saate luua ja hallata elastne andmebaasi kaustu PowerShelli abil

Elastne andmebaasi kaustu PowerShelli kaudu loomise kohta leiate üksikasjalikumat teavet leiate teemast [uue andmebaasi elastne rakenduskausta PowerShelliga loomine](sql-database-elastic-pool-create-powershell.md).

Elastne andmebaasi kaustu PowerShelli kaudu haldamise kohta leiate üksikasjalikumat teavet leiate teemast [jälgimine ja haldamine on elastne andmebaasi rakenduskausta PowerShelliga](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Seotud teave

- [SQL Azure'i andmebaasi cmdlettide] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure cmdleti viide] (https://msdn.microsoft.com/library/azure/dn708514(v=azure.300\).aspx)
