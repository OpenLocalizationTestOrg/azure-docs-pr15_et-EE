<properties
    pageTitle="Aktiivse Geo-kopeerimine Azure'i SQL-andmebaas"
    description="Aktiivse Geo-kopeerimine võimaldab teil andmebaasi mis tahes Azure'i andmekeskuste 4 koopiad häälestamine."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Ülevaade: SQL-i andmebaasi aktiivse Geo-kopeerimine

Aktiivse Geo-kopeerimine abil saate konfigureerida kuni nelja loetavad teisene andmebaaside samas või mõnes muus andmete keskmist kohtades (piirkonnad). Teisene andmebaaside on saadaval päringute ja andmete keskmist katkestuste või esmane andmebaasiga ühenduse loomiseks ei suuda Tõrkesiirde.

>[AZURE.NOTE] Aktiivse Geo kopeerimine (loetavad sekundaaride) on nüüd saadaval kogu teenuse astme kõik andmebaasid. Aprill 2017, pensionile-loetav teisese tüüp ja -loetav olemasolevaid andmebaase uuendatakse automaatselt loetav sekundaaride.

 Aktiivse Geo kopeerimine [PowerShelli](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md) [Azure'i portaal](sql-database-geo-replication-portal.md)abil saate konfigureerida või [REST API - loomine või värskendamine andmebaasi](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Konfigureerida: Azure'i portaalis](sql-database-geo-replication-portal.md)
- [Konfigureerimine: PowerShelli](sql-database-geo-replication-powershell.md)
- [Konfigureerige: T-SQL-is](sql-database-geo-replication-transact-sql.md)

Kui mis tahes põhjusel teie peamine andmebaasi nurjub, või lihtsalt peab võrguühenduseta, saate *Tõrkesiirde* ühtegi teisene andmebaasi. Tõrkesiirde aktiveerimisel ühele teisene andmebaaside kõik muud sekundaaride lingitakse automaatselt uue esmase.

Saate [Azure portaali](sql-database-geo-replication-failover-portal.md), [PowerShelli](sql-database-geo-replication-failover-powershell.md), [Transact-SQL-i](sql-database-geo-replication-failover-transact-sql.md), [REST API - plaanitud Tõrkesiirde](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)või [REST API - planeerimata Tõrkesiirde](https://msdn.microsoft.com/library/azure/mt582027.aspx)teisese Tõrkesiirde.


> [AZURE.SELECTOR]
- [Tõrkesiirde: Azure'i portaal](sql-database-geo-replication-failover-portal.md)
- [Tõrkesiirde: PowerShelli](sql-database-geo-replication-failover-powershell.md)
- [Tõrkesiirde: T-SQL-is](sql-database-geo-replication-failover-transact-sql.md)

Pärast Tõrkesiirde, veenduge, et autentimise nõuded oma server ja andmebaas on konfigureeritud uue esmase. Lisateavet leiate teemast [SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md).


Aktiivse Geo-kopeerimine funktsioon rakendab süsteem esitada andmebaasi koondamise samas Microsoft Azure'i piirkonnas või eri piirkondadest (geo koondamine). Aktiivse Geo-kopeerimine tiražeerib asünkroonselt sooritatud toimingud andmebaasist kuni nelja koopiad erinevates serverites, kasutades andmebaasi lugeda kinnitatud hetktõmmise eraldamise (RCSI) jaoks eraldi. Aktiivse Geo-kopeerimine on konfigureeritud, luuakse teise andmebaasi määratud serveris. Algse andmebaasi muutub esmane andmebaas. Esmane andmebaas tiražeerib sooritatud toimingud asünkroonselt iga teisene andmebaaside. Ainult täielik tehingud ei kopeeritud. Ajal mis tahes hetkel antud, teise andmebaasi võib olla veidi taha esmane andmebaas, sekundaarne on tagatud pole kunagi osaline tehingud. Kindlate andmete RPO leiate [Järjepidevuse ülevaade](sql-database-business-continuity.md).

Aktiivse Geo-kopeerimine esmane eeliseid on, et pakub andmebaasi taseme katastroofi taastamise lahendus madal taastamise ajal. Kui asetate teises regioonis serveris teise andmebaasi, saate lisada kuni paindlikkuse rakenduse. Rist-piirkond koondamise võimaldab rakendusi jäädavalt kadumist on kogu andmekeskuse või osade andmekeskuses loodusõnnetuste, katastroofiline inimeste vigu või pahatahtlik toimingud taastada. Järgmisel joonisel on näide aktiivse Geo-kopeerimine konfigureeritud primaarne Põhja keskse meil regioonis ja teiseseid piirkonna Lõuna keskse US.

![Seose Geo-Dispersioonanalüüs](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Mõne muu põhieelis on see, et teisene andmebaaside on loetav ja et kirjutuskaitstud teenustest, nt aruandlusteenuste töö saab kasutada. Kui kavatsete kasutada teise andmebaasi koormust tasakaalustavad, saate selle piirkonna esmase luua. Piirkonna teisese loomist suurendada rakenduse paindlikkuse katastroofiline ebaõnnestumist.  

Teiste stsenaariumide, kus saab kasutada aktiivse Geo-kopeerimine on järgmised.

- **Andmebaasi migreerimine**: abil saate aktiivse Geo-kopeerimine teise võrgus minimaalne tööseisakute serverist ühe andmebaasi migreerimine.
- **Rakenduse uuendused**: saate luua ka täiendavat teisese fail tagasi koopiana rakenduse uuendamine.

Reaal järjepidevuse saavutamiseks lisamine andmebaasi koondamise andmekeskuste vahel on ainult osa lahendus. Taastamisel on rakenduse (teenus)--lõpuni pärast katastroofiline tõrge nõuab kõik komponendid, mis moodustavad teenuse ja mis tahes sõltuvad teenused. Nende komponentide näiteks kliendi tarkvara (nt brauseri kohandatud JavaScript), veebi, salvestusruumi ja DNS-i. See on oluline, et kõik komponendid on, et sama tõrkeid ja muutuvad kättesaadavaks taastamise aja eesmärk (RTO) rakenduse sees. Seega peate tuvastada järgnevaid kõigi teenuste ja annavad garantiid ja mõista. Seejärel tuleb teha tagamaks, et teie teenuse funktsioone, mida ta sõltub teenuste Tõrkesiirde ajal piisavalt juhiseid. Tõrkejärgseks kujundamisel lahenduste kohta leiate lisateavet teemast [Kujundamise pilve lahenduste jaoks katastroof taastamise abil aktiivse Geo-kopeerimine](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktiivse Geo-Dispersioonanalüüs võimalused
Aktiivse Geo-kopeerimine funktsioon pakub järgmisi olulisi funktsioone:

- **Asünkroonne automaatne kopeerimine**: saate luua ainult teise andmebaasi, lisades olemasoleva andmebaasiga. Teisese saab luua ainult eri Azure'i SQL-andmebaasi server. Pärast loomist teise andmebaasi lisatakse primaarse andmebaasist kopeeritud andmeid. Selle protsessi tuntakse külv. Pärast teisene andmebaas on loodud ja seemnetega, värskendused esmane andmebaas on asünkroonselt kopeeritud teise andmebaasi automaatselt. Asünkroonne dispersioonanalüüs tähendab, et tehingud kinnitatud esmane andmebaasi enne, kui need on kopeeritud teise andmebaasi. 

- **Mitme teisene andmebaaside**: kahe või enama teisene andmebaaside koondamise ja kaitsetase esmane andmebaas ja rakenduse suurendamiseks. Mitme teisene andmebaaside olemasolul rakenduse jääb kaitstud isegi juhul, kui üks teisene andmebaaside nurjub. Kui on ainult üks teisene andmebaas ja see nurjub, rakendus on avatud suurem seni, kuni on loodud uue teisene andmebaasi.

- **Loetavad teisene andmebaaside**: rakenduse pääsevad teise andmebaasi kirjutuskaitstud toimingute abil samas või mõnes muus turvalisus põhisumma, esmane andmebaas juurdepääsuks kasutada. Teisene andmebaaside tegutseda hetktõmmise eraldamise režiimis, et veenduda, et dispersioonanalüüs värskenduste esmase (log Kordus) pole viibib päringud teisese täide.

>[AZURE.NOTE] Kui skeemi värskendusi, et see peamine saab teise andmebaasi skeemi lock nõudvate on viibib log kordus teise andmebaasi.

- **Aktiivse geo kopeerimine elastne rakenduskausta andmebaaside**: aktiivse Geo-kopeerimine saab konfigureerida iga andmebaasi elastne andmebaasi igal pool. Teise andmebaasi saab teise elastne andmebaasi pargi. Tavaline andmebaase, saab teisese kui astme teenus on sama elastne andmebaasi rakenduskausta ja vastupidi. 

- **Teisene andmebaasi konfigureeritav jõudlusega**: jõudluse väiksem kui esmast saab luua teise andmebaasi. Nii põhi- ja andmebaaside peavad olema sama teenuse kiht. See suvand soovitatav rakenduste abil kõrge andmebaasi kirjutamine tegevus kuna suurema dispersioonanalüüs lag suurendab olulisi andmete kaotsimineku pärast Tõrkesiirde. Lisaks pärast Tõrkesiirde rakenduse jõudlus mõjutab seni, kuni uus primaarne täiendamist jõudluse kõrgemale tasemele. Log IO protsent diagrammi Azure'i portaalis pakub hea võimalus säilitada dispersioonanalüüs laadi nõutava teisese minimaalsete jõudluse taseme. Näiteks kui teie peamine andmebaas on P6 (1000 DTU) ja selle log IO protsent on 50% teisese peab olema vähemalt P4 (500 DTU). Saate kuulata ka IO logiandmed [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) või [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) andmebaasi vaadete kasutamine.  SQL-andmebaasi jõudluse kohta lisateabe saamiseks vt [SQL-andmebaasi suvandid ning jõudlust](sql-database-service-tiers.md). 

- **Kasutaja kontrollitud Tõrkesiirde ja failback**: teise andmebaasi saab konkreetselt sisse peamine roll igal ajal rakenduse või kasutaja. Reaal katkestuste ajal suvandi "planeerimata" tuleks kasutada, mis kohe soodustab teisese olema esmane. Kui nurjunud primaarne taastab ja on saadaval uuesti, süsteem automaatselt märgib taastatud esmase, teisese ja viia see uue esmase ajakohasena. Dispersioonanalüüs asünkroonne laadist väike andmete saate kaotsi ajal planeerimata failovers esmane nurjumisel enne selle tiražeerib teisese Viimati tehtud muudatused. Kui esmane koos mitme sekundaaride ei üle, süsteemi automaatselt ümber konfigureeritud dispersioonanalüüs seosed ja linke ülejäänud sekundaaride äsja esiletõstetud primaarne ilma kasutaja sekkumiseta. Pärast selle Tõrkesiirde põhjustanud katkestuste leevendada, võib olla soovitav rakenduse esmane alale tagastamiseks. Selleks, et tuleks kasutada Tõrkesiirde käsu "kavandatud" suvandiga. 

- **Mandaadi ja tulemüüri reeglid sünkroonis**: soovitame kasutatav [andmebaas tulemüüri reeglid](sql-database-firewall-configure.md) geo kopeeritud andmebaasid nii reegleid saab paljundada kõigi teisene andmebaaside pääseks tulemüüri samu reegleid esmase andmebaasi. Seda moodust välistab kliendid käsitsi konfigureerimine ja tulemüüri reeglid serverites majutuse nii põhi- ja andmebaaside haldamine. Samuti abil [sisalduvad andmebaasi kasutajate](sql-database-manage-logins.md) juurdepääs andmetele tagab nii põhi- ja andmebaasid on alati sama kasutaja mandaat nii ajal Tõrkesiirde, on pole katkestuste tõttu vastuolude sisselogimise ja paroolide abil. Lisaks [Azure Active Directory](../active-directory/active-directory-whatis.md), kliendid saate hallata kasutajate juurdepääsu nii põhi- ja andmebaaside ja kõrvaldada haldamise andmebaasid kokku mandaati.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Üleminekut või kasutuselevõttu esmane andmebaas
Saate uuendada või alandada mis tahes teisene andmebaaside logimata esmase andmebaasi erinevate jõudluse tasemele (jooksul sama teenuse kiht). Täiendamisel, soovitame teise andmebaasi täiendamine esmalt ja seejärel täiendamine esmane. Kui alandamine, liikumisjärjestuse: alandada esmast esmalt ja seejärel alandada teisese. 

Teise andmebaasi peab olema sama teenuse kiht esmane, et esmane andmebaasi migreerimine muu teenuse kiht nõuab teilt geo-dispersioonanalüüs link lõpetada ja teise andmebaasi ümber nimetada või lihtsalt kukutage see. Seejärel migreerimine esmast uue teenuse kiht ja konfigureerima geo-dispersioonanalüüs. Teie uus teisese luuakse automaatselt jõudluse samal tasemel esmase vaikimisi.

## <a name="preventing-the-loss-of-critical-data"></a>Kriitiliste andmete kaotsimineku vältimine
Võrkude latentsusajaga, tõttu kasutab pidev Kopeeri on asünkroonne dispersioonanalüüs süsteem. Asünkroonne dispersioonanalüüs muudab mõned andmekao vältimatu, kui ilmneb tõrge ilmneb. Mõni rakendus võib siiski nõuda pole andmete kaotsimineku. Kriitilised värskendused kaitsmiseks rakendus arendaja saate helistada [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) süsteemi protseduuri kohe pärast toime tehingu. Helistamiseks **sp_wait_for_database_copy_sync** plokid helistaja jutulõnga kuni viimase toime tehingu on kopeeritud teise andmebaasi. Protseduur on oodake, kuni kõik ootel tehingud on kinnitanud teise andmebaasi. **sp_wait_for_database_copy_sync** on rakendatud teatud pidev Kopeeri link. Esmane andmebaasiga ühenduse õigustega kasutaja saate helistada seda toimingut.

>[AZURE.NOTE] Põhjustatud **sp_wait_for_database_copy_sync** protseduuri kõne viivitus võib olla suur. Viivitus sõltub tehingu pikkust suurust praegu ja selle kõne ei tagasta seni, kuni kogu log on kopeeritud. Vältige helistamiseks seda toimingut, kui tingimata vajalik.

## <a name="programmatically-managing-active-geo-replication"></a>Aktiivse Geo-kopeerimine programmiliselt haldamine

Nagu varem, aktiivse Geo-kopeerimine saab hallata ka programmiliselt Azure PowerShelli ja REST API abil. Järgmises tabelis on kirjeldatud käskude saadaval.

- **Azure'i ressursihaldur API ja Rollipõhine Turve**: aktiivse Geo-kopeerimine sisaldab kogumi [Azure'i ressursihaldur API -de]( https://msdn.microsoft.com/library/azure/mt163571.aspx) haldamine, sh [Azure ressursihaldur vastavalt PowerShelli cmdlet-käsud](sql-database-geo-replication-powershell.md). Nende API-de nõuavad ressursi rühmad ja tugi Rollipõhine Turve (RBAC). Kuidas rakendada Accessi rollid leiate [Azure Role-Based juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Palju uusi funktsioone aktiivse Geo-kopeerimine toetatakse ainult abil [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) vastavalt [Azure SQL-i REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) ja [Azure SQL-i andmebaasi PowerShelli cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt574084.aspx). (Klassikaline) REST API] (https://msdn.microsoft.com/library/azure/dn505719.aspx) ja [Azure'i SQL-andmebaasi (klassikaline) cmdlet-käsud](https://msdn.microsoft.com/library/azure/dn546723.aspx) on toetatud tagasiühilduvuse jaoks, on soovitatav kasutada Azure ressursihaldur vastavalt API-d. 


### <a name="transact-sql"></a>Transact-SQL-is

|Käsk|Kirjeldus|
|-------|-----------|
|[Muuda andmebaasi (SQL Azure'i andmebaas)](https://msdn.microsoft.com/library/mt574871.aspx)|TEISENE serveri lisamine argumendi abil saate luua teise andmebaasi mõne olemasoleva andmebaasiga ja alustab andmete kopeerimine|
|[Muuda andmebaasi (SQL Azure'i andmebaas)](https://msdn.microsoft.com/library/mt574871.aspx)|Aktiveerige olema esmane Tõrkesiirde algatada teisene andmebaasi TÕRKESIIRDE või FORCE_FAILOVER_ALLOW_DATA_LOSS abil
|[Muuda andmebaasi (SQL Azure'i andmebaas)](https://msdn.microsoft.com/library/mt574871.aspx)|TEISENE serveri eemaldamine abil andmete kopeerimine SQL-andmebaasi ja määratud teisene andmebaasi lõpetada.|
|[sys.geo_replication_links (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/mt575501.aspx)|Tagastab loogilise Azure'i SQL-andmebaasi server kõik olemasolevad dispersioonanalüüs lingid iga andmebaasi teave.|
|[sys.dm_geo_replication_link_status (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/mt575504.aspx)|SQL-i andmebaasi saab dispersioonanalüüs viimast, viimase dispersioonanalüüs lag ja muu teabe kopeerimine link.|
|[sys.dm_operation_status (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/dn270022.aspx)|Kuvatakse kõik andmebaasi toimingud, sh dispersioonanalüüs linkide oleku olek.|
|[sp_wait_for_database_copy_sync (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/dn467644.aspx)|põhjustab rakenduse ootama, kuni kõik sooritatud toimingud on kopeeritud ja aktiivne teise andmebaasi kinnitama.|
||||

### <a name="powershell"></a>PowerShelli

|Cmdlet-käsk|Kirjeldus|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Saab üks või mitu andmebaasid.|
|[Uue AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Olemasoleva andmebaasi teisene andmebaasi loob ja käivitab andmete kopeerimine.|
|[Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Aktiveerib teise andmebaasi olema esmane Tõrkesiirde algatada.|
|[Eemalda – AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Andmete kopeerimine SQL-andmebaasi ja määratud teisene andmebaasi lõpeb.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Saab geo-dispersioonanalüüs linkide Azure'i SQL-andmebaasi ja ressursirühma või SQL serveri vahel.|
||||

### <a name="rest-api"></a>REST API-GA

|API-GA|Kirjeldus|
|---|-----------|
|[Looge või Update andmebaasi (createMode = taastamine)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Loob, värskendab või taastab esmane või teisene andmebaasi.|
|[Saada loomine või andmebaasi oleku värskendamine](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Tagastab olek loomine töötamise ajal.|
|[Määrake teise andmebaasi esmane (kavandatud Tõrkesiirde)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Saab reklaamida teise andmebaasi Geo-Dispersioonanalüüs koostöös saada esmane uue andmebaasi.|
|[Määrake teise andmebaasi esmane (planeerimata Tõrkesiirde)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Teisene andmebaasi Tõrkesiirde jõustamine ja määrata teisese esmase.|
|[Dispersioonanalüüs linkide hankimine](https://msdn.microsoft.com/library/azure/mt600929.aspx)|SQL-i andmebaasi geo-dispersioonanalüüs koostöö saab kõik dispersioonanalüüs lingid. See toob sys.geo_replication_links kataloogi vaates nähtav teave.|
|[Saate hankida lingi kopeerimine](https://msdn.microsoft.com/library/azure/mt600778.aspx)|SQL-i andmebaasi geo-dispersioonanalüüs koostöö saab teatud dispersioonanalüüs link. See toob sys.geo_replication_links kataloogi vaates nähtav teave.|
||||



## <a name="next-steps"></a>Järgmised sammud

- Äri järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [äri järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md).
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md).
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md).
- Uue esmane server ja andmebaas autentimisnõuded kohta lisateabe saamiseks vt [SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md).
