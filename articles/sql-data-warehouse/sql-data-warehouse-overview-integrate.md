<properties
   pageTitle="SQL-i andmebaas koos integreeritud lahenduste koostamine | Microsoft Azure'i"
   description="Tööriistad ja lahendusi, mis SQL-i andmebaas integreerida partnerite. "
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
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>SQL-i andmebaas koos muude teenustega kasutada.
Lisaks oma põhifunktsioone SQL-i andmebaas võimaldab kasutajatel paljude muude kõrval Azure teenustega kasutada.  Täpsemalt me praegu on võtnud juhiseid sügavalt integreerida järgmist:

+ Power BI
+ Azure'i andmed Factory
+ Azure'i masina õpetused
+ Azure'i voo Analytics

Töötame üle Azure ökosüsteemi rohkem teenuseid suhtlemiseks.

##<a name="power-bi"></a>Power BI
Power BI integreerimine võimaldab teil ära kasutada Arvuta power SQL-i andmebaas koos dünaamilise aruandlus ja visualiseerimine Power BI. Power BI integreerimine praegu sisaldab järgmist:

+ **Otse ühendust luua**: täpsemat ühenduse loogilise pinustamine vastu SQL-i andmebaas.  See pakub kiirem analüüsi suuremas ulatuses.
+ **Avage Power BI**: nupu Ava Power BI edastab eksemplari teavet Power BI, võimaldades rohkem sujuvalt ühenduse.

[Integreerida Power BI](./sql-data-warehouse-integrate-power-bi.md) ja [Power BI dokumentatsiooni](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) kohta lisateabe saamiseks vt.

##<a name="azure-data-factory"></a>Azure'i andmed Factory
Azure'i andmed Factory annab kasutajatele hallatavate platvormi keerulise ekstrakti-laadi torustike loomiseks.  SQL-i andmebaas integreerimine Azure andmete Factory sisaldab järgmist:

+ **Salvestatud toimingute**: korraldab SQL-i andmebaas on salvestatud toimingute täitmist.
+ **Koopia**: kasutamine andmete teisaldamiseks SQL-i andmebaas ADF.  Seda toimingut saate kasutada ADF's standard andmete liikumine süsteem või PolyBase all hõlmab. 

Leiate [Azure'i andmed Factory koos integreerida](./sql-data-warehouse-integrate-azure-data-factory.md) või [Azure'i andmed Factory dokumentatsiooni kohta](https://azure.microsoft.com/documentation/services/data-factory/) lisateabe saamiseks.

##<a name="azure-machine-learning"></a>Azure'i masina õpetused
Azure'i masina õppimine on täielikult hallatud Kasutusanalüüsi teenus, mis võimaldab kasutajatel luua keerulisi mudeleid suure hulga sõnastikupõhise tööriistade kasutamine.  SQL-i andmebaas on toetatud nii lähte-ja sihtkoha mudelite järgmised funktsioonid:

+ **Andmeid lugeda:** Juhtida mudelite skaala vastu SQL-i andmebaas T-SQL-i abil.
+ **Andmete kirjutamiseks:** Kinnita muudatused mis tahes mudel naasmiseks SQL-i andmebaas.

Teemast [Azure seadme õppimisega integreerida](./sql-data-warehouse-integrate-azure-machine-learning.md) või lisateabe saamiseks [Azure'i masina õ dokumentatsiooni](https://azure.microsoft.com/services/machine-learning/) .

##<a name="azure-stream-analytics"></a>Azure'i voo Analytics
Azure'i voo Analytics on keerukas, täielikult hallatud taristu töötlemiseks ja tarbimine Azure'i sündmuse keskuse kaudu loodud sündmuse andmed.  SQL-i andmebaas integreerimine võimaldab streaming tõhusalt töödelda ja lubamine süvitsi, täpsemate analüüsi relatsiooniliste andmete kõrval talletatud andmed.  

+ **Töö väljund:** Väljundi voo Analytics töökohta otse saata SQL-i andmebaas.

Leiate [Azure'i voo Analytics integreerida](./sql-data-warehouse-integrate-azure-stream-analytics.md) või [Azure voo Analytics dokumentatsiooni kohta](https://azure.microsoft.com/documentation/services/stream-analytics/) lisateabe saamiseks.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
