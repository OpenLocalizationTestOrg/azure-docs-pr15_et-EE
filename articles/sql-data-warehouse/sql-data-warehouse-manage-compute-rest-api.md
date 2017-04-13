<properties
   pageTitle="Arvuta power rakenduses Azure SQL-i andmete ladu (REST) haldamine | Microsoft Azure'i"
   description="PowerShelli ülesannete haldamiseks Arvutage power. Skaala Arvuta ressursid, reguleerides DWUs. Või Peata ja Jätka arvutada ressursside kulude salvestamiseks."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Arvuta power rakenduses Azure SQL-i andmete ladu (REST) haldamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-data-warehouse-manage-compute-overview.md)
- [Portaal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShelli](sql-data-warehouse-manage-compute-powershell.md)
- [ÜLEJÄÄNUD](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaala jõudluse skaala läbi arvutada materjale ja mälu muutmisega nõudmistele oma töökoormus. Salvestada kulud skaleerimise tagasi ressursid-tippväärtus korda või otsingurakenduse Arvuta ajal täielikult eemaldada. 

Selle saidikogumi tööülesannete kasutab Azure'i portaalis.

- Arvuta skaala
- Paus Arvuta
- Elulookirjelduse Arvuta

Teavet selle kohta leiate teemast [haldamine arvutada ülevaade][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skaala Arvuta power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Saate muuta selle DWUs, [loomine või värskendamine andmebaasi][] REST API. Järgmises näites määrab selle teenuse DW1000 andmebaasi MySQLDW, mis on majutatud serveris minuserver. Server on Azure ressursirühm, nimega ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Paus Arvuta

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Kasutage andmebaasi peatamiseks [Viige andmebaasi][] REST API-ga. Järgmises näites peatab Database02 majutatud serveris nimega Server01 andmebaas. Server on Azure ressursirühm, nimega ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Elulookirjelduse Arvuta

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Kasutage andmebaasi alustamiseks [Elulookirjelduse andmebaasi][] REST API-ga. Järgmises näites alustab Database02 majutatud serveris nimega Server01 andmebaas. Server on Azure ressursirühm, nimega ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Järgmised sammud

Muude haldustoimingute teemast [halduse ülevaade][].

<!--Image references-->

<!--Article references-->
[Juhtimise ülevaade]: ./sql-data-warehouse-overview-manage.md
[Arvuta ülevaade haldamine]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Paus andmebaas]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Elulookirjelduse andmebaas]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Loomiseks või andmebaasi värskendamine]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
