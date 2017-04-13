<properties
    pageTitle="Azure'i SQL-andmebaasi server taseme tulemüüri reeglid REST API abil | Microsoft Azure'i"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Azure'i SQL-andmebaasi server taseme tulemüüri reeglite abil REST API konfigureerimine


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-firewall-configure.md)
- [Azure'i portaal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShelli](sql-database-configure-firewall-settings-powershell.md)
- [REST API-GA](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure'i SQL-andmebaasi kasutab tulemüüri reeglid lubama ühendusi serverid ja andmebaasid. Saate määratleda juhteksemplari või kasutaja andmebaasi server ja andmebaas-taseme tulemüüri sätted oma Azure'i SQL-andmebaasi serveri valikuliselt andmebaasi juurdepääsu lubamine.

> [AZURE.IMPORTANT] Rakenduste Azure oma andmebaasiserveriga ühenduse loomiseks peab olema lubatud Azure ühendused. Tulemüüri reeglid ja Azure võimaldavad ühenduste kohta leiate lisateavet teemast [Azure SQL-i andmebaasi tulemüüri](sql-database-firewall-configure.md). Kui teete ühendused Azure pilveteenuse Rühmaboks sisse, tuleb avada mõne pordid. Lisateabe saamiseks vt selle **SQL-andmebaasi V12: väljaspool vs sees** [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md) osas


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Läbi REST API serveritaseme tulemüüri reeglite haldamine
1. REST API läbi tulemüüri reeglite haldamiseks peavad olema kinnitatud. Lisateavet leiate teemast [arendaja juhend autoriseerimine Azure'i ressursihaldur API-ga](../resource-manager-api-authentication.md).
2. Serveritaseme reeglite loomist, värskendatud või kustutatud REST API abil

    Luua või värskendada serveritaseme tulemüüri reegel, käivitada panna meetodit kasutades järgmist:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Koosolekukutse kehasse.

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Olemasoleva serveritaseme tulemüüri reegli eemaldamiseks käivitada Kustuta meetodit kasutades järgmist:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Kasutades REST API tulemüüri reeglite haldamine

* [Loomine ja värskendamine tulemüüri reegel](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Tulemüüri reegli kustutamine](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Tulemüüri reegel hankimine](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Loetle kõik tulemüüri reeglid](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Järgmised sammud

Vt artikli kohta, kuidas luua server ja andmebaas-taseme tulemüüri reeglid Transact-SQL-i abil, kuidas [konfigureerida Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reeglid T-SQL-i abil](sql-database-configure-firewall-settings-tsql.md). 

Kuidas artikleid serveritaseme tulemüüri reegli loomise kohta teiste meetodite abil vaadake jaoks: 

- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid PowerShelli kaudu konfigureerimine](sql-database-configure-firewall-settings-powershell.md)

Andmebaasi loomiseks, juhendi leiate [Azure'i portaalis minutites SQL-andmebaasi loomine](sql-database-get-started.md).
Azure'i SQL-andmebaasiga ühenduse loomiseks kaudu avatud lähte- või muude tootjate rakenduste korral lugege teemat [Kliendi kiire alustada koodinäiteid SQL-andmebaasiga](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Mõistmaks, kuidas liikuda andmebaasidele, lugege teemat [Accessi ja logige sisse andmebaasi turvalisuse haldamine](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Lisaressursid

- [Andmebaasi turvamine](sql-database-security.md)
- [Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 
