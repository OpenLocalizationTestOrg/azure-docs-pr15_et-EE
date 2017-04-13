<properties
    pageTitle="SQL-andmebaasi server taseme tulemüüri reegli konfigureerimine | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida tulemüüri juurdepääsu Azure SQL serveri IP-aadressid."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Azure'i SQL-andmebaasi server taseme tulemüüri reegli Azure'i portaalis konfigureerimine


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-firewall-configure.md)
- [Azure'i portaal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShelli](sql-database-configure-firewall-settings-powershell.md)
- [REST API-GA](sql-database-configure-firewall-settings-rest.md)

Azure SQL server kasutab tulemüüri reeglid lubama ühendusi serverid ja andmebaasid. Saate määratleda server ja andmebaas-taseme tulemüüri sätted juhteksemplari või kasutaja andmebaasi Azure SQL-i serveri loogiline serveri valikuliselt Luba juurdepääs andmebaasile. Selles teemas käsitletakse serveritaseme tulemüüri reeglid.

> [AZURE.IMPORTANT] Rakenduste Azure oma Azure SQL serveriga ühenduse loomiseks peab olema lubatud Azure ühendused. Mõistmaks, kuidas tulemüüri reeglite töö, lugege teemat [konfigureerimine on Azure SQL serveri tulemüüri \- ülevaade](sql-database-firewall-configure.md). Kui teete ühendused Azure pilveteenuse Rühmaboks sisse, tuleb avada mõne pordid. Lisateabe saamiseks vt selle **SQL-andmebaasi V12: väljaspool vs sees** [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md) osas

**Soovitus:** Kasutage serveritaseme tulemüüri reeglid administraatorid ja kui teil on palju andmebaasidele, mis on sama juurdepääs nõuded ja te ei soovi iga andmebaasi konfigureerimine eraldi aega. Microsoft soovitab võimalusel turvalisuse täiustamiseks ja teha andmebaasi kantavate andmebaasi taseme tulemüüri reeglite abil.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Olemasoleva serveritaseme tulemüüri reeglid Azure portaali kaudu hallata

Korrake samme serveritaseme tulemüüri reeglite haldamiseks.

- Praeguse arvuti lisamiseks klõpsake nuppu Lisa kliendi IP.
- Täiendavad IP-aadresside lisamiseks tippige reegli nimi, käivitage IP-aadress ja lõpuks IP-aadress.
- Olemasoleva reegli muutmiseks klõpsake mõnda reeglit väljad ja muuta.
- Olemasoleva reegli kustutamiseks viige kursor reegli kuni X kuvatakse rea lõpus. Klõpsake nuppu X, et reeglit eemaldada.

Klõpsake nuppu **Salvesta** muudatused salvestada.

## <a name="next-steps"></a>Järgmised sammud

Vt artikli kohta, kuidas luua server ja andmebaas-taseme tulemüüri reeglid Transact-SQL-i abil, kuidas [konfigureerida Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reeglid T-SQL-i abil](sql-database-configure-firewall-settings-tsql.md). 

Kuidas artikleid serveritaseme tulemüüri reegli loomise kohta teiste meetodite abil vaadake jaoks: 

- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid PowerShelli kaudu konfigureerimine](sql-database-configure-firewall-settings-powershell.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglite abil REST API konfigureerimine](sql-database-configure-firewall-settings-rest.md)

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

 
