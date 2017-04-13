<properties
   pageTitle="Azure'i andmed Factory kasutamine SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid kasutades Azure Factory (ADF) SQL Azure'i andmebaas lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Azure'i andmed Factory kasutamine SQL-andmebaas

Azure'i andmed Factory annab täielikult hallatud meetodi orkesteroinnin andmeid ja SQL-i andmebaas on salvestatud toimingute täitmist.  See võimaldab teil hõlpsam ülesseadmine ja ajakava keerukate ekstraktida transformatsioon ja laadi (ETL) toimingute abil SQL-i andmebaas. Azure'i andmed Factory täielikuma ülevaate leiate [Azure'i andmed Factory dokumentatsiooni][].

## <a name="data-movement"></a>Andmete liikumine

Azure'i andmed Factory võimaldab andmete liikumine nii asutusesisestest allikatest ja Azure teenuste vahel.  Kokkuvõttes praeguse integreerimine Azure andmete Factory toetab andmete liikumine ja sealt järgmisest kohast:

+ Azure'i bloobimälu
+ Azure'i SQL-andmebaas
+ Asutusesisese SQL serveriga
+ SQL Server IaaS

Andmete häälestamise kohta teavet teemast Kopeeri tegevuse [Azure'i andmed Factory andmete kopeerimine][]

## <a name="stored-procedures"></a>Salvestatud toimingute
 Seda saab kasutada andmete edastamiseks plaanida samal viisil Azure'i andmed Factory saate kasutada ka korraldab salvestatud toimingute täitmist.  See võimaldab keerukamaid torujuhtmete luua ja laiendab Azure'i andmed Factory võimalus kasutada arvutuslikke power SQL-i andmebaas.

## <a name="next-steps"></a>Järgmised sammud
Integreerimine ülevaate leiate teemast [SQL-i andmebaas integreerimine ülevaade][].
Veel arengu näpunäiteid teemast [SQL-i andmebaas arengu ülevaade][].

<!--Image references-->

<!--Article references-->

[Azure'i andmed Factory andmete kopeerimine]: ../data-factory/data-factory-data-movement-activities.md
[SQL-i andmebaas arengu ülevaade]: ./sql-data-warehouse-overview-develop.md
[SQL-i andmebaas integreerimine ülevaade]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure'i andmed Factory dokumentatsioon]:https://azure.microsoft.com/documentation/services/data-factory/

