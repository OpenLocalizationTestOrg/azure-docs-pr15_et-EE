<properties
   pageTitle="SQL Azure'i andmebaasi õpetused uurimine | Microsoft Azure'i"
   description="Siit saate teada, SQL-andmebaasi funktsioonide ja võimaluste kohta"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>SQL Azure'i andmebaasi õpetused uurimine

Saamiseks allpool olevaid linke teid ülevaade iga loetletud funktsiooni ala ja lihtsa juhise järgi start õpetuse iga ala. Lahendus ulatusega kiirülevaate algust, mis illustreerivad täielik lahendus põhineb real stsenaariumid, SQL-andmebaasi kasutamine teemast [Azure SQL-i andmebaasi lahenduse kiirülevaate algab](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio abil

Järgmised õpetused, saate teada, SQL Server Management Studio abil saate hallata ja päringu Azure'i SQL-andmebaasi kohta.


> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Azure'i SQL-andmebaasi serveritaseme põhisumma login abil ühendamine](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| Selles õppetükis saate teada, kuidas abil serveritaseme põhisumma login Azure SQL-i andmebaasiga ühenduse loomiseks.|
| [Kasutaja Azure SQL-andmebaasiga ühenduse loomine](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | Selles õppetükis saate teada, kuidas andmebaasi taseme kasutaja kontot Azure SQL-andmebaasiga ühenduse loomiseks.|
||||

## <a name="elastic-pools"></a>Elastne kaustu

Järgmised õpetused tutvustame teile [elastne kaustu](sql-database-elastic-pool.md) abil jõudluse eesmärki suuresti üksteisest erinevad ja ettearvamatuid mustreid mitme andmebaaside haldamine.

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Kohapeal on elastne loomine](sql-database-elastic-pool-create-portal.md) | Selles õppetükis saate teada, kuidas luua scalable kogumi Azure SQL-i andmebaasid. |
| [Kuvari elastne andmebaasist loobumine](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | Selles õppetükis saate teada, kuidas jälgida üksikute elastne andmebaasiga võimalikke probleeme. |
| [Teatise rakenduskausta ressursi lisamine](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | Selles õppetükis saate teada, kuidas lisada ressursse, mis saadavad meili inimesed või URL-i lõpp-punktid teatiste stringide kui ressursi tabab kasutamine lävi, mille seadistasite reeglid. |
| [Andmebaasi viimine kohapeal on elastne](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | Selles õppetükis saate teada, kuidas liikuda andmebaasi kohapeal on elastne. |
| [Andmebaasi kolima kohapeal on elastne](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | Selles õppetükis saate teada, kuidas andmebaasi kolima kohapeal on elastne. |
| [Jõudluse, on sätete muutmine](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | Selles õppetükis saate teada, kuidas kohandada on jõudlus ja salvestusruumi limiidid. |
||||

## <a name="elastic-database-jobs"></a>Elastne andmebaasi tööde haldamine

Järgmised õpetused tutvustame teile kasutades [elastne andmebaasi tööd](sql-database-elastic-jobs-overview.md).

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Elastne Andmebaasiriistad kasutamise alustamine](sql-database-elastic-scale-get-started.md) | Selles õppetükis saate teada, kuidas lihtsa sharded rakendust elastne Andmebaasiriistad võimaluste kasutamiseks. |
| [Azure'i SQL-andmebaasi elastne töö alustamine](sql-database-elastic-jobs-getting-started.md)  | Selles õppetükis saate teada, kuidas soovite loomine ja haldamine töö mis rühma seotud andmebaaside haldamine.  | 
||||

## <a name="elastic-queries"></a>Elastne päringud

Järgmised õpetused tutvustame teile töötab [elastne päringud](sql-database-elastic-query-overview.md). 

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Päringute üle horisontaalselt sektsioonitud (sharded) andmebaas)](sql-database-elastic-query-getting-started.md) | Selles õppetükis saate teada, kuidas luua aruandeid kõik andmebaasid horisontaalselt sektsioonitud (sharded) andmebaasi [elastne päringu](sql-database-elastic-query-overview.md) abil |
| [Päringute üle vertikaalselt sektsioonitud andmebaasi)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | Selles õppetükis saate teada, kuidas luua aruandeid kõik andmebaasid on vertikaalselt andmebaasi [elastne päringu](sql-database-elastic-query-overview.md) abil |
| [Skaala-out olemasoleva andmebaasi migreerimine](sql-database-elastic-convert-to-use-elastic-tools.md)| Selles õppetükis saate teada, horisontaalselt skaala (Kildu) SQL Azure'i andmebaasi. |
||||

## <a name="performance-optimization"></a>Jõudluse optimeerimine

Järgmised õpetused tutvustame teile optimeerida [jõudlust ühe andmebaasid](sql-database-performance-guidance.md). Mitme andmebaasi optimeerides, leiate teemast [elastne kaustu](#elastic-pools).

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Andmebaasi teenuse taseme- ja jõudluse taseme muutmine](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | Selles õppetükis saate teada, kuidas skaalal või mastaapimiseks Azure'i SQL-andmebaasiga, kasutades teenuse astme jõudlust. |
| [SQL-i andmebaasi Advisor päringu jõudluse tutvustus](sql-database-performance.md#performance-overview) | Selles õppetükis saate teada, kuidas avada ja kasutada SQL-i andmebaasi Advisor päringu jõudluse ülevaate.|
| [SQL-i andmebaasi Advisor jõudluse soovitused](sql-database-advisor.md#viewing-recommendations) | Selles õppetükis saate teada, kuidas vaadata ja rakendada SQL-i andmebaasi Advisor jõudluse soovitusi. |
| [Vaadake üle ülemise CPU tarbimine päringud](sql-database-query-performance.md#review-top-cpu-consuming-queries)| Selles õppetükis saate teada, kuidas kasutada SQL-i andmebaasi Advisor päringu jõudluse ülevaate ülemise CPU tarbimine päringute üle vaadata.|
| [Üksikute päringu üksikasjade kuvamine](sql-database-query-performance.md#viewing-individual-query-details)| Selles õppetükis saate teada, kuidas kasutada SQL-i andmebaasi Advisor päringu jõudluse ülevaate üksikute päringu jõudluse üksikasjade kuvamine.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL-i andmebaasi migreerimine ja arhiivimine 

Järgmised õpetused tutvustame teile [migreerimine olemasoleva SQL serveri andmebaasi Azure SQL-andmebaasiga](sql-database-cloud-migrate.md).

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [SQL Server Data Tools for Visual Studio abil ühilduvusprobleemide tuvastamiseks](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | Selles õppetükis saate teada, kuidas kasutada SQL Server Data Tools for Visual Studio Azure'i SQL-andmebaasi ühilduvuse määratlemiseks. |
| [Probleemide ühilduvusega seotud probleemid SQL Server Data Tools for Visual Studio abil](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | Selles õppetükis saate teada, kuidas kasutada SQL Server Data Tools for Visual Studio Azure'i SQL-andmebaasi ühilduvusprobleemi lahendamiseks. |
| [SQL-andmebaasi ühilduvuse abil SqlPackage.exe määratlemine](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | Selles õppetükis saate teada, kuidas kasutada SQLPackage.exe käsurea kasuliku Azure'i SQL-andmebaasi ühilduvuse määratlemiseks.|
| [SQL-andmebaasi ühilduvuse abil SSMS määratlemine](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |Selles õppetükis saate teada, kuidas kasutada SQL Server Management Studio Azure'i SQL-andmebaasi ühilduvuse määratlemiseks.|
| [SQL serveri andmebaasi migreerimine SQL-andmebaasiga, kasutades Microsoft Azure'i andmebaasi viisard andmebaasi juurutamine](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | Selles õppetükis saate teada, kuidas ühilduvad SQL serveri andmebaasi migreerimine Azure SQL-i andmebaasi SQL Server Management Studio andmebaasi Microsoft Azure'i andmebaasi viisardi abil.
| [SQL serveri andmebaasi eksportida BACPAC faili SSMS abil](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Selles õppetükis saate teada, kuidas ühilduvad SQL serveri andmebaasi eksportida BACPAC faili eksportida andmed taseme rakenduse viisardi abil SQL Server Management Studio.|
| [SQL serveri andmebaasi eksportida BACPAC faili SqlPackage abil](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Selles õppetükis saate teada, kuidas ühilduvad SQL serveri andmebaasi eksportida BACPAC faili SQLPackage.exe kasuliku käsurea kaudu.|
| [Azure'i SQL-andmebaasi SSMS abil BACPAC faili importimine](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Selles õppetükis saate teada, kuidas Azure'i SQL-andmebaasi BACPAC faili eksportida andmed taseme rakenduse viisardi abil SQL Server Management Studio andmebaasi importida. |
| [Azure'i SQL-andmebaasi kasutamise SqlPackage BACPAC faili importimine](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | Selles õppetükis saate teada, kuidas andmebaasi Azure SQL-andmebaasi kasutades SQLPackage käsurea kasuliku BACPAC faili importida. |
| [Azure'i SQL-andmebaasi Azure'i portaalis BACPAC faili importimine](sql-database-import.md) | Selles õppetükis saate teada, kuidas andmebaasi importida Azure SQL-andmebaasi BACPAC faili, mis on talletatud mõne Azure'i bloobimälu Azure'i portaalis.|
| [Azure'i SQL-andmebaasi PowerShelli kaudu BACPAC faili importimine](sql-database-import-powershell.md) | Selles õppetükis saate teada, kuidas andmebaasi importida Azure SQL-andmebaasi BACPAC faili PowerShelli kaudu.|
| [Azure'i portaalis Azure SQL-andmebaasi arhiivimine](sql-database-export.md#export-your-database) | Selles õppetükis saate teada, kuidas Azure'i SQL-andmebaasiga Azure'i portaalis BACPAC faili arhiivida. |
| [PowerShelli kasutamine Azure SQL-andmebaasi arhiivimine](sql-database-export-powershell.md) | Selles õppetükis saate teada, kuidas arhiivida Azure'i SQL-andmebaasiga BACPAC faili PowerShelli kaudu. |
| [Kopeerige Azure'i portaalis Azure SQL-i andmebaas](sql-database-copy.md#copy-your-sql-database) | Selles õppetükis saate teada, kuidas kopeerida Azure'i portaalis Azure SQL-andmebaasi. |
| [Kopeerige PowerShelli kaudu Azure SQL-i andmebaas](sql-database-copy-powershell#copy-your-sql-database) | Selles õppetükis saate teada, kuidas kopeerida Azure SQL-andmebaasi PowerShelli kaudu. |
| [Kopeerige Transact-SQL-i abil SQL Azure'i andmebaas](sql-database-copy-transact-sql.md#copy-your-sql-database) | Selles õppetükis saate teada, kuidas kopeerida abil Transact-SQL Azure'i SQL-andmebaasi. |
||||

##<a name="develop"></a>Töötada

Järgmised õpetused tutvustame teile [SQL-i andmebaasi arendamine](sql-database-develop-overview.md) ja [ühenduvuse teekide](sql-database-libraries.md)abil.

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Ühenduse loomine SQL-andmebaasiga, kasutades .NET (C#)](sql-database-develop-dotnet-simple.md) | Selles õppetükis saate teada, kuidas abil C# Azure SQL-andmebaasiga ühenduse loomiseks. |
| [Java abil SQL-andmebaasiga ühenduse loomine](sql-database-develop-java-simple.md) | Selles õppetükis saate teada, kuidas abil Java Azure SQL-andmebaasiga ühenduse loomiseks. |
| [Ühenduse loomine SQL-andmebaasi Node.js abil](sql-database-develop-nodejs-simple.md) | Selles õppetükis saate teada, kuidas abil Node.js Azure SQL-andmebaasiga ühenduse loomiseks. |
| [Ühenduse loomine SQL-andmebaasi PHP abil](sql-database-develop-php-simple.md) | Selles õppetükis saate teada, kuidas kasutades PHP Azure SQL-andmebaasiga ühenduse loomiseks. |
| [Ühenduse loomine SQL-andmebaasi Python abil](sql-database-develop-python-simple.md) | Selles õppetükis saate teada, kuidas kasutades Python Azure SQL-andmebaasiga ühenduse loomiseks. |
| [Ühenduse loomine SQL-andmebaasi Ruby abil](sql-database-develop-ruby-simple.md) | Selles õppetükis saate teada, kuidas abil Ruby Azure SQL-andmebaasiga ühenduse loomiseks. |
||||
 
## <a name="database-access"></a>Accessi andmebaasi

Järgmised õpetused, saate teada, [sisselogimise ja kasutajate loomise ja haldamise](sql-database-manage-logins.md)kohta.

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Azure'i portaalis Azure'i SQL-andmebaasi server taseme tulemüüri reegli loomine](sql-database-configure-firewall-settings.md)  | Selles õppetükis saate teada, kuidas konfigureerida SQL-andmebaasi server taseme tulemüüri Azure'i portaalis.  |
| [Kasutades Transact-SQL-i andmebaasi taseme tulemüüri reegli loomine](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | Selles õppetükis saate teada, kuidas abil Transact-SQL-i andmebaasi taseme tulemüüri reegli loomiseks.|
| [Serveritaseme tulemüüri reeglite abil Transact-SQL-i haldamine](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | Selles õppetükis saate teada, kuidas hallata Azure'i SQL-andmebaasi server taseme tulemüüri Transact-SQL-i abil.|
| [PowerShelli kasutamine serveritaseme tulemüüri reeglite haldamine](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | Selles õppetükis saate teada, kuidas hallata Azure'i SQL-andmebaasi server taseme tulemüüri PowerShelli kaudu.|
| [Kasutades REST API serveritaseme tulemüüri reeglite haldamine](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | Selles õppetükis saate teada, kuidas hallata Azure'i SQL-andmebaasi server taseme tulemüüri lähtestamine API abil.|
| [Azure'i SQL-andmebaasi serveritaseme põhisumma login abil ühendamine](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| Selles õppetükis saate teada, kuidas abil serveritaseme põhisumma login Azure SQL-i andmebaasiga ühenduse loomiseks.|
| [Andmebaasi sisselogimise juurdepääsu andmine] (sql-database-manage-logins.md#granting-database-access-to-a-login() | Selles õppetükis saate teada, kuidas andmebaasi juurdepääsu serveritaseme sisselogimist.|
| [Saate luua uue andmebaasi kasutaja SSMS abil](sql-database-get-started-security.md#create-new-database-user-using-ssms) | Selles õppetükis saate teada, kuidas luua uus andmebaas kasutaja olemasoleva andmebaasi SSMS abil.|
| [Uue andmebaasi db_owner kasutusõiguste andmine](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | Selles õppetükis saate teada, kuidas olemasoleva andmebaasiga db_owner kasutusõiguste andmine.|
| [Kasutaja Azure SQL-andmebaasiga ühenduse loomine](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | Selles õppetükis saate teada, kuidas andmebaasi taseme kasutaja kontot Azure SQL-andmebaasiga ühenduse loomiseks.|
||||


## <a name="data-security"></a>Andmekaitse

Järgmised õpetused, saate teada, [Azure'i SQL-andmebaasi andmete turvamise](sql-database-security.md)kohta. 

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Azure'i portaalis andmebaasi ohtude tuvastamise lubamine](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | Selles õppetükis saate teada, kuidas häälestada ohtude tuvastamise andmebaasi Azure'i portaalis.|
| [Turvaline tundlikku teavet žestid alati krüptitud](sql-database-always-encrypted-azure-key-vault.md) | Selles õpetuses on alati krüptitud viisardi abil kaitsta tundlikku andmeid Azure SQL-andmebaasis.|
| [Turvaline senstive andmete läbipaistvaid andmete krüptimine](https://msdn.microsoft.com/library/dn948096.aspx)| Selles õppetükis saate teada, kuidas läbipaistvaid andmete krüptimise senstive andmete turvalisuse tagamiseks.|
| [Andmeveerg krüptimine](https://msdn.microsoft.com/library/ms179331.aspx)| Selles õppetükis saate teada, kuidas krüptida Transact-SQL-i abil andmeid sisaldav veerg.|
| [Dünaamiliste andmete kaitset häälestamine](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | Selles õppetükis saate teada, kuidas häälestada dünaamilise varjab Azure SQL-i andmebaasi. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Süsteemi järjepidevuse ja päringu skaala-Out

Järgmised õpetused tutvustame teile abil [Geo-taastamine ja aktiivse Geo-kopeerimine](sql-database-business-continuity.md) reccover vigu, järjepidevuse ja päringu skaala välja.

| Õppeteema  | Kirjeldus  |
|---|---|---|
| [Azure'i SQL-andmebaasi taastamiseks eelmises punktis aja Azure'i portaal](sql-database-point-in-time-restore-portal.md)| Selles õppetükis saate teada, kuidas andmebaasi taastamine varasemasse ajas Azure'i portaalis.|
| [Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajas PowerShelli abil](sql-database-point-in-time-restore-powershell.md) | Selles õppetükis saate teada, kuidas taastada andmebaasi varasemasse ajahetkel PowerShelli kaudu|
| [Kustutatud portaalis Azure SQL Azure'i andmebaasi taastamine](sql-database-restore-deleted-database-portal.md) | Selles õppetükis saate teada, kuidas taastada kustutatud andmebaasi Azure'i portaalis.|
| [Funktsiooni PowerShelli abil kustutatud SQL Azure'i andmebaasi taastamine](sql-database-restore-deleted-database-powershell.md) | Selles õppetükis saate teada, kuidas taastada kustutatud andmebaasi PowerShelli kaudu.|
| [Azure'i SQL-andmebaasi Azure'i portaalis Geo-Dispersioonanalüüs konfigureerimine](sql-database-geo-replication-portal.md)| Selles õppetükis saate teada, kuidas konfigureerida aktiivse Geo kopeerimine Azure'i portaalis.|
| [Azure'i SQL-andmebaasi PowerShelli kaudu Geo-Dispersioonanalüüs konfigureerimine](sql-database-geo-replication-powershell.md)| Selles õppetükis saate teada, kuidas konfigureerida aktiivse Geo-kopeerimine PowerShelli kaudu.|
| [Geo-kopeerimise abil Transact-SQL Azure'i SQL-andmebaasi konfigureerimine](sql-database-geo-replication-transact-sql.md)| Selles õppetükis saate teada, kuidas konfigureerida aktiivse Geo-kopeerimine Transact-SQL-i abil.|
| [Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Azure'i portaalis](sql-database-geo-replication-failover-portal.md) | Selles õppetükis saate teada, kuidas Tõrkesiirde Azure'i portaalis geo kopeeritud teise koopia.|
| [Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi PowerShelli abil](sql-database-geo-replication-failover-powershell.md) | Selles õppetükis saate teada, kuidas Tõrkesiirde geo kopeeritud teise koopia PowerShelli kaudu.|
| [Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Transact-SQL-i abil](sql-database-geo-replication-failover-transact-sql.md) | Selles õppetükis saate teada, kuidas Tõrkesiirde geo kopeeritud teise koopia Transact-SQL-i abil.|
||||

## <a name="data-sync"></a>Andmete sünkroonimine

Selles õppetükis saate teada, [Andmete sünkroonimise](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)kohta.

| Õppeteema  | Kirjeldus  |
|---|---|---| 
| [Alustamine: Azure SQL-i andmete sünkroonimine (eelvaade)](sql-database-get-started-sql-data-sync.md)  | Selles õppetükis saate teada, põhialuste Azure SQL-i andmete sünkroonimine Azure'i klassikaline portaalis. |
||||

## <a name="next-steps"></a>Järgmised sammud

[SQL Azure'i andmebaasi lahendus kiire käivitamise uurimine](sql-database-solution-quick-starts.md)
