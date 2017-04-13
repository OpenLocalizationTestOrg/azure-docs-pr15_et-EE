<properties
    pageTitle="Jõudluse hinnale Kildu kaardi Manager"
    description="ShardMapManager klassi ja andmed sõltuvad marsruutimise jõudluse hinnale"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Jõudluse hinnale Kildu kaardi Manager

Saate hõivata jõudlus [Kildu kaardi halduri](sql-database-elastic-scale-shard-map-management.md), eriti siis, kui kasutate [andmed sõltuvad marsruutimist](sql-database-elastic-scale-data-dependent-routing.md). Hinnale on loodud meetodid Microsoft.Azure.SqlDatabase.ElasticScale.Client klassi.  

Hinnale kasutatakse [andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md) toimingute jälgimiseks. Nende hinnale juurdepääsetavat jõudluse monitoris, kategoorias "Elastne andmebaasi: Kildu haldamine".

**Uusim versioon:** Avage [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Vt ka [kasutama uusimat elastne andmebaasi kliendi teek rakenduse täiendamine](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Eeltingimused

* Jõudluse kategooria ja hinnale loomiseks kasutaja peab olema osa kohalike **administraatorite** rühma hosting taotlus arvutis.  

* Jõudluse counter eksemplari loomine ja värskendamine on hinnale, kasutaja peab olema kas **Administraatorid** või **Jõudluse kuvari kasutajate** rühma liige. 

## <a name="create-performance-category-and-counters"></a>Jõudluse kategooria ja hinnale loomine 

Helistage soovitud hinnale loomiseks [ShardMapManagmentFactory klassi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx)CreatePeformanceCategoryAndCounters meetod. Ainult administraator saab käivitada meetodit: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Samuti saate [selles](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShelli skripti käivitada meetodit. Meetodi loob järgmised jõudluse hinnale.  

* **Vahemälus talletatud vastendused**: vastendused vahemälus talletatud Kildu kaardi arv.
*  **DDR sekundis**: määr andmete sõltuvad marsruutimise toimingute Kildu kaarti. See loendur värskendatakse, kui kõne eduka ühenduse sihtkoht Kildu [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) tulemusi. 
*  **Otsingu Vahemälutabamusi sekundis vastendamise**: määr eduka vahemälu otsing toimingute vastendused Kildu kaart. 
*  **Otsing vahemälu jätab sekundis vastendamise**: määr nurjunud vahemälu otsing toimingute vastendused Kildu kaart.
*  **Vastendused lisatud või uuendatud vahemälu sekundis**: määra mis vastendused lisatakse või värskendati vahemälu Kildu kaardi. 
*  **Vastendused eemaldatakse vahemälu sekundis**: määr, kus on vastendused eemaldatakse vahemälu Kildu kaardi. 

Jõudluse hinnale luuakse iga vahemällu talletatud Kildu kaart protsessi kohta.  


## <a name="notes"></a>Märkmete
Järgmised sündmused käivitamine jõudluse hinnale loomine:  

* Lähtestamine [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) [alustamisjuhendi laadimise](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), kui selle ShardMapManager sisaldab Kildu kaart. Nendeks [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) ja [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) meetoditest.
* Eduka otsingu Kildu kaardi ( [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) või [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)abil). 

* Kildu kaardi CreateShardMap() abil luua.

Jõudluse hinnale värskendatakse kõik vahemälu toimuvad Kildu kaardil ja vastendused. Eduka väljasaatmise Kildu kaardi kustutamise jõudluse hinnale astme reults DeleteShardMap () kasutamine.  

## <a name="best-practices"></a>Head tavad

* Jõudluse kategooria ja hinnale loomiseks tuleb teha ainult üks kord enne ShardMapManager objekti loomine. Iga käsu täitmine CreatePerformanceCategoryAndCounters() kustutab eelmise hinnale, (kaotamise andmetele, läbivalt kogu) ja loob uued.  

* Jõudluse counter eksemplarid luuakse protsessi kohta. Mis tahes rakenduse krahh või eemaldamine Kildu kaardi vahemälust tulemuseks jõudluse hinnale eksemplaride kustutamine.  

### <a name="see-also"></a>Vt ka

[Elastne andmebaasi funktsioonide ülevaade](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

