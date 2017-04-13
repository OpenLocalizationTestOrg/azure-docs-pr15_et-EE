< atribuudid
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Kasutama uusimat elastne andmebaasi kliendi teek rakenduse täiendamine

Uus versioon [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md) on saadaval NuGetand NuGetPackage halduri kasutajaliidese Visual Studio kaudu. Täienduste sisaldavad veaparandused ja kasutajatugi uute võimaluste kliendi teek.

**Uusim versioon:** Avage [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Rakenduse uus teegiga taasloomine ning muuta oma olemasoleva Kildu kaardi halduri metaandmete talletatud teie Azure SQL-i andmebaasid funktsioonidega.

Järgmiste toimingute järjestuses tagab vanemad versioonid kliendi teek olemasolu enam teie keskkonnas kui metaandmete objektid on värskendatud, mis tähendab, et vana versioon metaandmete objekte ei looda pärast uuele versioonile üleminekut.   

## <a name="upgrade-steps"></a>Versioonitäienduse järgmiselt.

**1. uuendada oma rakendusi.** Visual Studio, laadige alla ja viidata uusim versioon kliendi teegi kõigi oma arengu projekte, mis kasutavad teek; seejärel uuesti luua ja juurutada. 

 * Visual Studio lahendusse, valige **Tööriistad** --> **Nugeti Package Manager** -->  **Haldamine Nugeti pakettide lahendus**. 
 * (Visual Studio 2013) Vasakul paanil valige **värskendused**ja seejärel paketti **Azure SQL-i andmebaasi elastne skaala kliendi teek** , mis kuvatakse aknas nuppu **Update** .
 * (Visual Studio 2015) Määrake väljal Filter versioonitäiendus **saadaval**. Valige värskendada pakett ja klõpsake nuppu **Värskenda** .
    
 
 * Luua ja juurutada. 

**2 uuendada oma skriptide.** Kui kasutate **PowerShelli** skriptide haldamiseks shards, [allalaadimine teegis uue versiooni](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) ja kopeerige see, millest täidate skriptide kataloogi. 

**3. Võtke kasutusele tükeldatud ühendamine teenust.** Kui kasutate elastne tükeldatud ühendamine andmebaasiriista ümber sharded andmed, [Laadige alla ja juurutamine tööriista uusima versiooni](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Üksikasjalikud juhised versioonitäienduse teenuse leiate [siit](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. oma Kildu kaardi halduri andmebaaside versiooni uuendamine**. Võtke kasutusele metaandmete toetavad oma Kildu kaartide Azure SQL-andmebaasis.  On kaks võimalust, võite saavutada selle, PowerShelli või C#. Mõlema variandi on näidatud allpool.

***Variant 1: Täiendada PowerShelli kaudu metaandmed***

1. Laadige alla käsurea kasuliku Nugeti [siia](http://nuget.org/nuget.exe) ja kausta salvestada. 

2. Avage käsuviip, liikuge samas kaustas ja küsimus käsk:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Liikuge alamkausta sisaldava uue klientrakenduse DLL versiooni saate lihtsalt alla laadinud, näiteks:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Elastne andmebaasi kliendi versioonitäienduse scriptlet alla laadida [Skripti keskele](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)ja salvestage see samasse kausta, mis sisaldab DLL.

5. Selles kaustas "PowerShelli.\upgrade.ps1" pidamine Käsuviip ja järgige viipasid.
 
***Variant 2: Versioonitäienduse metaandmete abil C#***

Teise võimalusena looge Visual Studio rakendus, mis avab teie ShardMapManager, itereerib üle kõik shards, ja sooritab metaandmete versiooniuuendus helistades [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) ja [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) meetodid, nagu järgmises näites: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Nende meetodite metaandmete uuendamine abil saate rakendada mitu korda ilma kahju. Näiteks kui kliendi varasem versioon loob kogemata mõne Kildu pärast värskendamist juba, saate käivitada versiooniuuenduse uuesti üle kõik shards metaandmete uusim versioon olemas kogu teie taristu tagamiseks. 

**Märkus:**  Uute versioonide kliendi teek avaldatud ajakohane töötada ka varasemate versioonide Kildu kaardi halduri metaandmete Azure SQL-i DB ja vastupidi.   Aga ära mõned uued funktsioonid uusima klient, peab metaandmete uuendada.   Pange tähele, et metaandmete uuendamine ei mõjuta iga kasutaja andmed või rakenduse kohased andmeid, ainult objektide loodud ja kasutatavaid Kildu kaardi ülemus.  Ja rakenduste toimima eespool kirjeldatud versioonitäienduse jada. 

## <a name="elastic-database-client-version-history"></a>Elastne andmebaasi kliendi versiooniajalugu 

Versiooniajalugu, minge [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 