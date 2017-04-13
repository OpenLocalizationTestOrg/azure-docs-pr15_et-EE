<properties
    pageTitle="Ülevaade: Haldusriistad SQL-i andmebaasi | Microsoft Azure'i"
    description="Võrdleb tööriistad ja haldamise Azure'i SQL-andmebaasi suvandid"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Ülevaade: Haldusriistad SQL-i andmebaasi

Selles teemas uurib ja võrreldakse tööriistad ja haldamise Azure SQL-i andmebaasid, et saaksite valida tööriista õige töö, oma äri ja suvandid. Valides õige tööriista sõltub sellest, mitu andmebaasid saate hallata, tööülesande ja toimingu sageduse.

## <a name="azure-portal"></a>Azure'i portaal

[Azure'i portaalis](https://portal.azure.com) on veebipõhine rakendus, kus saate luua, värskendada, ja Kustuta andmebaasid ja loogiline serverite ja andmebaasi tegevuse jälgimine. See tööriist on hea, kui te alles alustate Azure, mõned andmebaaside haldamine või vaja midagi kiiresti.

Portaali kasutamise kohta lisateabe saamiseks vt [SQL-i andmebaaside haldamine abil Azure portaali](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio ja SQL Server Data Tools Visual Studio

SQL Server Management Studio (SSMS) ja SQL serveri andmete tööriistad (SSDT) on klienttööriistade haldamiseks ja arendada oma andmebaasi pilveteenuses teie arvutis töötavad. Kui olete tuttav Visual Studio või muude integreeritud keskkonnas (interaktiivset), [proovige kasutada SSDT Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)rakendus arendaja. Mitme andmebaasi administraatorid tuttavaid SSMS, mida saab kasutada koos SQL Azure'i andmebaasid. [Alla laadida Office'i uusima versiooni SSMS](https://msdn.microsoft.com/library/mt238290) ja Kasuta alati Azure'i SQL-andmebaasi töötamisel uusim versioon. Azure'i SQL-i andmebaasid SSMS haldamise kohta leiate lisateavet teemast [SQL-i andmebaaside haldamine abil SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Alati jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) ja [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) uusima versiooni abil.


## <a name="powershell"></a>PowerShelli

PowerShelli saate hallata andmebaase ja elastne andmebaasi kaustu ja Azure ressursi juurutuste automatiseerimiseks. Microsoft soovitab see tööriist suure hulga andmebaaside haldamine ja juurutamise ja ressursside muudatuste tootmiskeskkonnas automatiseerimine.

Lisateabe saamiseks vt [SQL-i andmebaasi haldamine PowerShelli abil](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Elastne Andmebaasiriistad
Elastne Andmebaasiriistad abil saate näiteks teha järgmisi toiminguid 

* T-SQL-i skripti vastu andmebaase, kasutades on [elastne töö](sql-database-elastic-jobs-overview.md) täitmine
* Mitme rentniku mudeli andmebaaside teisaldamine ühe – rentniku mudeli [tükeldatud ühendamine tööriist](sql-database-elastic-scale-overview-split-and-merge.md)
* Mudeli ühe – rentniku või mitme rentniku mudeli [elastne skaala kliendi teek](sql-database-elastic-database-client-library.md)andmebaaside haldamine.
 

## <a name="additional-resources"></a>Lisaressursid

- [Azure'i ressursihaldur](https://azure.microsoft.com/features/resource-manager/)
- [Azure'i automatiseerimine](https://azure.microsoft.com/documentation/services/automation/)
- [Azure'i ajasti](https://azure.microsoft.com/documentation/services/scheduler/)