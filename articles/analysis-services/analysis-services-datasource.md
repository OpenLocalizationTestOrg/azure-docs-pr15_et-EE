<properties
   pageTitle="Andmeallika ühendusi | Microsoft Azure'i"
   description="Kirjeldatakse andmeallikaühendused Azure analüüsiteenuste andmemudelite jaoks."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Andmeallika ühendusi

Azure'i analüüsiteenuste andmemudelid nõuda eri andmepakkujate teatud andmeallikate ühendamisel. Mõnel juhul ühenduse andmeallikate kohalikke pakkujad, nt SQL serveri Omakliendi (SQLNCLI11) tabelmudelites võib ilmneda tõrge.

Näiteks; Kui teil on mõni-mälu või otsese päringu andmemudeli, mis ühendab cloud andmeallikaga Azure'i SQL-andmebaasi, näiteks kui kasutate kohalikke pakkujad peale SQLOLEDB, võidakse kuvada tõrketeade: **"Teenusepakkuja"SQLNCLI11.1"pole registreeritud"**.

Või kui teil DirectQuery mudel kohapealse andmeallikaga ühenduse loomine, kui kasutate kohalikke pakkujad võidakse kuvada tõrketeade: **"OLE DB rea määramine loomisel ilmnes tõrge. Vale süntaks lähedal 'Limiit' "**.

## <a name="data-source-providers"></a>Andmeallika andmepakkujate

Järgmised andmeallika pakkujad on toetatud-mälu otse päringu andmemudelite kohapealse ühendamisel või cloud andmeallikatega:

|               | **Andmeallikas**                     | **-Mälu**                            |  **Otsest päring**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Pilveteenuse**                     | SQL Azure'i andmebaas      | .NET raamistiku andmepakkuja SQL serverile | .NET raamistiku andmepakkuja SQL serverile |
|                           | Azure'i SQL-andmebaas            | .NET raamistiku andmepakkuja SQL serverile | .NET raamistiku andmepakkuja SQL serverile |
| **Kohapealse** (lüüsi) kaudu | SQL serveri                    | SQL serveri Omakliendi 11.0               | .NET raamistiku andmepakkuja SQL serverile |
|                           |  SQL serveri                             | Microsoft OLE DB pakkuja SQL serverile    |   .NET raamistiku andmepakkuja SQL serverile                                          |
|                           |  SQL serveri                             | .NET raamistiku andmepakkuja SQL serverile |  .NET raamistiku andmepakkuja SQL serverile                                           |
|                           | Oracle'i                        | Oracle Microsoft OLE DB pakkuja        | Oracle'i andmepakkuja .net-i jaoks               |
|                           |  Oracle'i                             | Oracle'i andmepakkuja .net-i jaoks               | Oracle'i andmepakkuja .net-i jaoks                                            |
|                           | Teradata                      | OLE DB pakkuja Teradata jaoks                | Teradata andmepakkuja .net-i jaoks             |
|                           |  Teradata                             | Teradata andmepakkuja .net-i jaoks             |  Teradata andmepakkuja .net-i jaoks                                            |
|                           | Kasutusanalüüsi platvormi süsteemi | .NET raamistiku andmepakkuja SQL serverile | .NET raamistiku andmepakkuja SQL serverile |


> [AZURE.NOTE] Veenduge, et 64-bitine pakkujat pole installitud kohapealse lüüsi kasutamisel.

Kui migreerimine on asutusesisese SQL Serveri analüüsiteenuste tabelmudeli Azure Analysis Services, võib olla vaja muuta pakkuja.

**Kui soovite määrata andmeallika pakkuja**

1. SSDT > **Tabular Model Exploreri** > **Andmeallikad**, paremklõpsake andmeallikaühenduse ja seejärel klõpsake nuppu **Redigeeri andmeallikat**.

2. **Ühenduse redigeerimine**nuppu **Täpsemalt** Advance atribuutide akna avamiseks.

3. **Määrake Täpsemad**atribuudid > **pakkujad**, seejärel valige sobiv pakkuja.

## <a name="impersonation"></a>Isikustamine
Mõnel juhul võib olla vaja määrata eri isikustamine konto. Saate määrata SSDT või SSMS isikustamine konto.

Kohapealse andmeallikaid:

- Kui SQL-i autentimist kasutades, tuleks isikustamine teenusekonto.
- Kui Windowsi autentimist kasutades seada Windowsi kasutaja ja parooli. SQL Server, teatud isikustamine kontoga Windowsi autentimine on toetatud ainult-mälu andmemudelite.

Pilveteenuse andmeallikaid:

- Kui SQL-i autentimist kasutades, tuleks isikustamine teenusekonto.


## <a name="next-steps"></a>Järgmised sammud

Kui teil on kohapealse andmeallikad, kindlasti [kohapealse lüüsi](analysis-services-gateway.md)installimine. Oma serveri SSDT või SSMS haldamise kohta leiate lisateavet teemast [haldamine serveris](analysis-services-manage.md).
