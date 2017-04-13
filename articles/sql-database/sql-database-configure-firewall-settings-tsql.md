<properties
    pageTitle="Abil T-SQL Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reegleid | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida tulemüüri SQL Azure'i andmebaasid IP-aadressid."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reeglite abil T-SQL-i konfigureerimine


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-firewall-configure.md)
- [Azure'i portaal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShelli](sql-database-configure-firewall-settings-powershell.md)
- [REST API-GA](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure'i SQL-andmebaasi kasutab tulemüüri reeglid lubama ühendusi serverid ja andmebaasid. Saate määratleda juhteksemplari või kasutaja andmebaasi server ja andmebaas-taseme tulemüüri sätted oma Azure'i SQL-andmebaasi serveri valikuliselt andmebaasi juurdepääsu lubamine.

> [AZURE.IMPORTANT] Rakenduste Azure oma andmebaasiserveriga ühenduse loomiseks peab olema lubatud Azure ühendused. Tulemüüri reeglid ja Azure võimaldavad ühenduste kohta leiate lisateavet teemast [Azure SQL-i andmebaasi tulemüüri](sql-database-firewall-configure.md). Kui teete ühendused Azure pilveteenuse Rühmaboks sisse, tuleb avada mõne pordid. Lisateabe saamiseks vt selle **SQL-andmebaasi V12: väljaspool vs sees** [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md) osas


## <a name="server-level-firewall-rules"></a>Serveritaseme tulemüüri reeglid

Serveritaseme põhisumma login või Azure Active Directory administraator saab serveritaseme tulemüüri reegli loomine Transact-SQL-i abil.

1. Käivitage päring aknasse ja virtuaalse juhtslaidi andmebaasiga on ühenduse SQL Server Management Studio abil.
2. Serveritaseme tulemüüri reeglid saate valitud, loodud, värskendatud või päringuakna jooksul kustutatud.
3. Luua või värskendada serveritaseme tulemüüri reeglid, käivitada soovitud `sp_set_firewall_rule` salvestatud protseduur. Järgmises näites võimaldab serveris Contoso on vahemik, IP-aadressid.<br/>Alustuseks näha, millised on juba olemas.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Järgmiseks tulemüüri reegli lisamine.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Serveritaseme tulemüüri reegli kustutamiseks käivitada sp_delete_firewall_rule salvestatud protseduur. Järgmises näites kustutab reeglit nimega ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Nende salvestatud toimingute kohta leiate lisateavet teemast [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) ja [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Andmebaasi taseme tulemüüri reeglid

Ainult andmebaasi õigustega kasutaja on **JUHTELEMENTI** andmebaasi (nt andmebaasi omanik) saate luua andmebaasi taseme tulemüüri reegel.

1. Pärast loomist serveritaseme tulemüüri IP-aadress, käivitage päringuakna klassikaline portaali kaudu või SQL Server Management Studio.
2. Ühenduse andmebaasi, mille soovite andmebaasi taseme tulemüüri reegli loomiseks.

    Saate luua uue või olemasoleva andmebaasi taseme tulemüüri reegli värskendada, käivitada soovitud `sp_set_database_firewall_rule` salvestatud protseduur. Järgmises näites luuakse uus tulemüüri reegli nimega ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Olemasoleva andmebaasi taseme tulemüüri reegli kustutamiseks käivitada soovitud `sp_delete_database_firewall_rule` salvestatud protseduur. Järgmises näites kustutab reeglit nimega ContosoFirewallRule.
`
   EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule "

Nende salvestatud toimingute kohta leiate lisateavet teemast [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) ja [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Järgmised sammud

Kuidas artikleid serveritaseme tulemüüri reegli loomise kohta teiste meetodite abil vaadake jaoks: 

- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid PowerShelli kaudu konfigureerimine](sql-database-configure-firewall-settings-powershell.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglite abil REST API konfigureerimine](sql-database-configure-firewall-settings-rest.md)

Andmebaasi loomiseks, juhendi leiate [Azure'i portaalis minutites SQL-andmebaasi loomine](sql-database-get-started.md).
Azure'i SQL-andmebaasiga ühenduse loomiseks kaudu avatud lähte- või muude tootjate rakenduste korral lugege teemat [Kliendi kiire alustada koodinäiteid SQL-andmebaasiga](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Mõistmaks, kuidas liikuda andmebaasidele, lugege teemat [Accessi ja logige sisse andmebaasi turvalisuse haldamine](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Lisaressursid

- [Andmebaasi turvamine](sql-database-security.md)
- [Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589)
