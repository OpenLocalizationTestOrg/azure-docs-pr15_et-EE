<properties
   pageTitle="SQL serveri tulemüüri ülevaade konfigureerimine | Microsoft Azure'i"
   description="Siit saate teada, kuidas konfigureerida tulemüüri SQL-i andmebaasi server ja andmebaas-taseme tulemüüri juurdepääsu haldamine reeglite abil."
   keywords="andmebaasi tulemüüri"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Konfigureerige Azure'i SQL-andmebaasi tulemüüri reeglid \- ülevaade


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-firewall-configure.md)
- [Azure'i portaal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShelli](sql-database-configure-firewall-settings-powershell.md)
- [REST API-GA](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure'i SQL-andmebaasi leiate Azure'i ja teiste Interneti-põhiste rakenduste relatsiooniandmebaasist teenuse. Oma andmete kaitsmiseks tulemüürid takistavad oma andmebaasiserveriga seni, kuni saate määrata, millises arvutis on õigus. Tulemüüri annab juurdepääsu andmebaasidele, mis põhineb iga taotluse pärit IP-aadress.

Konfigureerida tulemüüri, saate luua tulemüüri reeglid, mis määravad lubatud IP-aadresside vahemikud. Saate luua tulemüüri reeglid server ja andmebaas.

- **Serveritaseme tulemüüri reeglid:** Reegleid klientidel juurdepääs kogu Azure SQL-i server, st kõik andmebaasid jooksul sama loogilise server. **Juhtslaidi** andmebaasi talletatakse järgmisi reegleid. Portaali kaudu või Transact-SQL-lausete abil saate konfigureerida serveritaseme tulemüüri reeglid.
- **Andmebaasi taseme tulemüüri reeglid:** Reegleid lubada üksikuid andmebaasidele oma Azure'i SQL-andmebaasi serveri juurde. Saate luua reeglid iga andmebaasi ja need on talletatud üksikud andmebaasid. (Saate luua andmebaasi taseme tulemüüri reeglid **juhtslaidi** andmebaasi.) Reegleid võib olla kasulik juurdepääsu piiramine teatud sama loogilise server (turvaline) andmebaasid. Andmebaasi taseme tulemüüri reegleid saab konfigureerida ainult Transact-SQL-lausete abil.

**Soovitus:** Microsoft soovitab andmebaasi taseme tulemüüri reeglite võimaluse turvalisuse täiustamiseks ja teha veel portable andmebaasi kasutamine. Kasutage serveritaseme tulemüüri reeglid administraatorid ja kui teil on palju andmebaasidele, mis on sama juurdepääs nõuded ja te ei soovi iga andmebaasi konfigureerimine eraldi aega.


## <a name="firewall-overview"></a>Tulemüüri ülevaade

Esialgu kõik Transact-SQL-i juurdepääsu oma Azure SQL-i server on blokeeritud tulemüüri. Azure SQL serveris kasutamise alustamiseks tuleb Azure'i portaalis ja määrata ühe või mitme serveritaseme tulemüüri reeglid, mis võimaldavad juurdepääsu Azure SQL serveris. Tulemüüri reeglite abil saate määrata, millised IP-aadresside vahemikud Interneti kaudu on lubatud ja kas Azure rakendusi saate proovida Azure SQL serveriga.

Valikuliselt anda juurdepääsu vaid üks teie Azure SQL serveri andmebaasi, peate looma andmebaasi taseme reegli nõutav andmebaasi. Määrake IP-aadresside vahemiku andmebaasi tulemüüri reegel, mis on väljaspool serveritaseme tulemüüri reeglis määratud IP-aadresside vahemiku jaoks, ja veenduge, et kliendi IP-aadress kuulub andmebaasi taseme reeglis määratud vahemikus.

Ühenduse katsete Internet ja Azure peate esmalt läbi tulemüüri enne, kui nad jõuavad teie Azure SQL serveri või SQL-andmebaasi, nagu on näidatud järgmisel joonisel.

   ![Diagrammi kirjeldavad tulemüüri konfigureerimine.][1]

## <a name="connecting-from-the-internet"></a>Ühenduse loomine Interneti kaudu

Kui arvuti proovib Interneti kaudu oma andmebaasi serveriga ühendust luua, tulemüüri kontrollib esmalt taotluse vastu tulemüüri reeglid täiskomplekti pärit IP-aadress.

- Kui taotluse IP-aadress on üks serveritaseme tulemüüri reeglid määratud vahemikud, antakse Azure SQL-i andmebaasiserveriga ühenduse.

- Kui IP-aadress taotluse pole ühe serveritaseme tulemüüri reeglis määratud vahemikud, kontrollitakse andmebaasi taseme tulemüüri reeglid. Kui taotluse IP-aadress on üks määratud andmebaasi taseme tulemüüri reeglid vahemikud, antakse ühenduse ainult andmebaasi, mis sisaldab vastavaid andmebaasi taseme reegli.

- Kui IP-aadress taotluse pole määratletud serveritaseme või andmebaasi taseme tulemüüri reeglite, ühenduse taotlus nurjub.

> [AZURE.NOTE] Kohalikus arvutis: Azure'i SQL-andmebaasi juurdepääsu tagada võrgu-ja kohaliku arvuti tulemüüri võimaldab väljamineva TCP port 1433.


## <a name="connecting-from-azure"></a>Azure'i ühendamine

Kui rakendus: Azure'i proovib andmebaasi serveriga, tulemüüri kinnitab Azure ühendused lubatud. Tulemüüri säte koos algus- ja aadress 0.0.0.0 võrdne näitab, et need ühendused on lubatud. Kui ühenduse loomise katse on keelatud, ei jõua taotluse Azure'i SQL-andmebaasi server.

> [AZURE.IMPORTANT] See suvand konfigureerib tulemüüri lubama kõik ühendused: Azure'i sh ühenduste teiste klientide tellimused. Selle suvandi valimisel veenduge, et teie kasutajanime ja kasutaja õigused piirata vaid autoriseeritud kasutajad.

Saate lubada Azure ühenduste kahel viisil:

- [Microsoft Azure'i portaalis](https://portal.azure.com/), märkige ruut **Luba juurdepääs server Azure'i teenuste** uus server loomisel.

- [Klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=161793)serveris jaotises **Lubatud teenuste** **konfigureerimine** menüü nuppu **Jah** **Microsoft Azure'i teenuste**jaoks.

## <a name="creating-the-first-server-level-firewall-rule"></a>Esimese taseme serveri tulemüüri reegli loomine

[Azure'i portaalis](https://portal.azure.com/) või programmiliselt REST API-ga või Azure PowerShelli abil saab luua esimese serveritaseme tulemüüri säte. Edaspidised serveritaseme tulemüüri reeglid saab luua ja hallata nende meetodite abil nagu Transact-SQL-i kaudu. Jõudluse parandamiseks vahemällu serveritaseme tulemüüri reeglid ajutiselt andmebaasi tasemel. Vahemälu värskendamine, lugege teemat [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Serveritaseme tulemüüri reeglite kohta leiate lisateavet teemast [kohta: Azure'i portaalis Azure SQL serveri tulemüüri konfigureerimine](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Andmebaasi taseme tulemüüri reeglite loomine

Kui olete konfigureerinud esimese serveritaseme tulemüüri, soovite piirata juurdepääsu teatud andmebaaside. Kui määrate IP-aadresside vahemiku andmebaasi taseme tulemüüri reegel, mis on väljaspool vahemikku serveritaseme tulemüüri reeglis määratud, on need kliendid, mis on IP-aadressid andmebaasi taseme vahemikus juurdepääs andmebaasile. Teil võib olla kuni 128 andmebaasi taseme tulemüüri reeglid andmebaasi jaoks. Andmebaasi taseme tulemüüri reeglid juhtslaidi ja kasutajale andmebaaside jaoks saab luua ja hallata Transact-SQL-i kaudu. Andmebaasi taseme tulemüüri reeglid konfigureerimise kohta leiate lisateavet teemast [sp_set_database_firewall_rule (Azure SQL-i andmebaasid)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programmiliselt tulemüüri reeglite haldamine

Azure'i portaali lisaks tulemüüri reeglid saab hallata programmiliselt Transact-SQL-i, REST API-ga ja Azure PowerShelli abil. Järgmises tabelis on kirjeldatud käskude iga meetodi jaoks saadaval.


### <a name="transact-sql"></a>Transact-SQL-is

| Kataloogi vaate või salvestatud protseduur                                                           | Tase     | Kirjeldus                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Server    | Kuvatakse praegune serveritaseme tulemüüri reeglid     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Server    | Loob või värskendab serveritaseme tulemüüri reeglid       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Server    | Eemaldab serveritaseme tulemüüri reeglid                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Andmebaasi  | Kuvab praeguse andmebaasi taseme tulemüüri reeglid   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Andmebaasi  | Loob või värskendab andmebaasi taseme tulemüüri reeglid |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Andmebaasid | Eemaldab andmebaasi taseme tulemüüri reeglid                |

### <a name="rest-api"></a>REST API-GA

| API-GA                  | Tase  | Kirjeldus                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Loendi tulemüüri reeglid](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Server | Kuvatakse praegune serveritaseme tulemüüri reeglid                 |
| [Tulemüüri reegli loomine](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Loob või värskendab serveritaseme tulemüüri reeglid                   |
| [Sea tulemüüri reegel](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Server | Olemasoleva serveritaseme tulemüüri reegli atribuutide värskendused |
| [Tulemüüri reegli kustutamine](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Eemaldab serveritaseme tulemüüri reeglid                              |


### <a name="azure-powershell"></a>Azure'i PowerShelli

| Cmdlet-käsk                                                                                              | Tase  | Kirjeldus                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Server | Tagastab praeguse serveritaseme tulemüüri reeglid                  |
| [Uue AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Server | Loob uue serveritaseme tulemüüri reegli                         |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Server | Olemasoleva serveritaseme tulemüüri reegli atribuutide värskendused |
| [Eemalda – AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Eemaldab serveritaseme tulemüüri reeglid                              |

> [AZURE.NOTE] Ei saa üles nii palju, kui muudatuste jõustumiseks tulemüürisätteid viivitamine viie minuti.

## <a name="troubleshooting-the-database-firewall"></a>Andmebaasi tulemüüri tõrkeotsing

Kui Microsoft Azure'i SQL-andmebaasi teenusele juurdepääsu käitumine, kui eeldate, võtke arvesse järgmist.

- **Kohaliku tulemüüri konfiguratsiooni:** Enne arvuti pääseb Azure'i SQL-andmebaasi, võib-olla peate looma tulemüüri erand TCP-port 1433 arvutis. Kui teete ühendused Azure pilveteenuse Rühmaboks sisse, peate avage täiendavate pordid. Lisateabe saamiseks vt selle **SQL-andmebaasi V12: väljaspool vs sees** [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md)osas.

- **Võrgu aadress tõlge (NAT):** Tõttu NAT, võib teie arvutis Azure SQL-i andmebaasiga ühenduse loomiseks kasutatava IP-aadressi erinevas kuvatakse teie arvuti IP konfigureerimissäteteks IP-aadress. Teie arvuti ühenduse Azure'i IP-aadressi vaatamiseks portaali sisse logida ja liikuge menüü **konfigureerimine** andmebaasi majutavas serveris. Jaotises **Lubatud IP-aadresside** kuvatakse **Praeguse kliendi IP-aadress** . Klõpsake käsku **Lisa** **Lubatud IP-aadresside** soovite lubada selle arvuti serverit.

- **Luba loendi muudatused on tehtud efekti veel:** Võib olla nii palju, kui muudatuste jõustumiseks Azure'i SQL-andmebaasi tulemüüri konfiguratsiooni viivitamine viie minuti.

- **Login ei ole lubatud või vale parooli kasutati:** Kui sisselogimine on õigused Azure'i SQL-andmebaasi server või kasutatud on vale, Azure SQL-i andmebaasiserveriga ühenduse on keelatud. Tulemüüri säte loomise ainult pakub klientidele võimalust üritavad ühenduse oma serveriga; iga kliendi peate sisestama vajalikud turvalisus. Ettevalmistamine sisselogimise kohta leiate lisateavet teemast andmebaaside haldamine, sisselogimise ja kasutajate Azure SQL-andmebaasis.

- **Dünaamiline IP-aadress:** Kui teil on Interneti-ühenduse dünaamiline IP tegelemine ja teil on probleeme tulemüüri kaudu, võib proovige ühte järgmistest lahendustest:

 - Küsige oma Interneti-teenuse pakkuja (ISP) määratud teie klientarvutid juurdepääs Azure'i SQL-andmebaasi serveri IP-aadresse ja seejärel lisage IP-aadresside vahemiku tulemüüri tavaliselt.

 - Hangi staatiline IP tegelemine hoopis oma klientarvutite ja seejärel lisada IP-aadresside tulemüüri reeglid.

## <a name="next-steps"></a>Järgmised sammud

Kuidas artikleid server ja andmebaas-taseme tulemüüri reegli loomise kohta, vaadake teemat:

- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md)
- [Azure'i SQL-andmebaasi server ja andmebaas-taseme tulemüüri reeglite abil T-SQL-i konfigureerimine](sql-database-configure-firewall-settings-tsql.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglid PowerShelli kaudu konfigureerimine](sql-database-configure-firewall-settings-powershell.md)
- [Azure'i SQL-andmebaasi server taseme tulemüüri reeglite abil REST API konfigureerimine](sql-database-configure-firewall-settings-rest.md)

Andmebaasi loomiseks, juhendi leiate [Azure'i portaalis minutites SQL-andmebaasi loomine](sql-database-get-started.md).
Azure'i SQL-andmebaasiga ühenduse loomiseks kaudu avatud lähte- või muude tootjate rakenduste korral lugege teemat [Kliendi kiire alustada koodinäiteid SQL-andmebaasiga](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Mõistmaks, kuidas liikuda andmebaasidele, lugege teemat [Accessi ja logige sisse andmebaasi turvalisuse haldamine](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Lisaressursid

- [Andmebaasi turvamine](sql-database-security.md)
- [Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
