<properties
    pageTitle="Versioonile Azure SQL-i andmebaasi V12 PowerShelli kaudu | Microsoft Azure'i"
    description="Selgitab, kuidas võtta kasutusele Azure SQL-i andmebaasi V12 sh kuidas veebi ja andmebaaside versiooni uuendamine ja kuidas uuendada oma andmebaase otse PowerShelli kaudu kohapeal on elastne andmebaasi migreerimine V11 server."
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
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Versioonile Azure SQL-i andmebaasi V12 PowerShelli abil


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-upgrade-server-portal.md)
- [PowerShelli](sql-database-upgrade-server-powershell.md)


SQL-i andmebaasi V12 on uusima versiooni täiendamine versiooniks SQL-i andmebaasi V12 on soovitatav.
SQL-i andmebaasi V12 on palju [eelmise versiooni eeliseid](sql-database-v12-whats-new.md) , sh:

- SQL serveri täiustab ühilduvust.
- Täiustatud premium jõudlus ja jõudluse uuele tasemele.
- [Elastne andmebaasi kaustu](sql-database-elastic-pool.md).

Selles artiklis antakse juhiseid olemasoleva SQL-i andmebaasi V11 servers ja andmebaaside uuendamise SQL-i andmebaasi v12.

V12 täiendamise käigus versiooniks mis tahes veebi ja andmebaaside uue teenuse kiht nii, et juhised veebi ja andmebaaside uuendamise kaasatakse.

Lisaks on [pool elastne andmebaasi](sql-database-elastic-pool.md) migreerimine võib olla kulude tõhusam kui üleminekut üksikute jõudluse tasemeid (hinnad astme) ühe andmebaaside jaoks. Kaustu lihtsustada ka andmebaasi haldamine, sest teil on vaja vaid pool, mitte eraldi jõudluse tasemeid üksikute andmebaaside haldamine jõudluse sätete haldamine. Kui teil on mitu serverites andmebaaside, teisaldage need sama serveri ja laskmiseks on ära.

Saate järgige selle artikli hõlpsalt migreerimine andmebaaside V11 servers otse elastne andmebaasi kaustadesse.

Pange tähele, et teie andmebaasid on kogu aeg olema võrgus versioonitäienduse kogu tööd jätkata. Uus ajutine pukseerimine ühenduste andmebaasi jõudlusega tegeliku ülemineku ajal võib olla väga väike kestus, mis on tavaliselt umbes 90 sekundit, kuid võib olla nii palju kui 5 minutit. Kui teie taotlus on [ajutine viga käsitsemise ühenduse klemmliidesed](sql-database-connectivity-issues.md) siis piisab versioonitäienduse lõpus ühenduse katkestuste eest kaitsta.

SQL-i andmebaasi v12 üleminekut ei saa tagasi võtta. Pärast täiendamist ei saa serveris taastatud V11 abil.

Pärast V12, [teenuse taseme soovitused](sql-database-service-tier-advisor.md) ja [elastne rakenduskausta soovitused](sql-database-elastic-pool-create-portal.md) kohe on kättesaadavad seni, kuni teenust on aeg hindamaks, kui teie töökoormus uues serveris. V11 serveri soovitus ajalugu ei kehti V12 serverid nii, et seda ei säilitata.  

## <a name="prepare-to-upgrade"></a>Versioonitäienduse ettevalmistamiseks

- **Kõik veebi ja andmebaaside versiooni uuendamine**: kasutada portaali või [uuendada andmebaasid ja serveri PowerShelli](sql-database-upgrade-server-powershell.md)kasutamine.
- **Läbivaatus ja peatada Geo-Dispersioonanalüüs**: kui SQL Azure'i andmebaasi on konfigureeritud lubama Geo-Dispersioonanalüüs teie dokument oma praeguse konfiguratsiooni ja [Geo-Dispersioonanalüüs lõpetada](sql-database-geo-replication-portal.md#remove-secondary-database). Pärast versioonitäienduse lõpulejõudmist taaskonfigureerimine Geo-kopeerimise andmebaasi.
- **Avage järgmised pordid, kui teil on mõne Azure'i VM kliendid**: kui teie klient programmi loob ühenduse SQL-i andmebaasi V12 ajal Azure virtuaalse masina (VM) oma kliendi töötab, peate avama pordi vahemike 11000-11999 ja 14000-14999 VM. Lisateavet leiate teemast [V12 SQL-i andmebaasi Porte](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Eeltingimused

Uuendada serveri V12 PowerShelliga, peate uusim Azure PowerShelli installitud ja töötab. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfigureerige oma kasutajanimi ja parool ja valige oma tellimuse

Käivitage PowerShelli cmdlet-käskude Azure tellimuse eest tuleb kõigepealt luua Accessi Azure'i kontosse. Käivitage järgmised ja ilmub sisselogimise ekraan sisestama oma mandaadi. Kasutage sama meiliaadressi ja parooliga, mida kasutada Azure portaali sisse logida.

    Add-AzureRmAccount

Pärast sisselogimist edukalt peaksite nägema ekraanil, mida olete sisse loginud Id ja teil on juurdepääs Azure'i tellimused sisaldab osa teabest.

Valige tellimus, mida soovite koos teiega peate oma tellimuse Id (**-SubscriptionId**) või tellimuse nime (**-SubscriptionName**). Saate kopeerida eelmises etapis või kui teil on mitu tellimust käivitada cmdleti **Get-AzureRmSubscription** ja kopeerige soovitud tellimuse teave selle resultset.

Teie tellimuse teavet, et määrata oma praeguse tellimuse käivitage järgmine cmdlet-käsk:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Ainult valitud kohal vastu tellimuse käivitatakse järgmised käsud.

## <a name="get-recommendations"></a>Soovituste hankimine

Saada soovitust serveri värskendamine, käivitage järgmine cmdlet-käsk:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Lisateabe saamiseks vt [loomine kohapeal on elastne andmebaasi](sql-database-elastic-pool-create-portal.md) ja [Azure'i SQL-andmebaasi hinnad taseme soovitusi](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Versioonitäienduse käivitamine

Server, käivitage järgmine cmdlet-käsk täiendamise alustamiseks tehke järgmist.

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Kui käivitate selle käsu versiooniuuendus hakkab. Saate kohandada väljundi soovituse ja sisestage redigeeritud soovitus selle cmdlet-käsu.


## <a name="upgrade-a-server"></a>Serveri versiooni täiendamine


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Kohandatud versioonitäiendus kaardistamine

Kui soovitusi ei ole sobivad oma serveri ja ettevõtte puhul, siis saate valida, kuidas oma andmebaasid on uuendatud ja saab vastendada ühe- või elastne andmebaasi.

ElasticPoolCollection ja DatabaseCollection parameetrid on valikulised.

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Pärast üleminekut versioonile SQL-i andmebaasi V12 andmebaaside jälgimine


Pärast üleminekut, on soovitatav jälgida andmebaasi tagama rakenduste töötab soovitud jõudluse ja optimeerimine kasutus vastavalt vajadusele.

Lisaks üksikud andmebaasid saate jälgida elastne andmebaasi kaustu [portaalis](sql-database-elastic-pool-manage-portal.md) jälgimiseks või [PowerShelli abil](sql-database-elastic-pool-manage-powershell.md)


**Ressursi andmed:** Basic, Standard, ja andmebaaside ressursi andmed on saadaval on [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV kasutaja andmebaasi kaudu. See DMV pakub lähedal reaalajas ressursside tarbimine teavet 15 teise granulaarsus toiming eelmise tunni jaoks. Intervalli DTU protsent tarbimine arvutatakse järgmiselt: maksimaalne protsent tarbimine CPU, IO ja log mõõtmed. Siin on päringu arvutada Keskmine DTU protsent tarbimine viimase tunni jooksul.

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Täiendavad jälgimisega seotud teave.

- [Azure'i SQL-andmebaasi jõudluse juhiseid ühe andmebaaside jaoks](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Hind ja jõudluse kaalutluste kohta kohapeal on elastne andmebaasi](sql-database-elastic-pool-guidance.md).
- [Azure'i SQL-andmebaasi dünaamilise halduse vaadete abil jälgimine](sql-database-monitoring-with-dmvs.md)



**Teatiste:** Häälestada 'Teatiste' Azure'i portaalis märku DTU tarbimine täiendatud andmebaasi lähenedes teatud kõrge. Andmebaasi teatiste saate häälestada erinevate jõudluse mõõdikute DTU, CPU, IO ja samamoodi nagu Azure'i portaalis. Otsige sirvides üles oma andmebaasi ja valige **sätted** tera **teatiste reeglid** .

Näiteks saate häälestada meiliteatise klõpsake "DTU protsent" kui keskmine DTU protsent väärtus on suurem kui 75% viimase viie minuti jooksul. Vaadake lisateavet selle kohta, kuidas konfigureerida teatiste [teatiste](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vastuvõtmine.



## <a name="next-steps"></a>Järgmised sammud

- [Create kohapeal on elastne andmebaasi](sql-database-elastic-pool-create-portal.md) ja lisada pool mõned või kõik andmebaasid.
- Saate [muuta oma andmebaasi teenuse taseme- ja jõudluse](sql-database-scale-up.md).



## <a name="related-information"></a>Seotud teave

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Algus-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Lõpeta – AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
