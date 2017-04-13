<properties
   pageTitle="SQL-i andmebaas tehingute | Microsoft Azure'i"
   description="Näpunäiteid tehingud arendamise lahenduste rakendamisel SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Tehingute SQL-andmebaas

Kui ootate, toetab SQL-i andmebaas tehingud andmete ladu töökoormus osana. Siiski tagada jõudlus SQL-i andmebaas hoitakse skaala mõned funktsioonid on piiratud, SQL serveri võrdlemisel. Selles artiklis tõstab esile erinevused ja teised on loetletud. 

## <a name="transaction-isolation-levels"></a>Tehingu eraldamise tasemed
SQL-i andmebaas rakendab HAPPE tehingud. Siiski eraldamine selgituseks tugi on piiratud `READ UNCOMMITTED` ja seda ei saa muuta. Saate rakendada mitmesuguseid kodeerimise meetodid määrdunud loeb andmeid, kui see on teie jaoks oluline. Viisid kasutada CTAS ja tabeli partition üle (sageli nimetatakse libistades akna mustri), saate takistada kasutajatel päringute andmeid, mis on endiselt valmis. Populaarsed lähenemine on ka eelnevalt andmete filtreerimiseks vaated.  

## <a name="transaction-size"></a>Tehingu suurus
Ühe andmete muutmist tehingu maht on piiratud. Täna rakendatakse limiit "jaotuse" kohta. Seetõttu saab arvutada kokku eraldatud limiit korrutatakse jaotuse arvu. Kui soovite ligikaudse tehingu ridade maksimumarv jagatakse jaotuse ots suuruse iga rea kohta. Eri pikkusega võtke arvesse võtta mõne veeru keskmine pikkus, mitte maksimummaht abil.

Järgmises tabelis järgmised oletused on tehtud:

* Ilmnenud on mõni isegi andmete jaotumine 
* Keskmine rea pikkus on 250 baiti

| [DWU][]    | Piiramise kohta jaotuse (GiB) | Arvu üldiste müügijaotustega tutvumine | MAX tehingu suurus (GiB) | # Ridade levitamise kohta. | Max ridade kande kohta |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   90                       |   6 000 000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  nimiväärtuses 12 000 000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11.25                      | 60                      |  675                       |  45.000.000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60,000,000             |  3,600,000,000           |
| DW3000 | 22,5                       | 60                      |  1350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2700                     | 180,000,000             | 10,800,000,000           |

Tehingu mahupiirangu rakendatakse või toimingu kohta. See on rakendatud kõik Samaaegne tehingud üle. Seetõttu on lubatud iga tehingu kirjutamise Logi selle suurt hulka andmeid. 

Optimeerida ja minimaalseks Logi kirjutatud andmed palun vaadake artiklis [tehingud head tavad][] .

> [AZURE.WARNING] Maksimaalse mahu on võimalik ainult saavutada räsi või ROUND_ROBIN jaotatud tabelid, mille andmed on paarisarv. Kui tehing kirjutamise andmete moonutatud mood selle jaotuse on tõenäoliselt jõutakse enne maksimaalse mahu limiit.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Tehingu olek
SQL-i andmebaas kasutab funktsiooni XACT_STATE() teatada nurjunud tehingu väärtuse -2 kasutamine. See tähendab, et tehingu on nurjunud ja tagasipööramine ainult märgitud

> [AZURE.NOTE] Kasutus -2 XACT_STATE funktsiooni nurjunud tehingu tähistamiseks tähistab erinevat käitumist SQL serveriga. SQL Server kasutab tähistada un committable toimingu väärtuse -1. SQL serveri saate ilma vajaduseta märgita nii Un committable luba vigu tehingu sees. Näiteks `SELECT 1/0` põhjustada tõrke, kuid mitte jõustada tehingu mõne un committable olekusse. SQL serveri võimaldab loeb ka un committable tehingu. Siiski SQL-i andmebaas ei luba teil seda teha; Kui ilmneb tõrge SQL-i andmebaas tehingu sees sisestab automaatselt riigi-2 ja te ei saa teha, mis tahes täiendavaid valige laused, kuni see lause tagasi pöörata. Seetõttu on oluline, et kontrollida, et oma rakenduse koodi kuvamiseks, kui seda kasutatakse XACT_STATE() nagu vajalikud muudatused teha.

Näiteks võidakse kuvada SQL serveri tehingu, mis näeb välja umbes järgmine:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Kui jätate oma kood, kui see on eespool, siis kuvatakse järgmine tõrketeade:

Sõnum 111233, tase 16 olek 1 rida 1 111233; Praeguse tehing katkestati ja ootel muudatusi, on tagasi võetud. Põhjus: Tehingu tagasipööramine ainult olekus oli pole selgesõnaliselt alla enne DDL, piirmäära või SELECT-lause.

Saate ka ei saa väljund ERROR_ * funktsioonid.

Rakenduses SQL-i andmebaas on vaja veidi muuta kood:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Oodatud käitumine on nüüd järgida. Tehingu viga hallatakse ja funktsioonide ERROR_ * Sisestage väärtused ootuspäraselt.

Kõik, mis on muutunud on see, et selle `ROLLBACK` tehingu oli enne tõrke teave Loe soovitud `CATCH` plokk.

## <a name="errorline-function"></a>Funktsioon Error_Line()
See on märkida, et SQL-i andmebaas rakendada või toetavad ERROR_LINE() funktsiooni. Kui teil on see teie kood, peate selle vastavaks SQL-i andmebaas eemaldada. Kasutage selle asemel päringu siltide koodi samaväärset funktsiooni rakendada. Lugege artiklit [sildi][] selle funktsiooni kohta lisateabe saamiseks.

## <a name="using-throw-and-raiserror"></a>VISATA ja RAISERROR abil
On rohkem kaasaegse rakendamiseks tõstmine erandid SQL-i andmebaas, kuid RAISERROR toetatakse ka. On mõned erinevused siiski tähelepanu väärt on.

- Kasutaja määratletud arvude ei saa VISATA vahemikus 100 000 – 150 000 tõrketeated
- Veebisaidil 50,000 on fikseeritud RAISERROR tõrketeated
- Kasutus sys.messages ei toetata

## <a name="limitiations"></a>Limitiations
SQL-i andmebaas on mõned piirangud, mis on seotud tehingud.

Need on järgmised:

- Jaotatud tehinguid
- Pesastatud tehinguid lubatud
- Pole lubatud punktide salvestamine
- Nimega tehinguid
- Pole märgitud kannete
- Näiteks DDL ei toeta `CREATE TABLE` sees kasutaja määratletud tehingu

## <a name="next-steps"></a>Järgmised sammud
Optimeerimine tehingud kohta lisateabe saamiseks lugege teemat [tehingud head tavad][].  Muud SQL-i andmebaas heade tavade kohta leiate teemast [SQL-i andmebaas parimaid tavasid][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Tehingud head tavad]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL-i andmebaas head tavad]: ./sql-data-warehouse-best-practices.md
[SILDI]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
