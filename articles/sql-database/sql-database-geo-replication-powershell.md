<properties 
    pageTitle="Aktiivse Geo-kopeerimine konfigureerimine Azure'i SQL-andmebaasi PowerShelli kaudu | Microsoft Azure'i" 
    description="Azure'i SQL-andmebaasi PowerShelli kaudu aktiivse Geo-kopeerimine konfigureerimine" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Azure'i SQL-andmebaasi PowerShelliga Geo-Dispersioonanalüüs konfigureerimine

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-geo-replication-overview.md)
- [Azure'i portaal](sql-database-geo-replication-portal.md)
- [PowerShelli](sql-database-geo-replication-powershell.md)
- [T-SQL-IS](sql-database-geo-replication-transact-sql.md)

Selles artiklis kirjeldatakse, kuidas konfigureerida aktiivse Geo-kopeerimine SQL-i andmebaasi PowerShelliga.

Tõrkesiirde PowerShelli kaudu algatada teemast [Azure SQL-i andmebaasi PowerShelliga kavandatud või planeerimata Tõrkesiirde algatada](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Aktiivse Geo kopeerimine (loetavad sekundaaride) on nüüd saadaval kogu teenuse astme kõik andmebaasid. Aprillis 2017 pensionile-loetav teisese tüüp ja -loetav olemasolevaid andmebaase uuendatakse automaatselt loetav sekundaaride.



Aktiivse Geo-kopeerimine konfigureerimiseks PowerShelli kaudu teil vaja järgmist:

- Azure'i tellimuse. 
- Azure'i SQL-andmebaasi - esmane andmebaas, mida soovite korrata.
- Azure'i PowerShelli 1.0 või uuem versioon. Saate alla laadida ja installida Azure PowerShelli moodulid järgmised [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md)abil.


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfigureerige oma kasutajanimi ja parool ja valige oma tellimuse

Kõigepealt tuleb luua Accessi Azure'i kontosse nii käivitage PowerShelli ja seejärel käivitage järgmine cmdlet-käsk. Sisestage sisselogimine sama meiliaadressi ja parooliga, mida kasutada Azure portaali sisse logida.


    Login-AzureRmAccount

Pärast edukalt sisselogimisel kuvatakse osa teabest kuval, mis sisaldab olete sisse loginud ID-d ja Azure tellimused, kui teil on juurdepääs.


### <a name="select-your-azure-subscription"></a>Valige Azure tellimuse

Valige tellimus, peate tellimuse ID-ga. Saate kopeerida eelmises juhises kuvatav teave tellimuse Id, või kui teil on mitu tellimust ja te vajate rohkem üksikasju saate käivitage cmdlet-käsk **Get-AzureRmSubscription** ja kopeerige soovitud tellimuse teave selle resultset. Järgmine cmdlet-käsk kasutab praeguselt tellimuselt tellimuse Id:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Pärast edukalt töötab **Valige-AzureRmSubscription** naasete selle PowerShelli Küsi.


## <a name="add-secondary-database"></a>Teisene andmebaasi lisamine


Järgmised toimingud luua teisene uue andmebaasi Geo-Dispersioonanalüüs seos.  
  
Teisese lubamiseks peate olema tellimuse omanik või kaasomaniku. 

**Uus-AzureRmSqlDatabaseSecondary** cmdlet-käsu abil saate lisada teise andmebaasi partner server olete serveris kohaliku andmebaasiga ühendatud (esmane andmebaas). 

Selle cmdlet asendab **Algus-AzureSqlDatabaseCopy** koos parameetriga **– IsContinuous** .  See väljund **AzureRmSqlDatabaseSecondary** objekti, mida saate kasutada cmdletid selgeks tähistamiseks lingi teatud dispersioonanalüüs. Selle cmdlet-käsk tagastab kui teisene andmebaas on loodud ja täielikult seemnetega. Sõltuvalt andmebaasi suurus võib kuluda minutit tundi.

Tiražeeritud andmebaasi teisene server on sama nimi andmebaasi esmane serveris ja on vaikimisi teenuse samal tasemel. Teise andmebaasi saab lugeda või loetav ja võib olla üks andmebaas või elastne andmebaasis. Lisateavet leiate teemadest [New-AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) ja [Teenuse astme](sql-database-service-tiers.md).
Kui teisese loonud ja seemnetega, hakkavad andmete imitatsiooniga esmane andmebaasist uue teise andmebaasi. Järgmised juhised kirjeldavad selle ülesande PowerShelli kaudu-loetav ja on loetav sekundaaride, kas üks andmebaas või elastne andmebaasi loomiseks.

Kui partneri andmebaas on juba olemas (nt - tulemusena lõpetab eelmise Geo-Dispersioonanalüüs seose) käsk nurjub.



### <a name="add-a-non-readable-secondary-single-database"></a>Lisage-loetav teisese (ühe andmebaas)

Järgmine käsk loob-loetav teisese andmebaasi serveri "srv2" ressursi rühma "rg2", "mydb":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Lisage loetav teisese (ühe andmebaas)

Järgmine käsk loob loetav teisese andmebaasi serveri "srv2" ressursi rühma "rg2", "mydb":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Lisage-loetav teisese (elastne andmebaas)

Järgmine käsk loob-loetav teisese andmebaasi "mydb" nimega "ElasticPool1 server"srv2"ressursi rühma"rg2"," elastne andmebaasi kogumi:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Lisage loetav teisese (elastne andmebaas)

Järgmine käsk loob loetav teisese andmebaasi "mydb" nimega "ElasticPool1 server"srv2"ressursi rühma"rg2"," elastne andmebaasi kogumi:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Teisene andmebaasi eemaldamine

**Eemalda-AzureRmSqlDatabaseSecondary** cmdlet-käsu abil saate jäädavalt lõpetada kopeerimine teise andmebaasi ja oma esmase partnerlus. Pärast seose lõpetamist, saab teise andmebaasi lugemis-ja kirjutamisõigusega andmebaasi. Kui teisel andmebaasiga ühendust on katkenud käsu õnnestub, kuid teisese muutuvad lugemis-ja kirjutamisõigusega, kui ühendus on taastatud. Lisateabe saamiseks vt [Eemalda-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) ja [Teenuse astme](sql-database-service-tiers.md).

Selle cmdlet asendab Peata-AzureSqlDatabaseCopy kopeerimise. 

Jõustatud lõpetamise, mis eemaldab kopeerimise ja jätab endise teisese autonoomse andmebaasi, mis on täielikult kopeeritud enne lõpetamist võrdub selle eemaldamine. Kõik lingi andmed kuvatakse puhastada endise primaarne ja endise teisene, kui või kui need on saadaval. Selle cmdlet-käsk tagastab kui dispersioonanalüüs link on eemaldatud. 


Teisese kõrvaldada kasutajatel peaks olema nii põhi- ja andmebaaside vastavalt RBAC kirjutusõiguse. Teemast Rollipõhine juurdepääsu reguleerimine üksikasju.

Järgmine eemaldab dispersioonanalüüs link nimega "mydb", et server "srv2" ressursi rühma "rg2" andmebaasi. 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Geo-dispersioonanalüüs konfiguratsiooni ja seisundi jälgimine

Jälgimisega seotud ülesannete hulka geo-dispersioonanalüüs konfiguratsiooni ja andmete kopeerimine seisund.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) saab hankida teavet edasi dispersioonanalüüs linkide sys.geo_replication_links kataloogi vaates nähtav.

Järgmine käsk toob dispersioonanalüüs seos esmase andmebaasi "mydb" ja klõpsake serveri "srv2" ressursi rühma "rg2" teisese olekut.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Järgmised sammud

- Lisateavet kohta aktiivse Geo-kopeerimine, lugege teemat - [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md)
- Äri järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [äri järjepidevuse ülevaade](sql-database-business-continuity.md)

