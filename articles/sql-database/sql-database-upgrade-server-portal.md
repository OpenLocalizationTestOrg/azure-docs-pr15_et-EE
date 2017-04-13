<properties
    pageTitle="Azure'i portaalis Azure SQL-i andmebaasi V12 Uuenda | Microsoft Azure'i"
    description="Selgitab, kuidas võtta kasutusele Azure SQL-i andmebaasi V12 sh kuidas veebi ja andmebaaside versiooni uuendamine ja kuidas uuendada oma andmebaase otse Azure'i portaalis kohapeal on elastne andmebaasi migreerimine V11 server."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Azure'i portaalis Azure SQL-i andmebaasi V12 versioonile


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-upgrade-server-portal.md)
- [PowerShelli](sql-database-upgrade-server-powershell.md)


SQL-i andmebaasi V12 on olemasolev serverid versiooniks SQL-i andmebaasi V12 on soovitatav uusim versioon.
SQL-i andmebaasi V12 on palju [eelmise versiooni eeliseid](sql-database-v12-whats-new.md) , sh:

- SQL serveri täiustab ühilduvust.
- Täiustatud premium jõudlus ja jõudluse uuele tasemele.
- [Elastne andmebaasi kaustu](sql-database-elastic-pool.md).

Selles artiklis antakse juhiseid olemasoleva SQL-i andmebaasi V11 serverid ja andmebaaside uuendamise SQL-i andmebaasi v12.

Ajal ülemineku V12 on täiendate mis tahes veebi ja andmebaaside uue teenuse kiht nii, et juhised veebi ja andmebaaside uuendamise kaasatakse.

Lisaks on [pool elastne andmebaasi](sql-database-elastic-pool.md) migreerimine võib olla kulude tõhusam kui üleminekut üksikute jõudluse tasemeid (hinnad astme) ühe andmebaaside jaoks. Kaustu lihtsustada ka andmebaasi haldamine, sest teil on vaja vaid pool, mitte eraldi jõudluse tasemeid üksikute andmebaaside haldamine jõudluse sätete haldamine. Kui teil on mitu serverites andmebaaside kaaluge liiguvad sama server ja laskmiseks on ära. Saate hõlpsasti [Automaatne migreerimine andmebaaside V11 serveritest otse kaustadesse elastne andmebaasi PowerShelli kaudu](sql-database-upgrade-server-powershell.md). Samuti saate portaali V11 andmebaasi migreerimine on, kuid portaalis peab olema juba V12 server on loomiseks. Juhised on esitatud selle artikli loomiseks pool pärast täiendamist serveri, kui teil on [andmebaase, mis saavad kaustast](sql-database-elastic-pool-guidance.md).

Pange tähele, et teie andmebaasid on kogu aeg olema võrgus versioonitäienduse kogu tööd jätkata. Uus ajutine pukseerimine ühenduste andmebaasi jõudlusega tegeliku ülemineku ajal võib olla väga väike kestus, mis on tavaliselt umbes 90 sekundit, kuid võib olla nii palju kui 5 minutit. Kui teie taotlus on [ajutine viga käsitsemise ühenduse klemmliidesed](sql-database-connectivity-issues.md) siis piisab versioonitäienduse lõpus ühenduse katkestuste eest kaitsta.

SQL-i andmebaasi v12 üleminekut ei saa tagasi võtta. Pärast täiendamist ei saa serveris taastatud V11 abil.

Pärast V12, [teenuse taseme soovitused](sql-database-service-tier-advisor.md) ja [elastne rakenduskausta jõudluse huvides](sql-database-elastic-pool-guidance.md) kohe on kättesaadavad seni, kuni teenust on aeg hindamaks, kui teie töökoormus uues serveris. V11 serveri soovitus ajalugu ei kehti V12 serverid nii, et seda ei säilitata.

## <a name="prepare-to-upgrade"></a>Versioonitäienduse ettevalmistamiseks

- **Kõik veebi ja andmebaaside versiooni uuendamine**: [Kõik veebi ja andmebaaside versiooni uuendamine](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) jaotise all või vaadake teemast [jälgimine ja haldamine elastne andmebaasi kohapeal (PowerShelli) on](sql-database-elastic-pool-manage-powershell.md).
- **Läbivaatus ja peatada Geo-Dispersioonanalüüs**: kui SQL Azure'i andmebaasi on konfigureeritud lubama Geo-Dispersioonanalüüs teie dokument oma praeguse konfiguratsiooni ja [Geo-Dispersioonanalüüs lõpetada](sql-database-geo-replication-portal.md#remove-secondary-database). Pärast versioonitäienduse lõpulejõudmist taaskonfigureerimine Geo-kopeerimise andmebaasi.
- **Avage järgmised pordid, kui teil on mõne Azure'i VM kliendid**: kui teie klient programmi loob ühenduse SQL-i andmebaasi V12 ajal Azure virtuaalse masina (VM) oma kliendi töötab, peate avama pordi vahemike 11000-11999 ja 14000-14999 VM. Lisateavet leiate teemast [V12 SQL-i andmebaasi Porte](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Versioonitäienduse käivitamine

1. [Azure'i portaalis](https://portal.azure.com/) Sirvi server soovite uuendada, valides **SIRVIDA** > **SQL-i serverid**ja valides v2.0 server, mida soovite täiendada.
2. Valige **SQL-i andmebaasi värskenduses**, valige **selle serveri versiooni täiendamine**.

      ![Serveri versiooni täiendamine][1]

3. Serveri versiooniks SQL-i andmebaasi värskenduses on püsiv ja pöördumatu. Kinnitage uuele versioonile, tippige oma serveri nimi ja klõpsake nuppu **OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Kõik veebi ja andmebaaside versiooni uuendamine

Kui teie server on mis tahes veebi- ja andmebaaside peate täiendama need. SQL-i andmebaasi v12 täiendamise käigus ei värskendada kõik veebi ja andmebaaside uue teenuse kiht.    

SQL-andmebaasi teenuse soovitab aidata teil üleminekut, on asjakohane taseme- ja jõudluse tase (hinnad taseme) iga andmebaasi. Teenuse soovitab parima taseme töötab olemasoleva andmebaasi töökoormus analüüsib ajalooliste kasutus andmebaasi.

3. Valige **selle serveri versiooni täiendamine** tera iga andmebaasi üle vaadata ja valige soovitatavad hinnad taseme uuendada. Saate alati saadaval hinnakirjad astme Sirvi ja valige üks, mis sobib kõige paremini teie keskkonnas.


     ![andmebaasid][2]


7. Pärast nupu soovitatud taseme esitatakse, kus saate valida mõne teise astme ja klõpsake selle taseme muutmiseks **Valige** nupp **Vali oma hinnakirjad taseme** tera. Valige uue taseme iga veebi- ja andmebaasi.

    Pange tähele, et oluline on see, et SQL-andmebaasi pole lukus mis tahes kindla teenuse taseme või jõudluse tasemele nõuetele oma andmebaasi muutmine ning saate hõlpsalt muuta jõudluse tasemed on saadaval teenuste astme vahel. Tegelikult Basic, Standard ja Premium SQL-i andmebaasid on arve alusel ja teil on võimalus mastaapimiseks iga andmebaasi üles või alla 4 korda 24 tunni jooksul.

    ![soovitused][6]


Kui kõik andmebaasid serveris on õigus olete valmis versioonitäienduse käivitamine

## <a name="confirm-the-upgrade"></a>Kinnitage uuele versioonile

3. Kui kõik andmebaasid serveris on õigus saada versioonile versiooniuuendus peate **Serveri nimi Tippige** kinnitamaks, et soovite versioonitäienduseks ja seejärel klõpsake nuppu **OK**.

    ![Veenduge, et täiendamine][3]


4. Uuele versioonile käivitab ja kuvab tekstivormingus edenemist teatis. Käivitatakse versiooniuuendus. Sõltuvalt üksikasjad kindla andmebaasi täiendamine versiooniks V12 võib võtta aega. Sel ajal jäävad kõik andmebaasid serveris võrgus, kuid server ja andmebaas haldamise toiminguid tuleb piirata.

    ![täiendamine on pooleli][4]

    Uue jõudlus tegeliku ülemineku ajal taseme ajutine pukseerimine ühenduste andmebaasi võib juhtuda väga väike kestus (tavaliselt mõõdetuna sekundites). Kui rakendus on ajutine viga töötlemine (proovi uuesti loogika) ühenduse klemmliidesed, siis piisab versioonitäienduse lõpus ühenduse katkestuste eest kaitsta.

5. Pärast selle toimingu versioonitäienduse lõpulejõudmist kuvatakse **Värskenduses** tera **lubatud**.

    ![V12 lubatud][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Elastne andmebaasi kohapeal on oma andmebaasid viimine

[Azure'i portaalis](https://portal.azure.com/) sirvige V12 server ja klõpsake nuppu **Lisa kausta**.

- või -

Kui kuvatakse teade selle kohta, **klõpsake siin, et kuvada selle serveri soovitatav elastne andmebaasi kaustu**, klõpsake seda hõlpsasti luua kausta, mis on optimeeritud oma serveri andmebaasidega. Lisateavet leiate teemast [hinna ja jõudluse kaalutluste kohta kohapeal on elastne andmebaasi](sql-database-elastic-pool-guidance.md).

![Kausta lisamine server][7]

[Create kohapeal elastne andmebaas on](sql-database-elastic-pool.md) artikli oma rakenduskausta loomise lõpuleviimiseks järgige.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Pärast üleminekut versioonile SQL-i andmebaasi V12 andmebaaside jälgimine

>[AZURE.IMPORTANT] Täiendada uusimaks versiooniks, SQL Server Management Studio (SSMS) v12 uute võimaluste ärakasutamiseks. [Alla laadida SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx).

Pärast üleminekut, jälgivad rakendused töötavad soovitud jõudluse tagamiseks andmebaas ja seejärel optimeerimine sätteid vastavalt vajadusele.

Lisaks jälgimise üksikute andmebaase, saate jälgida elastne andmebaasi kaustu [kuvaris, hallata, ja suurus soovitud elastne andmebaasi rakenduskausta Azure'i portaalis](sql-database-elastic-pool-manage-portal.md) või [PowerShelli abil](sql-database-elastic-pool-manage-powershell.md).


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




**Teatiste:** Häälestada 'Teatiste' Azure'i portaalis märku DTU tarbimine täiendatud andmebaasi lähenedes teatud kõrge. Andmebaasi teatiste saab setup erinevate jõudluse mõõdikute DTU, CPU, IO ja samamoodi nagu Azure'i portaalis. Otsige sirvides üles oma andmebaasi ja valige **sätted** tera **teatiste reeglid** .

Näiteks saate häälestada meiliteatise klõpsake "DTU protsent" kui keskmine DTU protsent väärtus on suurem kui 75% viimase viie minuti jooksul. Vaadake lisateavet selle kohta, kuidas konfigureerida teatiste [teatiste](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vastuvõtmine.





## <a name="next-steps"></a>Järgmised sammud

- [Otsi soovitused ühendada ning koostada andmebaas](sql-database-elastic-pool-create-portal.md).
- Saate [muuta oma andmebaasi teenuse taseme- ja jõudluse](sql-database-scale-up.md).



## <a name="related-links"></a>Seotud lingid

- [Mis on uut rakenduses SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Plaanimine ja võtta kasutusele SQL-i andmebaasi V12 ettevalmistamine](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
