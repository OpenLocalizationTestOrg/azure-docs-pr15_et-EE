<properties 
   pageTitle="SQL Azure'i andmebaasi jõudluse ülevaate | Microsoft Azure'i" 
   description="Azure'i SQL-andmebaasi pakub jõudluse tööriistu, mis aitavad teil tuvastada ala, mida saate praeguse päringu jõudlust parandada." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>SQL-i andmebaasi jõudluse tutvustus

SQL Azure'i andmebaas pakub jõudluse tööriistu, mis aitavad teil tuvastada ja parandada oma andmebaasi nutikad häälestamise toimingud ja soovitused. 

1. Otsige sirvides üles oma andmebaasi [Azure portaali](http://portal.azure.com) ja klõpsake nuppu **Kõik sätted** > **jõudluse **  >  **Ülevaade** **jõudluse** lehe avamiseks. 


2. Klõpsake **soovitusi** [SQL-i andmebaasi Advisor](#sql-database-advisor)avamiseks ja **päringute** [Päringu jõudluse ülevaate](#query-performance-insight)avamiseks klõpsake.

    ![Vaate jõudlus](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Jõudluse ülevaade

Klõpsake paani **jõudluse** või **Ülevaade** viib teid andmebaasi jõudluse armatuurlaud. See vaade pakub teie andmebaasi jõudlust kokkuvõtet ja aitab teil jõudluse parandamine ja tõrkeotsing. 

![Jõudluse](./media/sql-database-performance/performance.png)

- **Soovitused** paani pakub häälestamine oma andmebaasi soovitused jaotus (top 3 soovitused kuvatakse, kui seal on rohkem). Sellel paanil klõpsamine viib teid **SQL-i andmebaasi Advisor**. 
- Paani **Tuning tegevuse** annab ülevaate poolelioleva ja lõpuleviidud toimingute andmebaasi häälestamine, võimaldades kiirvaade häälestamine tegevuse ajalugu. Sellel paanil klõpsamine viib teid andmebaasi täielik häälestamise ajaloo kuvamine.
- **Automaatne häälestamine** paani kuvatakse automaatne häälestamine (häälestamise toimingud on konfigureeritud automaatselt rakendada andmebaasi) andmebaasi konfigureerimine. Sellel paanil klõpsates avatakse automatiseerimise konfiguratsiooni dialoogi.
- Paani **andmebaasipäringud** kuvatakse Kokkuvõte päringu jõudluse andmebaasi (üldine DTU kasutus- ja ülemine ressurss tarbimine päringud). Sellel paanil klõpsamine viib teid **Päringu jõudluse ülevaate**.



## <a name="sql-database-advisor"></a>SQL-i andmebaasi Advisor


[SQL-i andmebaasi Advisor](sql-database-advisor.md) pakub nutikad häälestamise soovitused, mis aitavad teie andmebaasi jõudlust parandada. 

- Soovitused mis indeksite loomiseks või langev (ja suvandi index soovitused automaatselt ilma mis tahes kasutaja suhtluse ja automaatselt tagasipööramine soovitusi, mis on negatiivset mõju jõudluse rakendamiseks).
- Soovitused kui avastatakse andmebaasi skeemi probleeme.
- Soovitused, kui päringud saavad parameetriga päringud.




## <a name="query-performance-insight"></a>Päringu täitmise ülevaade

[Päringu täitmise ülevaate](sql-database-query-performance.md) võimaldab vähem aega, andes andmebaasi jõudluse tõrkeotsingu:

- Teie andmebaasi ressursi (DTU) tarbimine süvitsi ülevaate. 
- Ülemise CPU tarbimine päringud, mis võib olla häälestatud paranenud jõudlus. 
- Võimalus süvitsiminek päringu üksikasju. 


## <a name="additional-resources"></a>Lisaressursid

- [Azure'i SQL-andmebaasi jõudluse juhiseid ühe andmebaaside jaoks](sql-database-performance-guidance.md)
- [Millal tuleks kasutada elastne andmebaasi kohapeal on?](sql-database-elastic-pool-guidance.md)