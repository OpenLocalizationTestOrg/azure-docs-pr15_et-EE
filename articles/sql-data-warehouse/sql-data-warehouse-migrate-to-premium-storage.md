<properties
   pageTitle="Migreerimine oma olemasoleva Azure SQL-i andmebaas premium mälu | Microsoft Azure'i"
   description="Juhised migreerimine olemasoleva SQL-i andmebaas premium salvestusruum"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Migreerimise Premium salvestusruumi üksikasjad
SQL-i andmebaas hiljuti kasutusele [Suuremat jõudlust prognoositavust Premium salvestusruum][].  Meil on nüüd valmis migreerimine Premium salvestusruumi olemasolevate andmete ladu praegu Standard säilitamise kohta.  Kuidas ja millal automaatse migratsioon tekkida ja kuidas saate migreerida omas, kui soovite määrata selle tööseisakute ilmnemisel kohta lisateabe saamiseks lugege edasi.

Kui teil on rohkem kui üks andmebaas, [Automaatne migreerimise ajakava][] allpool abil määrata, kui ka migreeritakse.

## <a name="determine-storage-type"></a>Salvestusruumi tüübi määratlemine
Kui olete loonud DW allpool kuupäevi, kasutate praegu kindlad.  Standard salvestusruumi, et automaatne migreerimise iga andmebaas on andmebaas tera ülaosas teade [Azure portaali][] , mis ütleb, "on eelseisvad*versioonile premium mälu nõuab mõne katkestuste.  Lugege lisateavet ->*. "

| **Piirkond**          | **Enne seda kuupäeva DW**   |
| :------------------ | :-------------------------------- |
| Austraalia Ida      | Premium salvestusruumi pole veel saadaval |
| Austraalia kodutee | 5 August 2016                    |
| Brasiilia Lõuna        | 5 August 2016                    |
| Kanada Central      | 25 mai 2016                      |
| Kanada Ida         | 26 mai 2016                      |
| Kesk-USA          | 26 mai 2016                      |
| Hiina Ida.          | Premium salvestusruumi pole veel saadaval |
| Hiina Põhja         | Premium salvestusruumi pole veel saadaval |
| Ida-Aasia           | 25 mai 2016                      |
| Ida-USA             | 26 mai 2016                      |
| Ida-US2            | 27 mai 2016                      |
| India Central       | 27 mai 2016                      |
| India, Lõuna         | 26 mai 2016                      |
| India Lääne          | Premium salvestusruumi pole veel saadaval |
| Jaapan Ida          | 5 August 2016                    |
| Jaapan Lääne          | Premium salvestusruumi pole veel saadaval |
| Põhja Kesk-USA    | Premium salvestusruumi pole veel saadaval |
| Põhja-Euroopa        | 5 August 2016                    |
| Lõuna-, Kesk-USA    | 27 mai 2016                      |
| Kagu-Aasia      | 24 mai 2016                      |
| Lääne Euroopa         | 25 mai 2016                      |
| Lääne Kesk-USA     | 26 August 2016                   |
| Lääne USA.             | 26 mai 2016                      |
| Lääne US2            | 26 August 2016                   |

## <a name="automatic-migration-details"></a>Automaatse migreerimise üksikasjad
Vaikimisi meil kuvatakse andmebaasi migreerida teile 18.00-6 kohta teie regioonis kohaliku aja jooksul [automaatse migreerimise ajakava][] all.  Olemasoleva teie andmebaas on kasutuskõlbmatuks üleviimise ajal.  Meie hinnangul, et migreerimise võtab umbes üks tund TB salvestusruumi kohta andmebaas.  Me tagab ka, et te ei lisandu mis tahes osa automaatse üleviimise ajal.

> [AZURE.NOTE] Te ei saa kasutada oma olemasoleva andmebaas üleviimise ajal.  Pärast migreerimise lõpuleviimist, teie andmebaas on võrguühenduse taastamisel.

Allpool olevad andmed on juhised, et Microsoft võtab enda nimel migreerimise lõpule viimiseks, mis ei nõua mis tahes kaasamine oma osa.  Selles näites oletage, et teie olemasoleva DW Standard Storage praegu nimega "MyDW."

1.  Microsoft nimetab "MyDW" kuni "MyDW_DO_NOT_USE_ [ajatempli]"
2.  Microsoft peatab "MyDW_DO_NOT_USE_ [ajatempli]."  Selle aja jooksul on tehtud varukoopia.  Kui meil tekib probleeme selle käigus võidakse kuvada mitu paus/elulookirjeldused.
3.  Microsoft loob uue DW nimega "MyDW" Premium Storage toimingus 2 tehtud varukoopia.  "MyDW" ei kuvata, kuni kui taastamine on lõpule jõudnud.
4.  Kui taastamine on lõpule viidud, tagastab sama DWUs ja peatatud või aktiivne olekusse enne migreerimise "MyDW".
5.  Pärast migreerimise lõpuleviimist, kustutab Microsoft "MyDW_DO_NOT_USE_ [ajatempli]"
    
> [AZURE.NOTE] Nende sätete ei saa üle kanda migreerimise osana:
> 
>   -  Auditi andmebaasi tasemel peab olema lubatud uuesti
>   -  Tulemüüri reeglid tasemel **andmebaasi** peate olema readded.  Tulemüüri reeglid tasemel **Server** pole mõjutab.

### <a name="automatic-migration-schedule"></a>Automaatse migreerimise ajastamine
Automaatse migratsioon tekkida: 6 pm – 6 kohta (kohaliku aja järgi müüginäitajaid piirkonna kohta) järgmised katkestuste ajakava.

| **Piirkond**          | **Hinnanguline alguskuupäev**     | **Eeldatav lõppkuupäev**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Austraalia Ida      | Veel määramata           | Veel määramata           |
| Austraalia kodutee | 10 August 2016              | 24 August 2016              |
| Brasiilia Lõuna        | 10 August 2016              | 24 August 2016              |
| Kanada Central      | 23 juuni 2016                | 1 juuli 2016                 |
| Kanada Ida         | 23 juuni 2016                | 1 juuli 2016                 |
| Kesk-USA          | 23 juuni 2016                | 4 juuli 2016                 |
| Hiina Ida.          | Veel määramata           | Veel määramata           |
| Hiina Põhja         | Veel määramata           | Veel määramata           |
| Ida-Aasia           | 23 juuni 2016                | 1 juuli 2016                 |
| Ida-USA             | 23 juuni 2016                | 11 juuli 2016                |
| Ida-US2            | 23 juuni 2016                | 8 juuli 2016                 |
| India Central       | 23 juuni 2016                | 1 juuli 2016                 |
| India, Lõuna         | 23 juuni 2016                | 1 juuli 2016                 |
| India Lääne          | Veel määramata           | Veel määramata           |
| Jaapan Ida          | 10 August 2016              | 24 August 2016              |
| Jaapan Lääne          | Veel määramata           | Veel määramata           |
| Põhja Kesk-USA    | Veel määramata           | Veel määramata           |
| Põhja-Euroopa        | 10 August 2016              | 31 August 2016              |
| Lõuna-, Kesk-USA    | 23 juuni 2016                | 2 juuli 2016                 |
| Kagu-Aasia      | 23 juuni 2016                | 1 juuli 2016                 |
| Lääne Euroopa         | 23 juuni 2016                | 8 juuli 2016                 |
| Lääne Kesk-USA     | 14 August 2016              | 31 August 2016              |
| Lääne USA.             | 23 juuni 2016                | 7 juuli 2016                 |
| Lääne US2            | 14 August 2016              | 31 August 2016              |

## <a name="self-migration-to-premium-storage"></a>Ise migration, et Premium salvestusruum
Kui soovite määrata, millal teie tööseisakute toimub, saate järgmiste juhiste migreerimine olemasoleva andmebaas, klõpsake kindlad Premium mälu.  Kui valite omas migreerida, peate täitma migreerimise ise enne automaatse migreerimise algab selle piirkonna automaatse migreerimise põhjustada konflikti vältimiseks (vt [automaatse migreerimise ajakava][]).

### <a name="self-migration-instructions"></a>Ise migreerimise juhised
Kui soovite kontrollida oma tööseisakute, saate migreerida teie andmebaas omas varundus ja taaste abil.  Migreerimise taastamine osa oodatakse TB salvestusruumi DW umbes üks tund.  Kui te ei soovi säilitada sama nime pärast migreerimise lõpuleviimist, järgige [ümbernimetamine migreerimisel][]. 

1.  [Paus][] oma DW, mis võtab Automaatne varundamine
2.  Oma viimase hetktõmmise põhjal [taastamine][]
3.  Kustutage oma olemasoleva DW Standard säilitamise kohta. **Kui te ei tee seda toimingut, mida tuleb tasuda nii DWs jaoks.**

> [AZURE.NOTE] Nende sätete ei saa üle kanda migreerimise osana:
> 
>   -  Auditi andmebaasi tasemel peab olema lubatud uuesti
>   -  Tulemüüri reeglid tasemel **andmebaasi** peate olema readded.  Tulemüüri reeglid tasemel **Server** pole mõjutab.

#### <a name="optional-steps-to-rename-during-migration"></a>Valikuline: juhiseid migreerimisel ümbernimetamine 
Kaks andmebaasi samas loogiline serveris ei tohi olla sama nimi. SQL-i andmebaas toetab nüüd võimalus DW ümber nimetada.

Selles näites oletage, et teie olemasoleva DW Standard Storage praegu nimega "MyDW."

1.  Ümber nimetada "MyDW" abil käsu Muuda andmebaasi, et midagi toimingutega nagu "MyDW_BeforeMigration."  See käsk tapab kõik olemasolevad tehingud ja tuleb teha õnnestub juhtslaidi andmebaasis.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Paus][] "MyDW_BeforeMigration", mis võtab Automaatne varundamine
3.  [Taastada][] Viimati tehtud hetktõmmise uue andmebaasi nime on kasutatud (ex: "MyDW")
4.  Kustuta "MyDW_BeforeMigration".  **Kui te ei tee seda toimingut, mida tuleb tasuda nii DWs jaoks.**

> [AZURE.NOTE] Nende sätete ei saa üle kanda migreerimise osana:
> 
>   -  Auditi andmebaasi tasemel peab olema lubatud uuesti
>   -  Tulemüüri reeglid tasemel **andmebaasi** peate olema readded.  Tulemüüri reeglid tasemel **Server** pole mõjutab.

## <a name="next-steps"></a>Järgmised sammud
Muudatuse Premium salvestusruumi, kus on me suurendada bloobimälu andmebaasifailide arhitektuur teie andmebaas on arv.  Selle muudatuse eelised jõudluse maksimeerimiseks soovitame vahetu oma rühmitatud Columnstore indeksid järgmise skripti abil.  Allpool skripti toimib sunnib mõningaid olemasolevaid andmeid soovite täiendavaid plekid.  Kui teil ära tee midagi, andmed kuvatakse loomulikult ümber aja jooksul nagu suurema hulga andmete laadimine teie andmebaas tabelitesse.

**Eeltingimused:**

1.  Andmebaas peaks töötama 1000 DWUs või uuem versioon (vt [skaala Arvuta power][])
2.  Käivitamisel skript kasutaja peaks olema [mediumrc rolli][] või uuem versioon
    1.  Kasutaja lisamine selle rolli käivitada järgmist: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Kui teil tekib probleeme oma andmebaas, [luua tugi Piletite][] ja viide "Migreerimise Premium Storage" võimalik põhjus.

<!--Image references-->

<!--Article references-->
[automaatse migreerimise ajastamine]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[tugiteenuste Piletite loomine]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Paus]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Taastamine]: sql-data-warehouse-restore-database-portal.md
[migreerimisel ümbernimetamine]: #optional-steps-to-rename-during-migration
[skaala Arvuta power]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc roll]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Suurem jõudlus prognoositavuse Premium salvestusruum]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure'i portaal]: https://portal.azure.com
