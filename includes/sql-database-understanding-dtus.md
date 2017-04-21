Andmebaasi tehingu üksus (DTU) on SQL-andmebaasis, mis tähistab suhteline power andmebaaside põhjal tegelike mõõt mõõtühik: andmebaasi tehingu. Me võttis toimingute, mis on tüüpilised online toimingu päringut (OLTP) ja seejärel mõõta, kui palju tehinguid lõpuleviimist sekundis jaotises täielikult laaditud tingimused (see on lühendatud, saate lugeda Verine üksikasjad [võrdlusalus ülevaade](../articles/sql-database/sql-database-benchmark-overview.md)). 

Näiteks pakub Premium P11 andmebaasi 1750 DTUs 350 x rohkem DTU arvutada power kui 5 DTUs lihtsa andmebaasi. 

![SQL-andmebaasi Sissejuhatus: ühe andmebaasi DTUs taseme ja tase.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Kui migreerite olemasoleva SQL Serveri andmebaasiga, saate teada, kui tulemuslikkuse ja andmebaasi võivad nõuda Azure SQL-andmebaasis taseme kolmanda osapoole tööriista, [Azure SQL-i andmebaasi DTU kalkulaator](http://dtucalculator.azurewebsites.net/).

### <a name="dtu-vs-edtu"></a>DTU vs eDTU

Ühe andmebaaside DTU vaste otse eDTU elastne andmebaaside jaoks. Näiteks lihtsa elastne andmebaasi pargis andmebaasi pakub kuni 5 eDTUs. Mis on sama nimega ühekordse lihtsa andmebaasi jõudlus. Erinevus on, et elastne andmebaasi ei igal pool eDTUs seni, kuni see. 

![SQL-andmebaasi Sissejuhatus: elastne kaustu astme järgi.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Lihtne näide aitab. Võtke lihtne elastne andmebaasi rakenduskausta koos 1000 DTUs ja kukutage see 800 andmebaasid. Kui mis tahes hetkel kasutatakse ainult 200 800 andmebaaside kellaaeg (5 DTU X 200 = 1000), ei klõpsamist pool võimsus ja andmebaasi jõudlust ei halvendada. Selles näites on lihtsustatud selgust. Reaal matemaatika on küll natuke keerulisem. Portaali ei matemaatika teile ja teeb soovituse andmebaas kasutuse põhjal. Vaadake [hind ja jõudluse kaalutluste kohta kohapeal on elastne andmebaasi](../articles/sql-database/sql-database-elastic-pool-guidance.md) selgeks õppimiseks soovitusi või matemaatika ise teha. 
