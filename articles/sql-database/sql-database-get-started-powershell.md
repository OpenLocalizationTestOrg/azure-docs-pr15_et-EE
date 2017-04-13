<properties
    pageTitle="Uue SQL-andmebaasi seadistamine PowerShelli abil | Microsoft Azure'i"
    description="Siit saate teada, nüüd PowerShelliga SQL-andmebaasi loomiseks. Levinud andmebaasi häälestustoimingute saab hallata PowerShelli cmdlet-käsud."
    keywords="Looge uus sql-andmebaasi, andmebaasi seadistamine"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>SQL-i andmebaasi loomine ja teha levinud andmebaasi häälestustoimingute PowerShelli cmdlet-käsud


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-get-started.md)
- [PowerShelli](sql-database-get-started-powershell.md)
- [C#:](sql-database-get-started-csharp.md)



Siit saate teada, kuidas luua PowerShelli cmdlet-käskude abil SQL-andmebaasi. (Vt [Loo uus elastne andmebaasi rakenduskausta PowerShelliga](sql-database-elastic-pool-create-powershell.md)elastne andmebaaside loomise.)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Andmebaasi seadistamine: ressursirühm, server ja tulemüüri reegli loomine

Kui teil on juurdepääs cmdlettide vastuolus teie valitud Azure'i tellimus, on järgmiseks loomine ressursirühm, mis sisaldab server, kui andmebaas on loodud. Järgmise käsu kasutada mis tahes kehtiv asukoht valige saab redigeerida. Käivitage **(Get-AzureRmLocation | WHERE-objekti {$_. Pakkujad - eq "Microsoft.Sql"}). Asukoht** saada kehtiv asukohtade loendit.

Ressursi rühma loomiseks järgmine käsk:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Serveri loomine

SQL-i andmebaasid on loodud Azure'i SQL-andmebaasi serverid sees. Käivitage [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) luua server. Oma serveri nimi peab olema kordumatu kõik Azure'i SQL-andmebaasi serveriga. Kui serveri nimi on juba kasutusel, kuvatakse järgmine tõrketeade. Ka märkimist on, et see käsk võib kuluda mitu minutit. Saate kasutada mis tahes kehtiv asukoht valite käsu redigeerida, kuid peaksite kasutama samasse asukohta, mida kasutasite eelmises etapis loodud ressursirühma.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Kui käivitate selle käsu, küsitakse teie kasutajanime ja parooli. Ärge sisestage mandaat Azure. Sisestage kasutajanime ja parooli loomine serveri administraatorina. Skripti allosas selles artiklis näitab, kuidas määrata serveri kood.

Serveri üksikasjad kuvatakse pärast seda, kui server on loodud.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Serveri tulemüüri reegli serveri juurdepääsu konfigureerimine

Server, peate tulemüüri reegli loomiseks. Käivitage [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) käsk, asendades arvuti sobivad väärtused algus ja lõpp IP-aadressid.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Tulemüüri reegli üksikasjad kuvatakse pärast reegli loomist edukalt.

Luba muude Azure'i teenuste serverit, tulemüüri reegli lisamine ja seadke soovitud StartIpAddress ja EndIpAddress 0.0.0.0. See reegel võimaldab Azure liiklus mis tahes Azure'i tellimus serverit.

Lisateavet leiate teemast [Azure SQL-i andmebaasi tulemüüri](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>SQL-i andmebaasi loomine

Nüüd on teil ressursirühma, server ja tulemüüri reegli konfigureeritud nii, et pääseksite server.

Järgmine [uus AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) käsk loob (blank) SQL-andmebaasi Standard teenuse kiht, kus S1 jõudluse:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Andmebaasi üksikasjad kuvatakse pärast seda, kui andmebaas on loodud.

## <a name="create-a-sql-database-powershell-script"></a>SQL-i andmebaasi PowerShelli skripti loomine

Järgmist PowerShelli skripti loob SQL-andmebaasi ja oma sõltuvad ressursse. Asenda kõik `{variables}` väärtustega, mis on seotud oma tellimust ja ressursid (kui seate oma väärtuste eemaldamine **{}** ).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Järgmised sammud
Pärast SQL-i andmebaasi loomine ja teha lihtsa andmebaasi häälestustoimingute, olete valmis järgmist:

- [SQL-andmebaasi PowerShelliga haldamine](sql-database-manage-powershell.md)
- [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu tegemine](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Lisaressursid

- [SQL Azure'i andmebaasi cmdlettide] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure'i SQL-andmebaas](https://azure.microsoft.com/documentation/services/sql-database/)
