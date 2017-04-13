<properties
    pageTitle="Kõigi teemade SQL-andmebaasi teenuse | Microsoft Azure'i"
    description="Tabeli kõigi teemade Azure'i teenuse nimega SQL-andmebaasiga, mis on olemas http://azure.microsoft.com/documentation/articles/, pealkiri ja kirjeldus."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Kõigi teemade teenuse Azure SQL-andmebaas

Selles teemas on loetletud iga teema, mis kehtib Azure **SQL-andmebaasi** teenuse. Saate otsida selle veebilehe märksõnade abil **Klahvikombinatsiooni Ctrl + F**, praegune huvi pakkuvate teemade otsimiseks.




## <a name="new"></a>Uue

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 1 | [Daxko/CSI kasutatud Azure'i oma arengutsükli kiirendamiseks ja selle klienditeenindus ja jõudluse parandamiseks](sql-database-implementation-daxko.md) | Teada, kuidas Daxko/CSI kasutab SQL-andmebaasi oma arengutsükli kiirendamiseks ja selle klienditeenindus ja jõudluse parandamiseks |
| 2 | [Azure'i annab GEP globaalne REACHi ja tõhusam](sql-database-implementation-gep.md) | Teada, kuidas GEP kasutab jõuda rohkem globaalne kliendid ja saavutada tõhusamalt SQL-andmebaas |
| 3 | [Azure, laiendatud SnelStart kiiresti oma business teenuste 1000 uue Azure SQL-i andmebaasid kuus määr](sql-database-implementation-snelstart.md) | Teada, kuidas SnelStart kasutab SQL-andmebaasi laiendatud kiiresti oma business teenuste 1000 uue Azure SQL-i andmebaasid kuus määr |
| 4 | [Umbraco kasutab Azure'i SQL-andmebaasi, et kiiresti näha ja skaala teenuste tuhandete rentnikud pilveteenuses](sql-database-implementation-umbraco.md) | Siit saate teada, kuidas Umbraco kasutab SQL-andmebaasi kiiresti ette valmistada ja skaala teenused tuhandete rentnikud pilveteenuses |
| 5 | [Azure'i SQL-andmebaasi PowerShelliga haldamine](sql-database-manage-powershell.md) | Azure SQL-i andmebaasi haldamine PowerShelli abil. |
| 6 | [SSMS tugi: Azure'i AD MFA SQL-andmebaas ja SQL-i andmebaas](sql-database-ssms-mfa-authentication.md) | Mitme tegureid autentimise kasutamine SSMS SQL-andmebaas ja SQL-i andmebaas. |
| 7 | [Selgitab andmebaasi tehingu (DTUs) elastne andmebaasi tehingu ühikute ja (eDTUs)](sql-database-what-is-a-dtu.md) | Millist Azure SQL-andmebaasi mõistmine tehingu üksus on. |


## <a name="updated-articles-sql-database"></a>Värskendatud artikleid, SQL-andmebaas

Selles jaotises on loetletud tooted, mis olid värskendanud, kui värskendus on suur või märgatavalt. Iga värskendatud artiklis töötlemata koodilõigu lisatud allahindlusest teksti kuvada. **2016-10-05** **2016 – 08-22** vahemikus date värskendati artikleid.

| &nbsp; | Artikkel | Värskendatud teksti, koodilõigu | Millal värskendatud |
| --: | :-- | :-- | :-- |
| 8 | [Azure'i SQL-andmebaasi PowerShelliga haldamine](sql-database-command-line-tools.md) | Looge ressursirühma meie SQL-andmebaasi ja seotud Azure ressursse cmdlet-käsu New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" New-AzureRmResourceGroup-nime $resourceGroupName-asukoht $resourceGroupLocation* Lisateavet leiate teemast Azure ressursihaldur Azure PowerShelli kasutamine (… / PowerShelli azure-ressursside manager.md). Proovi skripti, vaadake teemat SQL-andmebaasi PowerShelli skripti (sql-andmebaasi-get-alustamine – powershell.md create-a-sql-database-powershell-script). **SQL-andmebaasi server loomine** Saate luua SQL-andmebaasi server cmdlet-käsu New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Asendage *server1* oma serveri nimi. Serverite nimed peavad olema kordumatud kõik Azure'i SQL-andmebaasi serverites. Kui serveri nimi on juba kasutusel, kuvatakse järgmine tõrketeade. See käsk võib kuluda mitu minutit. Funktsiooni resou | 2016-09-14 |
| 9 | [Kopeerige PowerShelli kaudu Azure SQL-i andmebaas](sql-database-copy-powershell.md) |   SQL-i andmebaasi päritolu (olemasoleva andmebaasi kopeerimiseks)--$sourceDbName = "DEG1" $sourceDbServerName = "server1" $sourceDbResourceGroupName = "rg1" SQL-i andmebaasi kopeerimine (uus db luua)--$copyDbName = "db1_copy" $copyDbServerName = "server2" $copyDbResourceGroupName = "rg2" Kopeeri andmebaasi sama server--New-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - serveri nimi $sourceDbServerName - DatabaseName $sourceDbName - CopyDatabaseName $copyDbName kopeerimine andmebaasi eri Server--New-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - serverinimi $sourceDbServerName - DatabaseName $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName kopeerige andmebaasi kohapeal on elastne andmebaasi--$poolName = "pool1" New-AzureRmSqlDatabaseCopy - ResourceGroupName $ sourceDbResourceGroupName - serveri nimi $sourceDbServerName - DatabaseName $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [C# kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("serveri tulemüür...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("serveri tulemüüri:" + fwr. FirewallRule.Id);  Console.WriteLine ("elastne pool...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("elastne rakenduskausta:" + epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("andmebaasi:" + dbr. Database.Id);  Console.WriteLine ("vajutage suvalist klahvi jätkata...");  Console.ReadKey();  } staatilise ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09-14 |
| 11 | [PowerShelli skripti andmebaaside sobib kohapeal on elastne andmebaasi tuvastamine](sql-database-elastic-pool-database-assessment-powershell.md) | **Jaoks elastne andmebaasi rakenduskausta leviloendite, me välistada juhtslaidi ja mis tahes andmebaase, mis on juba pool. Võite lisada rohkem andmebaase välistatud allolevas vastavalt vajadusele** $ListOfDBs = Get-AzureRmSqlDatabase - serveri nimi $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / Where-objekti {$_. DatabaseName-notin ("meister") - ja $_. CurrentServiceLevelObjectiveName-notin ("ElasticPool") - ja $_. CurrentServiceObjectiveName-notin ("ElasticPool")} $outputConnectionString = "andmeallika = $outputServerName; integreeritud turvalisus = false; algse kataloogi = $outputdatabaseName; Kasutaja ID-d = $outputDBUsername; Parooli = $outputDBpassword "$destinationTableName ="resource_stats_output" **mõõdikute Saidikogumi väljundi andmebaasi tabeli loomine** $sql =" kui võtmesõna NOT EXISTS (valige * kaudu sys.objects WHERE object_id = OBJECT_ID(N'$($destinationTableName)') ja tippige (N'U ")) ALUSTAGE saate luua tabeli $($destinationTableName) (database_name varchar(128) slo varchar(20), end_time kuupäeva ja kellaaja, avg_cpu hõljumine, avg_ | 2016-09-29 |
| 12 | [SQL-andmebaasi õpetus: Azure'i portaali kaudu luua SQL-andmebaasi minutit](sql-database-get-started.md) |   ! Uue sql-andmebaasi 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Klõpsake käsku **SQL-andmebaasi** SQL-andmebaasi tera avamiseks. See blade sisu erineb sõltuvalt arvu tellimuste ja teie olemasoleva objektide (nt olemasoleva serverid).  ! Uue sql-andmebaasi 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. Sisestage väljale **andmebaasi nimi** nimi, esimese andmebaasi – näiteks "minu andmebaasi". Roheline märge näitab, et olete andnud sobiv nimi.  ! Uue sql-andmebaasi 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Kui teil on mitu tellimust, valige tellimus. 6. **Ressursirühm**, all nuppu **Loo uus** ja sisestage esimese ressursirühma – näiteks "minu--ressursirühm" nimi. Roheline märge näitab, et olete andnud sobiv nimi.  ! Uue sql-andmebaasi 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. Klõpsake jaotises **allika valimine* | 2016-09-08 |
| 13 | [Proovige: SQL-andmebaasi Kasutamine C# SQL-andmebaasi loomine SQL-andmebaasi teegiga .net-i jaoks](sql-database-get-started-csharp.md) | **Peamine ressursid juurdepääsu teenuse loomine** Järgmist PowerShelli skripti loob Active Directory (AD) rakenduse ja teenuse põhilise, mida läheb vaja autentida meie C rakendus. Skripti väljundid läheb vaja eelnev C valimi väärtused. Üksikasjaliku teabe leiate teemast Azure PowerShelli kasutamine loomine teenuse põhilise juurdepääs ressurssidele (… / ressursi-rühma-autentida-teenus-principal.md).  Azure'i sisse logida.  Lisage AzureRmAccount kui teil on mitu tellimust, eemaldamine ja seatud tellimus, mida soovite töötada.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" Set-AzureRmContext - SubscriptionId $subscriptionId sisesta need väärtused AAD uus rakendus.  $appName on kuvatav nimi oma rakenduse, peab olema kordumatu kataloogis.  $uri ei pea olema reaal uri.  $secret on loote parool.  $appName = "{rakenduse nimi}" $uri = "http://{app-name}" $secret = "{rakenduse parooli}" Loo AAD rakenduse $azureAdApplication = New-AzureRmADApp | 2016-09-23 |
| 14 | [Azure'i portaalis Azure SQL-i andmebaaside haldamine](sql-database-manage-portal.md) | Saate vaadata või värskendada oma andmebaasi sätteid, klõpsake soovitud sätet SQL-i andmebaasi enne:! SQL-i andmebaasi sätted (. / media/sql-database-manage-portal/settings.png) **Kuidas otsida SQL andmebaase serveri täielik nimi?** Oma andmebaase serveri nime kuvamiseks klõpsake nuppu **Ülevaade** **SQL-andmebaasi** enne ja serveri nimi:! SQL-i andmebaasi sätted (. / media/sql-database-manage-portal/server-name.png) **Kuidas hallata reegleid tulemüüri juurdepääsu minu SQL server ja andmebaas?** Vaatamine, loomine või värskendamine tulemüüri reeglid, **SQL-andmebaasi** enne klõpsake **serveri tulemüüri seadmine** . Lisateavet leiate teemast konfigureerimine Azure'i SQL-andmebaasi server taseme tulemüüri reegli Azure'i portaalis (sql-andmebaasi-konfigureerimine-tulemüüri-settings.md). ! tulemüüri reeglid (. / media/sql-database-manage-portal/sql-database-firewall.png) **Kuidas muuta oma SQL-i andmebaasi taseme või jõudluse teenusetaseme?** Esimese taseme või jõudluse teenusetaseme SQL-i andmebaasi värskendamiseks klõpsake ** hinnakirjad taseme (s | 2016-09-20 |





## <a name="get-started"></a>Alustamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 15 | [Koostab Azure SQL-andmebaasi eraldamise ja tõhusalt koos mitme rentniku rakendused](sql-database-build-multi-tenant-apps.md) | Siit saate teada, kuidas SQL-andmebaasi koostab mitme rentniku rakendused |
| 16 | [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu käivitada](sql-database-connect-query-ssms.md) | Saate teada, kuidas Azure SQL-andmebaasiga ühenduse loomine SQL Server Management Studio (SSMS) abil. Käivitage valimi päringu abil Transact-SQL-i (T-SQL-i). |
| 17 | [Ühenduse loomine SQL-andmebaasiga, kasutades .NET (C#)](sql-database-develop-dotnet-simple.md) | Proovi kood see Kiire alustamine koostamiseks kaasaegne rakendus C# ja toetavad võimsad relatsiooniandmebaasist pilveteenuses koos Azure'i SQL-andmebaasi kasutada. |
| 18 | [Uue andmebaasi elastne rakenduskausta Azure portaali loomine](sql-database-elastic-pool-create-portal.md) | Kuidas lisada oma SQL-i andmebaasi konfigureerimine haldus ja ressursside ühiskasutamine mitme andmebaaside scalable elastne andmebaasi pool. |
| 19 | [SQL Azure'i andmebaasi õpetused uurimine](sql-database-explore-tutorials.md) | Siit saate teada, SQL-andmebaasi funktsioonide ja võimaluste kohta |
| 20 | [SQL-andmebaasi õpetus: Azure'i portaali kaudu luua SQL-andmebaasi minutit](sql-database-get-started.md) | Saate teada, kuidas häälestada loogilise SQL-andmebaasi server server tulemüüri reegel SQL-andmebaasi ja näidisandmed. Samuti saate teada, kuidas ühendada kliendi tööriistad, konfigureerimine kasutajatele ja andmebaasi tulemüüri reegli häälestamine. |
| 21 | [Azure'i SQL-andmebaasi tagab ja kaitseb](sql-database-helps-secures-and-protects.md) | Siit saate teada, kuidas SQL-andmebaas aitab tagada ja kaitsmine |
| 22 | [Azure'i SQL-andmebaasi õpib &amp; kohandub](sql-database-learn-and-adapt.md) | Siit saate teada, kuidas SQL-andmebaasi õpib ja kohandub |
| 23 | [Valige SQL Server suvand pilv: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Siit saate teada, millised cloud SQL Server suvand sobib teie taotlus: (PaaS) Azure SQL-i andmebaasi või SQL Server Azure'i Virtuaalmasinates pilveteenuses. |
| 24 | [SQL Azure'i andmebaasi skaalasid pealt](sql-database-scale-on-the-fly.md) | Siit saate teada, kuidas SQL-andmebaasi skaala pealt |
| 25 | [SQL Azure'i andmebaasi lahendus kiire käivitamise uurimine](sql-database-solution-quick-starts.md) | Lisateavet SQL Azure'i andmebaasi lahendused |
| 26 | [SQL Azure'i andmebaas töötab teie keskkonnas](sql-database-works-in-your-environment.md) | Siit saate teada, kuidas SQL-andmebaasiga, tagab ja kaitsevad |



## <a name="build-an-app"></a>Rakenduse koostamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 27 | [Tõrkeotsing, diagnoosimine ja SQL-i andmebaasi SQL-i ühenduse tõrgete ja siirdamiseks tõrgete vältimiseks](sql-database-connectivity-issues.md) | Saate teada, kuidas tõrkeotsing, diagnoosimine ja vältida siirdamiseks tõrge Azure SQL andmebaasis või SQL-i ühenduse tõrge.  |
| 28 | [Ühenduse loomine SQL-andmebaasi Visual Studio abil](sql-database-connect-query.md) | Kirjutage programmi C# päringu ja SQL-andmebaasiga ühenduse loomiseks. Teave IP aadressid, ühendusstringi, turvalist sisselogimine ja tasuta Visual Studio kohta. |
| 29 | [Lisaks 1433 ADO.NET 4.5 ja SQL-i andmebaasi V12 pordid](sql-database-develop-direct-route-ports-adonet-v12.md) | Kliendi ühenduste ADO.net-i Azure SQL-i andmebaasi v12 mõnikord puhverserverist mööduda ja andmebaasi suhelda. Pordid 1433 peale muutuvad oluline. |
| 30 | [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide](sql-database-develop-error-messages.md) | Teavet SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes, nt levinud andmebaasi ühenduse tõrkeid, andmebaasi Kopeeri probleemid ja üldisi tõrkeid.  |
| 31 | [Ühenduse loomine SQL-andmebaasiga, kasutades PHP opsüsteemis Windows](sql-database-develop-php-simple.md) | Esitab valimi PHP programmi, mis ühendub klientrakenduses Windows Azure'i SQL-andmebaasi ja lingid kliendile vajalikku vajalik tarkvara komponendid. |
| 32 | [Proovige: SQL-andmebaasi Kasutamine C# SQL-andmebaasi loomine SQL-andmebaasi teegiga .net-i jaoks](sql-database-get-started-csharp.md) | Proovige SQL-andmebaasi SQL-i ja C# rakenduste arendamise ja C# .net-i SQL-andmebaasi teegi kasutamise Azure'i SQL-andmebaasi loomine. |
| 33 | [Azure'i SQL-andmebaasi JSON funktsioonide kasutamise alustamine](sql-database-json-features.md) | SQL Azure'i andmebaas võimaldab sõeluda, päringu ja andmete vormindamine JavaScript Object märke (JSON) esituses. |
| 34 | [Azure'i SQL-andmebaasiga ühenduse tõrkeotsing](sql-database-troubleshoot-common-connection-issues.md) | Juhised ära tunda ja lahendada Azure SQL-i andmebaasi ühendus levinud tõrkeid. |
| 35 | [Tõrge "Andmebaasi server pole praegu saadaval" sql-andmebaasiga ühenduse loomisel](sql-database-troubleshoot-connection.md) | Tõrkeotsingut andmebaasi server pole praegu saadaval tõrge, kui rakendus loob SQL-andmebaasiga. |



## <a name="develop"></a>Töötada

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 36 | [Nõutav tuua autentimiseks rakenduse juurde pääseda SQL-andmebaasi kood](sql-database-client-id-keys.md) | Looge teenuse põhilise juurdepääsuks SQL-andmebaasi kood. |
| 37 | [Kasutades Java JDBC Windows SQL-andmebaasiga ühenduse loomine](sql-database-develop-java-simple.md) | Esitab Java koodi valimi abil saate Azure SQL-i andmebaasiga ühenduse loomiseks. Valimi kasutab JDBC ja see töötab Windows klientarvutis. |
| 38 | [Ühenduse loomine SQL-andmebaasi Node.js abil](sql-database-develop-nodejs-simple.md) | Esitab Node.js kood valimi abil saate Azure SQL-i andmebaasiga ühenduse loomiseks. |
| 39 | [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md) | Teavet saadaval connectivity teekide ja head tavad rakenduste SQL-andmebaasiga ühenduse loomine. |
| 40 | [Ühenduse loomine SQL-andmebaasi Python abil](sql-database-develop-python-simple.md) | Esitab Python kood valimi abil saate Azure SQL-i andmebaasiga ühenduse loomiseks. |
| 41 | [Ühenduse loomine SQL-andmebaasi Ruby abil](sql-database-develop-ruby-simple.md) | Pange valimil foneetiline koodi käivitada Azure SQL-i andmebaasiga ühenduse loomiseks. |
| 42 | [Ajaline Azure SQL-andmebaasi tabelitega töötamise alustamine](sql-database-temporal-tables.md) | Saate teada, kuidas alustada Azure'i SQL-andmebaasi ajaline tabelite abil. |



## <a name="performance-and-scale"></a>Jõudlus ja mastaapimine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 43 | [SQL-i andmebaasi Advisor](sql-database-advisor.md) | Advisor Azure SQL-i andmebaas sisaldab soovitusi jaoks oma olemasoleva SQL andmebaase, mida saate praeguse päringu jõudlust parandada. |
| 44 | [SQL-i andmebaasi Advisor Azure'i portaalis](sql-database-advisor-portal.md) | Azure'i portaalis Azure SQL-i andmebaasi Advisor saate läbi vaadata ja rakendada oma olemasolevaid SQL andmebaase, saate praeguse päringu jõudlust parandada. |
| 45 | [Azure'i SQL-andmebaasi võrdlusalus ülevaade](sql-database-benchmark-overview.md) | Selles teemas kirjeldatakse kasutatud Azure'i SQL-andmebaasi toimimist SQL Azure'i andmebaasi võrdlusalus. |
| 46 | [Kui kohapeal on elastne andmebaasi kasutada?](sql-database-elastic-pool-guidance.md) | Kohapeal on elastne andmebaas on saadaval ressursse, mis on ühiselt rühma elastne andmebaaside kogum. Selles dokumendis on toodud juhiseid, et aidata kohapeal on elastne andmebaasi kasutamise rühma andmebaaside sobivuse hindamiseks. |
| 47 | [Elastne Andmebaasiriistad kasutamise alustamine](sql-database-elastic-scale-get-started.md) | Ülevaade elastne andmebaasi tööriistad funktsiooni Azure'i SQL-andmebaasi, sh lihtne valimi rakenduse käivitamiseks. |
| 48 | [SQL-andmebaasi KKK](sql-database-faq.md) | Vastused levinud küsimustele kliendid küsida cloud andmebaasid ja Azure'i SQL-andmebaasi, Microsofti relatsiooniandmebaasist halduse süsteem (RDBMS) ja andmebaasi teenuse pilves. |
| 49 | [Alustamine-mälu (eelvaade) SQL-andmebaasis](sql-database-in-memory.md) | SQL-mälu tehnoloogiad oluliselt parandada jõudlust selgituseks ja analytics töökoormus. Saate teada, kuidas nende tehnoloogiate eeliseid. |
| 50 | [Kasutage-mälu OLTP (eelvaade) SQL-andmebaasis rakenduse jõudluse parandamiseks](sql-database-in-memory-oltp-migration.md) | Vastu-mälu OLTP olemasoleva SQL-andmebaasis selgituseks jõudluse parandamiseks. |
| 51 | [Kuvari-mälu OLTP salvestusruum](sql-database-in-memory-oltp-monitoring.md) | Hinnangu ja kuvari XTP-mälu salvestusruumi kasutada, võimsuse; tõrke võimsus 41823 |
| 52 | [Opsüsteem Azure SQL-andmebaasis päringu pood](sql-database-operate-query-store.md) | Siit saate teada, kuidas toimida päringu poe Azure SQL-andmebaasis |
| 53 | [SQL-i andmebaasi jõudluse tutvustus](sql-database-performance.md) | Azure'i SQL-andmebaasi pakub jõudluse tööriistu, mis aitavad teil tuvastada ala, mida saate praeguse päringu jõudlust parandada. |
| 54 | [Azure'i SQL-andmebaasi ja jõudluse ühe andmebaaside jaoks](sql-database-performance-guidance.md) | See artikkel aitab teil otsustada, milline teenuse kiht rakenduse valimiseks. Samuti soovitab, kuidas häälestada oma rakenduse toomine kõige Azure'i SQL-andmebaasi. |
| 55 | [SQL Azure'i andmebaasi päringu jõudluse tutvustus](sql-database-query-performance.md) | Päringu jõudluse jälgimise tuvastab enamik CPU tarbimine päringute SQL Azure'i andmebaasi. |
| 56 | [Teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) SQL-andmebaasiga](sql-database-scale-up.md) | Muuta teenuse kiht ja jõudluse taseme Azure SQL-andmebaasi näitab, kuidas mastaapimiseks SQL-andmebaasi üles või alla. SQL Azure'i andmebaasi hinnakirjad taseme muutmine. |
| 57 | [Andmebaasi jõudluse Azure'i SQL-andmebaasi jälgimine](sql-database-single-database-monitor.md) | Siit saate teada, jälgida oma andmebaasi Azure tööriistad ja dünaamilise halduse vaadete võimaluste kohta. |
| 58 | [SQL-andmebaasi jõudluse häälestamine näpunäited](sql-database-troubleshoot-performance.md) | Näpunäiteid jõudluse häälestamine Azure SQL-andmebaasis hindamise ja parandamise kohta. |
| 59 | [Kuidas kasutada partiide SQL-andmebaasi rakenduse jõudluse parandamiseks](sql-database-use-batching-to-improve-performance.md) | Teema sisaldab tõendusmaterjalide kogumiseks, et partiide andmebaasi toimingud oluliselt imroves kiiruse ja skaleeritavus Azure'i SQL-andmebaasi rakenduste. Kuigi need partiide meetodid mõnda SQL serveri andmebaasi, on see artikkel keskendub Azure. |



## <a name="tools-for-scaling-out"></a>Skaala läbi tööriistad

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 60 | [Kujundage mustrite rentnikuga SaaS rakenduste ja Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md) | Selles artiklis käsitletakse nõuded ja levinud andmete arhitektuur mustrid rentnikuga andmebaasi rakenduste töötamine cloud keskkonnas tuleb arvesse võtta ja erinevate kompromissidega, mis on seotud neid mustreid. See ka selgitatakse, kuidas Azure'i SQL-andmebaasi selle elastne kaustu ja elastne tööriistad, nendele nõuetele no kompromiss mood aadress. |
| 61 | [Olemasolevaid andmebaase skaala-out migreerimine](sql-database-elastic-convert-to-use-elastic-tools.md) | Sharded kasutada elastne Andmebaasiriistad loomisega on Kildu kaardi halduri andmebaaside teisendamine |
| 62 | [Koosteüksuse scalable cloud andmebaasid](sql-database-elastic-database-client-library.md) | Skaleeritav .net-i andmebaasi rakenduste teegiga elastne andmebaasi kliendi koostamine |
| 63 | [Jõudluse hinnale Kildu kaardi Manager](sql-database-elastic-database-perf-counters.md) | ShardMapManager klassi ja andmed sõltuvad marsruutimise jõudluse hinnale |
| 64 | [Lisada mõne Kildu elastne Andmebaasiriistad abil](sql-database-elastic-scale-add-a-shard.md) | Kuidas elastne skaala API abil saate lisada uue shards on Kildu määrata. |
| 65 | [Tükeldatud ühendamine teenuste juurutamine](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Tükeldamine ja koos elastne Andmebaasiriistad ühendamine |
| 66 | [Andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md) | Kuidas kasutada ShardMapManager klassi .NET rakendustes andmed sõltuvad marsruutimine, funktsioon elastne andmebaaside jaoks Azure'i SQL-andmebaasi |
| 67 | [Elastne andmebaasi vahendid KKK](sql-database-elastic-scale-faq.md) | Korduma kippuvad küsimused SQL Azure'i andmebaasi elastne skaala. |
| 68 | [Elastne andmebaasi tööriistad sõnastik](sql-database-elastic-scale-glossary.md) | Kasutatud elastne Andmebaasiriistad selgitus |
| 69 | [Skaala läbi Azure SQL-i andmebaasiga](sql-database-elastic-scale-introduction.md) | Service (SaaS) arendajate tarkvara saate hõlpsasti luua elastne, scalable andmebaaside nende tööriistade abil pilves |
| 70 | [Mandaadid elastne andmebaasi kliendi teek](sql-database-elastic-scale-manage-credentials.md) | Kuidas määrata õige mandaat, ainult lugemiseks, elastne andmebaasi rakenduste administraator |
| 71 | [Mitme Kildu päringud](sql-database-elastic-scale-multishard-querying.md) | Päringute sooritamine üle shards abil elastne andmebaasi kliendi teek. |
| 72 | [Mastaabitud kontorist pilve andmebaaside andmete liikumine](sql-database-elastic-scale-overview-split-and-merge.md) | Selles teemas kirjeldatakse töödelda shards ja ise hostitud vahendusel elastne andmebaasi API-de kasutamine andmete teisaldamiseks. |
| 73 | [Välja Kildu kaardi halduri andmebaaside skaala](sql-database-elastic-scale-shard-map-management.md) | Kuidas kasutada ShardMapManager, elastne andmebaasi kliendi teek |
| 74 | [Tükeldatud ühendamine turvalisus konfigureerimine](sql-database-elastic-scale-split-merge-security-configuration.md) | Häälestamine x409 krüptimise serdid |
| 75 | [Rakenduse uusim elastne andmebaasi kliendi teegi kasutamine täiendamine](sql-database-elastic-scale-upgrade-client-library.md) | Rakenduste ja teekide abil Nugeti täiendamine |
| 76 | [Elastne andmebaasi kliendi teek koos üksuse raames](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Elastne andmebaasi kliendi teek ja üksuse raames kodeerimise andmebaasi kasutamiseks |
| 77 | [Kasutades Dapper elastne andmebaasi kliendi teek](sql-database-elastic-scale-working-with-dapper.md) | Rakendust Dapper elastne andmebaasi kliendi teek. |
| 78 | [Mitme rentniku rakenduste elastne Andmebaasiriistad ja rea taseme turvalisus](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Saate teada, kuidas luua rakenduse väga paindlik andmete taseme koos Azure'i SQL-andmebaasi, mis toetab mitme rentniku shards elastne Andmebaasiriistad koos rea taseme turvalisus abil. |



## <a name="elastic-jobs"></a>Elastne tööde haldamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 79 | [Saate luua ja hallata mastaabitud välja SQL Azure'i andmebaasid, kasutades elastne tööd (eelvaade)](sql-database-elastic-jobs-create-and-manage.md) | Tutvustavad loomist ja haldamist ka elastne andmebaasi töö. |
| 80 | [Elastne andmebaasi töö alustamine](sql-database-elastic-jobs-getting-started.md) | Kuidas kasutada elastne andmebaasi tööde haldamine |
| 81 | [Mastaabitud kontorist pilve andmebaaside haldamine](sql-database-elastic-jobs-overview.md) | Näitab elastne andmebaasi töö teenus |
| 82 | [Saate luua ja hallata SQL-andmebaasi elastne andmebaasi töö abil PowerShelli (eelvaade)](sql-database-elastic-jobs-powershell.md) | PowerShelli abil saate hallata kaustu Azure SQL-andmebaas |
| 83 | [Installimise elastne andmebaasi töö ülevaade](sql-database-elastic-jobs-service-installation.md) | Tutvustavad elastne töö funktsiooni installimise. |
| 84 | [Desinstallige elastne andmebaasi töö komponendid](sql-database-elastic-jobs-uninstall.md) | Kuidas desinstallida elastne andmebaasiriista tööde haldamine |



## <a name="elastic-pools"></a>Elastne kaustu

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 85 | [Mis on Azure elastne kohapeal on?](sql-database-elastic-pool.md) | Sadu või tuhandeliste abil on andmebaaside haldamine. Hind ühe kogum, jõudlusühikud saate jaotada pool. Liikumine andmebaaside sisse või välja juures kuvatakse. |
| 86 | [C# kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool-create-csharp.md) | C# meetodid andmebaasi abil saate luua scalable elastne andmebaasi rakenduskausta Azure SQL-andmebaasis ressursid saate ühiskasutada kogu paljud andmebaasid. |
| 87 | [Luua uue andmebaasi elastne rakenduskausta PowerShelli abil](sql-database-elastic-pool-create-powershell.md) | Saate teada, kuidas PowerShelli abil skaala-out Azure'i SQL-andmebaasi ressursse, luues scalable elastne andmebaasi rakenduskausta mitme andmebaaside haldamine. |
| 88 | [PowerShelli skripti andmebaaside sobib kohapeal on elastne andmebaasi tuvastamine](sql-database-elastic-pool-database-assessment-powershell.md) | Kohapeal on elastne andmebaas on saadaval ressursse, mis on ühiselt rühma elastne andmebaaside kogum. Selles dokumendis on toodud PowerShelli skripti abil kohapeal on elastne andmebaasi kasutamise rühma andmebaaside sobivuse hindamiseks. |
| 89 | [Jälgimine ja haldamine on elastne andmebaasi rakenduskausta C#](sql-database-elastic-pool-manage-csharp.md) | C# meetodid andmebaasi abil saate hallata elastne andmebaasi kohapeal on Azure SQL-andmebaas. |
| 90 | [Jälgimine ja haldamine on elastne andmebaasi rakenduskausta Azure'i portaalis](sql-database-elastic-pool-manage-portal.md) | Teada, kuidas Azure'i portaal ja SQL-andmebaasi sisseehitatud ärianalüüsi abil hallata, jälgida ja paremalt-suurus scalable elastne andmebaasi pool optimeerimine andmebaasi jõudlus ja hallata maksumus. |
| 91 | [Jälgimine ja haldamine on elastne andmebaasi rakenduskausta PowerShelli abil](sql-database-elastic-pool-manage-powershell.md) | Saate teada, kuidas kohapeal on elastne andmebaasi haldamine PowerShelli abil. |
| 92 | [Jälgimine ja haldamine on elastne andmebaasi rakenduskausta Transact-SQL-i abil](sql-database-elastic-pool-manage-tsql.md) | T-SQL-i abil saate luua Azure'i SQL-andmebaasiga on elastne kogumi. Või T-SQL-i abil teisaldada selle datbase ja sealt kaustu. |
| 93 | [Elastne andmebaasi rakenduskausta arvelduste ja hinnakirjad teave](sql-database-elastic-pool-price.md) | Teavet teatud elastne andmebaasi kaustu. |



## <a name="elastic-query"></a>Elastne päring

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 94 | [Aruande üle mastaabitud kontorist pilve andmebaasid (eelvaade)](sql-database-elastic-query-getting-started.md) | Kuidas kasutada andmebaasi andmebaasipäringud rist |
| 95 | [Alustamine rist andmebaasipäringud (vertikaalne eraldamine) (eelvaade)](sql-database-elastic-query-getting-started-vertical.md) | elastne andmebaasi päringu kasutamine vertikaalselt liigendatud andmebaasid |
| 96 | [Aruande kõikide mastaabitud kontorist pilve andmebaasid (eelvaade)](sql-database-elastic-query-horizontal-partitioning.md) | Kuidas häälestada elastne päringute üle horisontaalne sektsioonid |
| 97 | [Azure'i SQL-andmebaasi elastne andmebaasi päringu ülevaade (eelvaade)](sql-database-elastic-query-overview.md) | Funktsiooni elastne päringu ülevaade |
| 98 | [Päringu tegemine cloud andmebaasid eri skeemide (eelvaade)](sql-database-elastic-query-vertical-partitioning.md) | Kuidas häälestada rist andmebaasipäringud vertikaalne sektsioonide üle |



## <a name="elastic-transaction"></a>Elastne tehingu

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 99 | [Jaotatud tehingud üle pilve andmebaasid](sql-database-elastic-transactions-overview.md) | Ülevaade elastne andmebaasi tehingud Azure'i SQL-andmebaas |



## <a name="backup-and-recovery"></a>Varundamine ja taastamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 100 | [SQL-i andmebaasi varundamine](sql-database-automated-backups.md) | Teavet SQL-andmebaasi sisseehitatud andmebaasi varundamine, mis võimaldavad teil tööks ajas tagasi Azure'i SQL-andmebaasi eelmises punktis või andmebaasi kopeerimiseks uue andmebaasi geograafilised piirkonnas (kuni 35 päeva). |
| 101 | [Ettevõtte järjepidevus Azure'i SQL-andmebaasi ülevaade](sql-database-business-continuity.md) | Siit saate teada, kuidas Azure'i SQL-andmebaasi toetab cloud järjepidevuse ja andmebaasi taastamine ja aitab hoida olulise pilve rakendus. |
| 102 | [Ühe tabeli Azure SQL-i andmebaasi varukoopia põhjal taastamine](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Saate teada, kuidas Azure'i SQL-andmebaasi varukoopia põhjal taastamine üheks tabeliks. |
| 103 | [Kujundage taotlus cloud avariitaastet kasutamine SQL-andmebaasi aktiivse Geo-kopeerimine](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Saate teada, kuidas kujundada cloud katastroofi taastamine lahendused äri järjepidevuse planeerimine geo-kopeerimise abil rakenduse andmete varundamine koos Azure'i SQL-andmebaasi. |
| 104 | [Teisese Tõrkesiirde või Azure'i SQL-andmebaasi taastamine](sql-database-disaster-recovery.md) | Saate teada, kuidas andmebaasi taastamine piirkondliku andmekeskuse katkestuste või nurjumise Azure SQL-i andmebaasi aktiivse Geo-kopeerimine ja Geo-taaste võimalusi. |
| 105 | [Läbimiseks Drill katastroofi taastamine](sql-database-disaster-recovery-drills.md) | Lugege juhiseid ja katastroofi taastamine harjutused, mis aitavad teha Azure'i SQL-andmebaasi kasutamise head tavad säilitada oma ülesanne kriitiliste tõrgete ja katkestuste olles ärirakenduste. |
| 106 | [Katastroofi taastamine rakendustes, mis kasutavad SQL-i andmebaasi elastne rakenduskausta strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Saate teada, kuidas kujundada oma pilve lahendus tõrkejärgseks, valides õige Tõrkesiirde mustri. |
| 107 | [Klassi RecoveryManager abil Kildu kaardi probleemide lahendamine](sql-database-elastic-database-recovery-manager.md) | Kasutada RecoveryManager klassi Kildu kaartide seotud probleemide lahendamine |
| 108 | [Azure'i SQL-andmebaasi loomiseks BACPAC faili importimine](sql-database-import.md) | Azure'i SQL-andmebaasi loomiseks olemasoleva BACPAC faili importimine. |
| 109 | [Azure'i SQL-andmebaasi taastamiseks eelmises punktis aja Azure'i portaal](sql-database-point-in-time-restore-portal.md) | Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajal. |
| 110 | [Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajas PowerShelli abil](sql-database-point-in-time-restore-powershell.md) | Eelmises punktis seisu Azure'i SQL-andmebaasi taastamine |
| 112 | [Azure'i SQL-andmebaasiga, kasutades automatiseeritud andmebaasi taastamine](sql-database-recovery-using-backups.md) | Lisateavet punkti õigel ajal taastada, mis võimaldab tagasi pöörata Azure'i SQL-andmebaasi eelmises punktis ajas (kuni 35 päeva). |
| 113 | [Kustutatud portaalis Azure SQL Azure'i andmebaasi taastamine](sql-database-restore-deleted-database-portal.md) | Taastada kustutatud Azure SQL-andmebaasi (Azure'i portaal). |
| 114 | [PowerShelli abil kustutatud Azure SQL-i andmebaasi taastamine](sql-database-restore-deleted-database-powershell.md) | Taastada kustutatud Azure SQL-i andmebaasi (PowerShelli). |
| 115 | [Andmebaasi taastamine ajas eelmises punktis, kustutatud andmebaasi taastamine või andmete keskmist katkestuste taastamine](sql-database-troubleshoot-backup-and-restore.md) | Saate teada, kuidas pilve andmebaasi taastamine tõrgete ja katkestuste kasutamine Azure'i SQL-andmebaasi varukoopiate ja koopiad. |



## <a name="migrate"></a>Migreerimine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 116 | [SQL serveri andmebaasi eksportida BACPAC faili SqlPackage abil](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, andmebaasi eksportida, eksportida BACPAC faili sqlpackage |
| 117 | [SQL serveri andmebaasi eksportida BACPAC faili SQL Server Management Studio abil](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, andmebaasi eksportida, eksportida BACPAC faili eksportida andmed rakendus viisard |
| 118 | [SQL-andmebaasiga abil SqlPackage BACPAC faili importimine](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, andmebaasi importida, sqlpackage BACPAC faili importimine |
| 119 | [Importimine BACPAC abil SSMS SQL-andmebaasiga](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi juurutada, andmebaasi migreerimine andmebaasi import ekspordi andmebaasi migreerimine viisard |
| 120 | [SQL serveri andmebaasi migreerimine SQL-andmebaasiga, kasutades Microsoft Azure'i andmebaasi viisard andmebaasi juurutamine](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, Microsoft Azure'i andmebaasi viisard |
| 121 | [SQL serveri andmebaasi migreerimine Azure'i SQL-andmebaasi tiražeerimise abil](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, importimine andmebaasi tiražeerimise |
| 122 | [SQL-andmebaasi ühilduvuse abil SqlPackage.exe määratlemine](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, SQL-andmebaasi ühilduvust, SqlPackage |
| 123 | [SQL Server Management Studio abil saate määratleda SQL-andmebaasi ühilduvust enne migreerimise Azure SQL-andmebaasiga](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, SQL-andmebaasi ühilduvust, taseme rakenduse ekspordiviisardi andmeid |
| 124 | [SQL Azure'i migreerimise viisardi abil saate lahendada SQL serveri andmebaasi ühilduvusega seotud probleemid enne migreerimise Azure SQL-andmebaasiga](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, ühilduvust, SQL Azure'i migreerimise viisard |
| 125 | [SQL serveri andmebaasi migreerimine SQL Server Data Tools for Visual Studio abil Azure SQL-andmebaasiga](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, ühilduvust, SQL Azure'i migreerimise viisardis SSDT |
| 126 | [SQL Server Management Studio abil enne migreerimise SQL-andmebaasiga SQL serveri andmebaasi ühilduvusega seotud probleemide lahendamine](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, ühilduvust, SQL Azure'i migreerimise viisard |
| 127 | [Azure'i SQL-andmebaasiga BACPAC faili arhiivida PowerShelli abil](sql-database-export-powershell.md) | Azure'i SQL-andmebaasiga BACPAC faili arhiivida PowerShelli abil |
| 128 | [Azure'i SQL-andmebaasi loomiseks PowerShelli abil BACPAC faili importimine](sql-database-import-powershell.md) | Azure'i SQL-andmebaasi loomiseks PowerShelli abil BACPAC faili importimine |
| 129 | [CSV andmete laadimine (tasapinnalise failid) SQL Azure'i andmebaas](sql-database-load-from-csv-with-bcp.md) | Suurus: väike andmete jaoks kasutatakse andmete importimiseks Azure'i SQL-andmebaasi bcp. |
| 130 | [Tellimuste vahel ja sealt Azure serverites andmebaaside teisaldamine](sql-database-troubleshoot-moving-data.md) | Kiirtoimingud kopeerida, teisaldada ja andmete ja andmebaasidega Azure'i SQL-andmebaasi migreerimine. |



## <a name="move-data-in-and-out"></a>Andmete teisaldamine ja vähendamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 131 | [SQL serveri andmebaasi migreerimine SQL-andmebaasiga pilveteenuses](sql-database-cloud-migrate.md) | Siit saate teada, kuidas on lood asutusesisese SQL serveri andmebaasi migreerimine Azure SQL-andmebaasiga pilveteenuses. Testida enne andmebaasi migreerimine ühilduvus Andmebaasiriistad migreerimise abil. |
| 132 | [Kopeerige Azure'i SQL-andmebaas](sql-database-copy.md) | Azure'i SQL-andmebaasi koopia loomine |
| 133 | [Azure'i SQL-andmebaasiga BACPAC faili Azure'i portaalis arhiivimine](sql-database-export.md) | Azure'i SQL-andmebaasiga BACPAC faili Azure'i portaalis arhiivimine |



## <a name="security"></a>Turvalisus

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 134 | [Ühenduse loomine SQL-andmebaasi või SQL-i andmebaas Azure Active Directory autentimist kasutades](sql-database-aad-authentication.md) | Saate teada, kuidas SQL-andmebaasiga ühenduse loomine Windows Azure Active Directory autentimist kasutades. |
| 135 | [Alati krüptitud: Kaitsta tundlikku andmeid SQL-andmebaasis ja salvestada oma krüptimise võtmed serdi Windowsi poest](sql-database-always-encrypted.md) | Kaitsta tundlikku andmete SQL-andmebaasis minutites. |
| 136 | [Alati krüptitud: Kaitsta tundlikku andmeid SQL-andmebaasis ja salvestaks muutmiseks Azure'i klahvi Vault](sql-database-always-encrypted-azure-key-vault.md) | Kaitsta tundlikku andmete SQL-andmebaasis minutites. |
| 137 | [SQL-andmebaasi - toega alltaseme kliendid ja auditeerimine muudab IP lõpp-punkti](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Teave SQL-andmebaasi alltaseme klientide tugi ja IP-lõpp-punkti muudatuste auditeerimine. |
| 138 | [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid konfigureerimine PowerShelli abil](sql-database-configure-firewall-settings-powershell.md) | Saate teada, kuidas konfigureerida tulemüüri SQL Azure'i andmebaasid IP-aadressid. |
| 139 | [Azure'i SQL-andmebaasi server taseme tulemüüri reeglite abil REST API konfigureerimine](sql-database-configure-firewall-settings-rest.md) | Saate teada, kuidas konfigureerida tulemüüri SQL Azure'i andmebaasid IP-aadressid. |
| 140 | [Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reeglite abil T-SQL-i konfigureerimine](sql-database-configure-firewall-settings-tsql.md) | Saate teada, kuidas konfigureerida tulemüüri SQL Azure'i andmebaasid IP-aadressid. |
| 141 | [Alustamine SQL-i andmebaasi dünaamiliste andmete Masking (Azure'i portaal)](sql-database-dynamic-data-masking-get-started.md) | Kuidas alustada SQL-i andmebaasi dünaamiliste andmete Masking Azure'i portaalis |
| 142 | [Alustamine SQL-i andmebaasi dünaamiliste andmete Masking (Azure'i klassikaline portaal)](sql-database-dynamic-data-masking-get-started-portal.md) | Kuidas alustada SQL-i andmebaasi dünaamiliste andmete Masking Azure'i klassikaline portaalis |
| 143 | [Konfigureerige Azure'i SQL-andmebaasi tulemüüri reeglid \- ülevaade](sql-database-firewall-configure.md) | Siit saate teada, kuidas konfigureerida tulemüüri SQL-i andmebaasi server ja andmebaas-taseme tulemüüri juurdepääsu haldamine reeglite abil. |
| 144 | [SQL-andmebaasi õpetus: SQL-andmebaasi kasutajakontode juurdepääsuks ja haldamiseks andmebaasi loomine](sql-database-get-started-security.md) | Saate teada, kuidas juurdepääsu ja hallata andmebaasi kasutajakontode loomine. |
| 145 | [SQL-i andmebaasi autentimise ja luba: juurdepääsu andmine](sql-database-manage-logins.md) | SQL-i andmebaasi turvalisuse haldamine, Lugege täpsemalt, kuidas hallata andmebaasi serveritaseme põhisumma konto kaudu juurdepääsu ja login turvalisus. |
| 146 | [Azure'i SQL-andmebaasi turvalisuse juhised ja piirangud](sql-database-security-guidelines.md) | Lisateavet Microsoft Azure'i SQL-andmebaasi juhised ja seotud piirangud. |
| 147 | [SQL-i andmebaasi ohtude tuvastamise kasutamise alustamine](sql-database-threat-detection-get-started.md) | Kuidas alustada SQL-i andmebaasi ohtude tuvastamise Azure'i portaalis |
| 148 | [Kuidas teha levinud haldustoiminguid, näiteks Azure'i SQL-andmebaasi administraatori parooli lähtestamine](sql-database-troubleshoot-permissions.md) | Kirjeldab, kuidas SQL-andmebaasis levinud haldustoimingute sooritamiseks. Näiteks administraatori parooli lähtestamine, mis ja juurdepääsu keelamine. |



## <a name="manage-your-database"></a>Andmebaasi haldamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 149 | [SQL-andmebaasi auditeerimine alustamine](sql-database-auditing-get-started.md) | SQL-andmebaasi auditeerimine alustamine |
| 150 | [Azure'i SQL-andmebaasi server taseme tulemüüri reegli Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md) | Saate teada, kuidas konfigureerida tulemüüri juurdepääsu Azure SQL serveri IP-aadressid. |
| 151 | [Azure'i portaalis Azure SQL-andmebaasi kopeerimine](sql-database-copy-portal.md) | Azure'i SQL-andmebaasi koopia loomine |
| 152 | [Azure'i SQL-i andmebaasid abil Azure automatiseerimine haldamine](sql-database-manage-automation.md) | Siit saate teada, kuidas saate kasutada Azure automatiseerimine teenuse tasandil Azure SQL-i andmebaaside haldamine. |
| 153 | [Ülevaade: Haldusriistad SQL-i andmebaasi](sql-database-manage-overview.md) | Võrdleb tööriistad ja haldamise Azure'i SQL-andmebaasi suvandid |
| 154 | [Azure'i SQL-andmebaasi dünaamilise halduse vaadete abil jälgimine](sql-database-monitoring-with-dmvs.md) | Saate teada, kuidas tuvastada ja diagnoosimine levinud jõudlusprobleemide dünaamilise halduse vaadete abil saate jälgida Microsoft Azure'i SQL-andmebaasi. |
| 155 | [SQL-andmebaasi turvamine](sql-database-security.md) | Turvalisusest Azure SQL-andmebaas ja SQL serveri, sh pilveteenuses erinevused ja SQL serveri kohapealse autentimine, autoriseerimine, ühenduse turvalisus, krüptimise ja nõuetele vastavus kärpida. |



## <a name="extended-events"></a>Laiendatud sündmused

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 156 | [Sündmuse faili target kood SQL-andmebaasis laiendatud sündmuste jaoks.](sql-database-xevent-code-event-file.md) | Pakub PowerShelli ja Transact-SQL-i kahefaasiline koodi näide, mis näitab sündmuse faili target laiendatud juhul Azure SQL andmebaasis. Azure'i salvestusruumi on nõutav osa stsenaarium. |
| 157 | [Helisemine puhvri target kood SQL-andmebaasis laiendatud sündmuste jaoks.](sql-database-xevent-code-ring-buffer.md) | Pakub Transact-SQL-i kood näide, mis on tehtud kiire ja lihtne kasutada Ring puhvri eesmärki, Azure SQL-andmebaasis. |
| 158 | [Laiendatud sündmused SQL-andmebaasis](sql-database-xevent-db-diff-from-svr.md) | Kirjeldatakse laiendatud Azure'i SQL-andmebaasi sündmuste (XEvents), ja kuidas sündmuse seansid veidi erineda sündmuse seanssi Microsoft SQL Server. |



## <a name="geo-replication"></a>Geo dispersioonanalüüs

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 159 | [Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Azure'i portaalis](sql-database-geo-replication-failover-portal.md) | Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Azure'i portaalis |
| 160 | [Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi PowerShelli abil](sql-database-geo-replication-failover-powershell.md) | Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi PowerShelli abil |
| 161 | [Kontaktiga kavandatud või planeerimata Tõrkesiirde Transact-SQL Azure'i SQL-andmebaasi jaoks](sql-database-geo-replication-failover-transact-sql.md) | Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Transact-SQL-i abil |
| 162 | [Ülevaade: SQL-i andmebaasi aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md) | Aktiivse Geo-kopeerimine võimaldab teil andmebaasi mis tahes Azure'i andmekeskuste 4 koopiad häälestamine. |
| 163 | [Azure'i portaalis Azure'i SQL-andmebaasi Geo-Dispersioonanalüüs konfigureerimine](sql-database-geo-replication-portal.md) | Azure'i SQL-andmebaasi Azure'i portaalis Geo-Dispersioonanalüüs konfigureerimine |
| 164 | [Azure'i SQL-andmebaasi PowerShelliga Geo-Dispersioonanalüüs konfigureerimine](sql-database-geo-replication-powershell.md) | Azure'i SQL-andmebaasi PowerShelli kaudu aktiivse Geo-kopeerimine konfigureerimine |
| 165 | [Kuidas hallata Azure'i SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md) | See teema selgitab turvakaalutlused haldamise pärast andmebaasi taastamine või Tõrkesiirde turvalisus. |
| 166 | [Geo-Dispersioonanalüüs konfigureerimine Transact-SQL Azure'i SQL-andmebaas](sql-database-geo-replication-transact-sql.md) | Geo-kopeerimise abil Transact-SQL Azure'i SQL-andmebaasi konfigureerimine |
| 167 | [Geo-taastamine Azure'i SQL-andmebaasi Azure'i portaalis geograafilise liigne varukoopia põhjal](sql-database-geo-restore-portal.md) | Geo taastamine Azure'i SQL-andmebaasi varukoopia geograafilise liigne (Azure'i portaal). |
| 168 | [Azure'i SQL-andmebaasi taastamine varukoopia geograafilise liigne PowerShelli abil](sql-database-geo-restore-powershell.md) | Azure'i SQL-andmebaasi uus server geograafilise liigne varukoopia põhjal taastamine |



## <a name="upgrade"></a>Versiooni täiendamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 169 | [Täiustatud päringu jõudluse ühilduvus tase 130 Azure SQL-andmebaasis](sql-database-compatibility-level-query-performance-130.md) | Juhised ja tööriistu, kindlaks teha, millised ühilduvus on parim Azure'i SQL-andmebaasi või Microsoft SQL serveri andmebaasi |
| 170 | [Pilveteenuse rakenduste abil SQL-i andmebaasi aktiivse Geo kopeerimine jooksva uuendamine haldamine](sql-database-manage-application-rolling-upgrade.md) | Saate teada, kuidas kasutada Azure'i SQL-andmebaasi geo-dispersioonanalüüs toeta online uuendamine rakenduse pilve. |
| 171 | [Azure'i portaalis Azure SQL-i andmebaasi V12 versioonile](sql-database-upgrade-server-portal.md) | Selgitab, kuidas võtta kasutusele Azure SQL-i andmebaasi V12 sh kuidas veebi ja andmebaaside versiooni uuendamine ja kuidas uuendada oma andmebaase otse Azure'i portaalis kohapeal on elastne andmebaasi migreerimine V11 server. |
| 172 | [Versioonile Azure SQL-i andmebaasi V12 PowerShelli abil](sql-database-upgrade-server-powershell.md) | Selgitab, kuidas võtta kasutusele Azure SQL-i andmebaasi V12 sh kuidas veebi ja andmebaaside versiooni uuendamine ja kuidas uuendada oma andmebaase otse PowerShelli kaudu kohapeal on elastne andmebaasi migreerimine V11 server. |
| 173 | [Plaanimine ja võtta kasutusele SQL-i andmebaasi V12 ettevalmistamine](sql-database-v12-plan-prepare-upgrade.md) | Kirjeldatakse ettevalmistused ja versioonilt Azure SQL-i andmebaasi V12 seotud piirangud. |
| 174 | [Mis on uut rakenduses SQL-i andmebaasi V12](sql-database-v12-whats-new.md) | Kirjeldatakse, miks on kasulik business süsteemide, mis kasutavad Azure'i SQL-andmebaasi pilves, Täiendamine versiooniks V12 kohe. |
| 175 | [Web ja Business Edition ajalise piirangu KKK](sql-database-web-business-sunset-faq.md) | Siit saate teada kui Azure SQL-i veebi ja andmebaaside pensionile ning te saate tutvuda funktsioonid ja funktsionaalsus uus teenus astme. |



## <a name="tools"></a>Tööriistad

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 176 | [Azure'i SQL-andmebaasi PowerShelliga haldamine](sql-database-command-line-tools.md) | Azure SQL-i andmebaasi haldamine PowerShelli abil. |
| 177 | [SQL-andmebaasi õpetus: Azure'i SQL-andmebaasiga ühenduse Exceli ja aruande koostamine](sql-database-connect-excel.md) | Saate teada, kuidas Microsoft Excel pilveteenuses Azure SQL-i andmebaasiga ühenduse loomiseks. Andmete importimine rakendusse Excel aruandlus ja andmete uurimine. |
| 178 | [SQL-i andmebaasi loomine ja teha levinud andmebaasi häälestustoimingute PowerShelli cmdlet-käsud](sql-database-get-started-powershell.md) | Siit saate teada, nüüd PowerShelliga SQL-andmebaasi loomiseks. Levinud andmebaasi häälestustoimingute saab hallata PowerShelli cmdlet-käsud. |
| 179 | [Alustamine: Azure SQL-i andmete sünkroonimine (eelvaade)](sql-database-get-started-sql-data-sync.md) | Selles õpetuses aitab teil alustada Azure SQL-i andmete sünkroonimine (eelvaade). |
| 180 | [Azure'i SQL-andmebaasi SQL Server Management Studio abil haldamine](sql-database-manage-azure-ssms.md) | Saate teada, kuidas kasutada SQL Server Management Studio SQL-andmebaasi serverid ja andmebaaside haldamine. |
| 181 | [Azure'i portaalis Azure SQL-i andmebaaside haldamine](sql-database-manage-portal.md) | Saate teada, kuidas kasutada Azure portaali haldamiseks relatsiooniandmebaasist Azure'i portaalis pilveteenuses. |
| 182 | [Teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) SQL-andmebaasi PowerShelli abil](sql-database-scale-up-powershell.md) | Muuta teenuse kiht ja jõudluse taseme Azure SQL-andmebaasi näitab, kuidas mastaapimiseks PowerShelliga SQL-andmebaasi üles või alla. SQL Azure'i andmebaasi PowerShelliga hinnakirjad taseme muutmine. |
| 183 | [SQL Server Management Studio kasutamine Azure RemoteApp SQL-i andmebaasiga ühenduse loomiseks](sql-database-ssms-remoteapp.md) | Selles õppetükis saate teada, kuidas kasutada SQL Server Management Studio Azure RemoteApp turvalisuse ja jõudluse SQL-andmebaasiga ühenduse loomisel abil |



## <a name="reference"></a>Viide

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 184 | [Andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md) | Iga draiveri, mille Klientprogrammide abil saate ühendada Azure'i SQL-andmebaasi või Microsoft SQL Server vähim versiooninumber on loetletud. Lisateavet versiooni draiverid, mis on välja antud ühenduse, mitte Microsoft on toodud link. |
| 185 | [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md) | Sellel lehel kirjeldatakse mõnda levinud ressursi piirangud Azure SQL-i andmebaasi. |
| 186 | [Azure SQL-i andmebaasi Transact-SQL-i erinevused](sql-database-transact-sql-information.md) | Transact-SQL-laused, mis on väiksem kui täielikult toetatud Azure SQL-andmebaasis |



## <a name="miscellaneous"></a>Mitmesugused

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 187 | [Kopeerige PowerShelli kaudu Azure SQL-i andmebaas](sql-database-copy-powershell.md) | Eksemplari PowerShelli kaudu Azure SQL-andmebaasi loomine |
| 188 | [Kopeerige Transact-SQL-i abil SQL Azure'i andmebaas](sql-database-copy-transact-sql.md) | Eksemplari abil Transact-SQL Azure'i SQL-andmebaasi loomine |
| 189 | [Juhised ja SQL Azure'i andmebaasi üldine piirangud](sql-database-general-limitations.md) | Sellel lehel kirjeldatakse üldised piirangud Azure'i SQL-andmebaasi, samuti interoperability ja tugi. |
| 190 | [SQL-andmebaasi hinnakirjad taseme soovitused](sql-database-service-tier-advisor.md) | Muutmisel hinnad astme hinnad taseme soovitused Azure'i portaalis on soovitada taseme, mis on parim sobib töötab olemasoleva Azure SQL-i andmebaasi tema töökoormus. Hinnakirjad astme kirjeldada SQL-andmebaasi teenuse taseme- ja jõudluse tase. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Lisad

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Seotud artiklid muude Azure'i teenuste

- [Rakenduse Azure rakenduse teenuste Azure varundamine](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentatsiooni tööriistad

- [Värskenduste Azure SQL-i andmebaasi teatamine](http://azure.microsoft.com/updates/?service=sql-database)

- [Otsige kõik dokumendid tüüpi Microsoft Azure'i, filtrid](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Tee pilte õppimise

- [Õppekeskuse tee graafika kõigi Microsoft Azure'i teenuste](http://azure.microsoft.com/documentation/learning-paths/)

- [Õppeteema: Lugege Azure'i SQL-andmebaas](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Õppeteema: SQL-andmebaasi elastne skaala](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


