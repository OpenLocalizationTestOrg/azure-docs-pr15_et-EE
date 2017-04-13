<properties
    pageTitle="Azure'i SQL-andmebaasi server taseme tulemüüri reeglid konfigureerimine PowerShelli abil | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida tulemüüri SQL Azure'i andmebaasid IP-aadressid."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Azure'i SQL-andmebaasi server taseme tulemüüri reeglid konfigureerimine PowerShelli abil


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-firewall-configure.md)
- [Azure'i portaal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShelli](sql-database-configure-firewall-settings-powershell.md)
- [REST API-GA](sql-database-configure-firewall-settings-rest.md)


Azure'i SQL-andmebaasi kasutab tulemüüri reeglid lubama ühendusi serverid ja andmebaasid. Saate määratleda valikuliselt andmebaasi juurdepääsu lubamine oma SQL-andmebaasi server server ja andmebaas-taseme tulemüüri sätted põhi andmebaasi või kasutaja andmebaasi.

> [AZURE.IMPORTANT] Rakenduste Azure oma andmebaasiserveriga ühenduse loomiseks peab olema lubatud Azure ühendused. Tulemüüri reeglid ja Azure võimaldavad ühenduste kohta leiate lisateavet teemast [Azure SQL-i andmebaasi tulemüüri](sql-database-firewall-configure.md). Kui teete ühendused Azure pilveteenuse Rühmaboks sisse, tuleb avada mõne pordid. Lisateabe saamiseks vt selle "SQL-i andmebaasi V12: väljaspool vs sees" [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md)osas.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Serveri tulemüüri reeglite loomine

Serveritaseme tulemüüri reeglite loomist, värskendada ja Azure PowerShelli abil kustutada.

Uue serveritaseme tulemüüri reegli loomiseks käivitada [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) cmdlet-käsu. Järgmises näites võimaldab serveris Contoso on vahemik, IP-aadressid.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Olemasoleva serveritaseme tulemüüri reegli muutmiseks käivitada [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) cmdlet-käsu. Järgmises näites muutub lubatud IP-aadresside reegli ContosoFirewallRule nimega vahemik.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Olemasoleva serveritaseme tulemüüri reegli kustutamiseks käivitada [Eemalda-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) cmdlet-käsu. Järgmises näites kustutab reeglit nimega ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Tulemüüri reeglite haldamiseks PowerShelli abil

Tulemüüri reeglite haldamiseks saate kasutada ka PowerShelli. Lisateavet leiate järgmistest teemadest:

* [Uue AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Eemalda AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Järgmised sammud

Kasutamise kohta teavet teemast Transact-SQL server ja andmebaas-taseme tulemüüri reeglid luua [konfigureerimine Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reeglid T-SQL-i abil](sql-database-configure-firewall-settings-tsql.md).

Serveritaseme tulemüüri reeglid teiste meetodite abil leiate teavet selle kohta, kuidas luua.

- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglite abil REST API konfigureerimine](sql-database-configure-firewall-settings-rest.md)

Andmebaasi loomiseks, juhendi leiate [Azure'i portaalis minutites SQL-andmebaasi loomine](sql-database-get-started.md).
Azure SQL-i andmebaasiga ühenduse kaudu avatud lähte- või muude tootjate rakenduste korral lugege teemat [Kliendi kiire alustada koodinäiteid SQL-andmebaasiga](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Mõistmaks, kuidas liikuda andmebaasidele, lugege teemat [Accessi ja logige sisse andmebaasi turvalisuse haldamine](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Lisaressursid

- [Andmebaasi turvamine](sql-database-security.md)
- [Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
