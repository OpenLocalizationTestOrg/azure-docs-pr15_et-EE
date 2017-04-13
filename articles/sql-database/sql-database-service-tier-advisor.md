<properties 
   pageTitle="Esimese taseme soovitused Azure'i SQL-andmebaasi hinnad" 
   description="Muutmisel hinnad astme hinnad taseme soovitused Azure'i portaalis on soovitada taseme, mis on parim sobib töötab olemasoleva Azure SQL-i andmebaasi tema töökoormus. Hinnakirjad astme kirjeldada SQL-andmebaasi teenuse taseme- ja jõudluse tase." 
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
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL-andmebaasi hinnakirjad taseme soovitused

 Esimese taseme hinnakirjad soovitused soovitamine teenuse taseme- ja jõudluse taset, mis sobib kõige paremini töötab mõne olemasoleva SQL Azure'i andmebaasi töökoormus.

> [AZURE.NOTE] Hinnakirjad taseme soovitused on ainult saadaval veebi ja andmebaaside ja elastne andmebaasi kaustu--ja saadaval ainult [Azure portaali](https://portal.azure.com/).


Esimese taseme soovitused saada hinnad ajal järgmisi toiminguid:

- [Teenuse taseme- ja jõudluse taseme muutmine (hinnad taseme) SQL-andmebaasiga](sql-database-scale-up.md)
- [Azure SQL serveri versioonile V12](sql-database-upgrade-server-portal.md)
- Liikuge sirvides oma V12 server. Vt [SQL-andmebaasi hinnad taseme soovitusi](sql-database-service-tier-advisor.md).
- [Kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Ülevaade

SQL-andmebaasi teenuse analüüsib praeguse jõudlus ja funktsioon nõuded hinnata ajalooliste ressursikasutus SQL-i andmebaasi. Lisaks määratakse lubatud teenuste taseme põhjal andmebaas ja lubatud [järjepidevuse](sql-database-business-continuity.md) funktsioonid suurust. 

See teave on analüüsida ja teenuse taseme- ja jõudluse taset, mis on parim sobib töötab soovitud andmebaasi tüüpilised töökoormus ja säilitades praeguse funktsioonikomplekt on soovitatav on kasutada.

- Teenuse uurib eelmise 15-30 päeva varasemate andmete (ressursikasutus, andmebaasi suurus ja andmebaasi tegevus) ja teeb tarbitud ressursside hulga ja tegeliku piirangud on praegu saadaval teenuste astme ja jõudlust võrdlus.
- Andmed on analüüsitud 15 teise intervallide ja iga intervalli resultset on liigitada olemasoleva teenuse kiht ja jõudluse taset, mis on parim paremini käsitlemise selle resultset töökoormus.
- Suurem 15-30 päeva analüüsi seejärel summeeritakse need 15 teise näidiseid ja teenuste taseme- ja jõudluse taset, kus saate hallata optimaalselt 95% ajalooliste töökoormus on soovitatav.

### <a name="recommendations"></a>Soovitused

Vastavalt oma andmebaasi kasutus, on praegu 2 kategooriat soovitusi, mis võivad ette tulla.


| Soovitused | Kirjeldus |
| :--- | :--- |
| Versiooni täiendamine | Võtke kasutusele uue taseme. |
| Pole saadaval | Andmebaasi jaoks on vaja vähemalt töökoormus või ligikaudu 35 tööpäevade arv. Pole piisavalt andmeid sisestada kehtivate soovitus. |

## <a name="getting-pricing-tier-recommendations"></a>Saada hinnad taseme soovitused

Saada hinnad olemasoleva andmebaasi veebi- ja valides taseme soovitusi, **Kõik sätted**, klõpsake käsku **hinnakirjad taseme (skaala DTUs)**. (Hinnakirjad taseme soovitusi, on ka teie [täiendamine Azure SQL serveri v12](sql-database-upgrade-server-portal.md).)

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Klõpsake nuppu **SIRVI** > **SQL andmebaase**.
4. Klõpsake **SQL andmebaase** labale andmebaas, mida soovite näha soovitusi.

    ![Valige andmebaas][1]

5. Enne andmebaasi, valige **Kõik sätted** seejärel **hinnakirjad taseme (skaala DTUs)**.


7. **Soovitatav hinnad astme** avamine, kus saab soovitatud tasandi nuppu ja seejärel klõpsake nuppu **Valige** teise astme muutmiseks.

    ![Eelvaate kasutajaks registreerumine][4]

8. Klõpsake **vaate kasutusteavet** **Hinnad taseme soovitus üksikasjad** tera soovitatav taseme andmebaasi, funktsioonide võrdlus praeguse ja Soovitatavad astme vahel, ajalooliste ressursi Kasutusanalüüsi graafiku vaatamiseks avada;

    ![Eelvaate kasutajaks registreerumine][5]



## <a name="summary"></a>Kokkuvõte

Hinnakirjad taseme soovitused aitavad automatiseeritud kogemus iga SQL-andmebaasi telemeetria andmete kogumine ja soovitamine parim teenuse taseme/jõudluse taseme kombinatsiooni põhjal andmebaasi tegelike tulemuste ja funktsioon vajadustest. Enne sätted klõpsake **hinnakirjad taseme (skaala DTUs)** hinnad taseme soovitused kõik veebi ja andmebaasid.



## <a name="next-steps"></a>Järgmised sammud

Kindla andmebaasi üksikasju, sõltuvalt läbimiseks on versiooniuuendus või allavahetamise tavaliselt ei juhtu hetkega. Portaal annab teatised andmebaasi siire uue taseme või täiendamise oleku saate jälgida päringu SQL-andmebaasi serveri põhi andmebaasi vaate [sys.dm_operation_status (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/dn270022.aspx) .


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
