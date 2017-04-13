<properties
    pageTitle="-Mälu OLTP parandab SQL-i txn täiuslik | Microsoft Azure'i"
    description="Vastu-mälu OLTP olemasoleva SQL-andmebaasis selgituseks jõudluse parandamiseks."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Kasutage-mälu OLTP (eelvaade) SQL-andmebaasis rakenduse jõudluse parandamiseks

[-Mälu OLTP](sql-database-in-memory.md) saab ilma tulemuslikkuse OLTP töökoormus [Premium](sql-database-service-tiers.md) Azure SQL-i andmebaaside jõudluse parandamiseks.

Järgmiste juhiste abil vastu-mälu OLTP olemasoleva andmebaasi.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Samm 1: Veenduge, et andmebaasi Premium toetab-mälu OLTP

Loodud November 2015 või uuem versioon Premium andmebaaside toeta In Memory funktsiooni. Saate kontrollida, kas andmebaasi Premium toetab In Memory funktsiooni, käitades järgmine Transact-SQL-lause. -Mälu on toetatud, kui Tagastatud tulem on 1 (mitte 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* tähistab *Äärmiselt tehingu töötlemine*

Kui uue andmebaasi V12 Premium peate olemasoleva andmebaasi teisaldada, saate eksportida ja seejärel andmete importimine järgnevalt.

#### <a name="export-steps"></a>Ekspordi järgmiselt.

Andmebaasi tootmise eksportimiseks soovitud bacpac, kasutades ühte:

- [Ekspordi](sql-database-export.md) funktsionaalsus [portaalis](https://portal.azure.com/).

- **Ekspordi rakendus** funktsiooni [ajakohane SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. **Objekti Exploreri**, laiendage **andmebaasid** .
 2. Paremklõpsake oma andmebaasi sõlm.
 3. Klõpsake nuppu **Tööülesanded** > **eksportimine rakendus**.
 4. Viisardi aken, mis kuvatakse töötada.


#### <a name="import-steps"></a>Imporditoimingud

Uue andmebaasi Premium on bacpac importida.

1. Azure'i [portaal](https://portal.azure.com/)
 - Liikuge server.
 - Valige suvand [Impordi andmebaasi](sql-database-import.md) .
 - Valige hinnad taseme lisatasu.

2. SSMS abil soovitud bacpac importida.
 - **Objekti Explorer**, Paremklõpsake sõlme **andmebaasid** .
 - Klõpsake nuppu **impordi rakendus**.
 - Viisardi aken, mis kuvatakse töötada.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Samm 2: Määratle objektide-mälu OLTP migreerimine

SSMS sisaldab **Tehingu jõudluse analüüs ülevaade** aruande, mis on aktiivne töökoormus andmebaasi suhtes saate käivitada. Tabelite ja salvestatud toimingute migration, et-mälu OLTP kuuluvaid aruandega.

SSMS aruande loomiseks:
- **Objekti Explorer**, paremklõpsake oma andmebaasi sõlm.
- Klõpsake **aruannete** > **Standard Reports** > **tehingu jõudluse analüüs ülevaade**.

Lisateavet leiate teemast [Determining kui tabeli või salvestatud protseduur peaks teisaldatud-mälu OLTP](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Samm 3: Võrdlev testi andmebaasi loomine

Oletame, et aruanne näitab andmebaasi on kasu teisendatakse mälu optimeeritud tabeli tabel. Soovitame teil esmalt testida kinnitamiseks märkimisel testida.

Peate andmebaasi tootmise testi koopia. Andmebaasi test peab olema sama teenuse taseme tasemel tootmise andmebaasi.

Et hõlbustada testimine, kohandada efektisuvandite andmebaasi testi järgmiselt:

1. Andmebaasiga ühenduse loomiseks testi SSMS abil.

2. Päringute abil (HETKTÕMMISE) suvand vajavad vältimiseks määrake sätte andmebaasi nagu on näidatud järgmises T-SQL-lause:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Samm 4: Migreerimine tabelid

Peate looma ja asustada mälu optimeeritud koopia tabel, mida soovite testida. Saate selle luua, kasutades kas:

- Mugav mälu optimeerimine viisardi SSMS.
- Käsitsi T-SQL-i.


#### <a name="memory-optimization-wizard-in-ssms"></a>Mälu optimeerimine viisardi SSMS

Migreerimise selle suvandi kasutamiseks tehke järgmist.

1. Andmebaasiga ühenduse loomiseks testi SSMS abil.

2. **Objekti Explorer**, Paremklõpsake tabelit ja klõpsake **Mälu optimeerimine Advisor**.
 - Kuvatakse **Tabeli Mälu optimeerija Advisor** viisard.

3. Klõpsake viisardis **migreerimise valideerimine** (või nuppu **Järgmine** ) kuvamiseks, kui tabelil on Mittetoetatavaid funktsioone, mida ei toetata mälu optimeeritud tabelid. Lisateavet leiate teemast:
 - *Mälu optimeerimine kontroll* [Mälu optimeerimine Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Kooste transact-SQL-mälu OLTP ei toeta](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migreerimine - mälu OLTP](http://msdn.microsoft.com/library/dn247639.aspx).

4. Kui tabel on pole Mittetoetatavaid funktsioone, saab nõustaja teha tegelik skeemi ja andmete migreerimise jaoks.


#### <a name="manual-t-sql"></a>Käsitsi T-SQL-is

Migreerimise selle suvandi kasutamiseks tehke järgmist.

1. Ühenduse test andmebaasi SSMS (või sarnane kasuliku) abil.

2. Hankige täielik T-SQL-skripti tabeli ja selle registrid.
 - SSMS, paremklõpsake oma tabeli sõlm.
 - Klõpsake **tabeli skripti** > **luua** > **päringu uues aknas**.

3. Klõpsake aknas skripti lisada asendaja (MEMORY_OPTIMIZED = ON) tabeli loomine lausesse.

4. Kui KLASTER register, NONCLUSTERED seda muuta.

5. Nimetage ümber SP_RENAME olemasolevasse tabelisse.

6. Luua uue eksemplari mälu optimeeritud tabeli käitamist redigeeritud tabeli loomine.

7. Kopeerige andmed mälu optimeeritud tabeli lisamine abil... VALIGE * SISSE:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Juhis 5 (valikuline): Salvestatud toimingute migreerimine

Funktsiooni In Memory ka muuta salvestatud toimingu jaoks paranenud jõudlus.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Kaalutlused üldjuhul kompileeritud salvestatud toimingute abil

Üldjuhul kompileeritud salvestatud protseduur peab olema T-SQL-i abil klausel järgmised suvandid:

- NATIVE_COMPILATION

- SCHEMABINDING: See tähendab tabelid, mis on salvestatud protseduur ei tohi olla nende veergude määratlusi muuta mis tahes viisil, mis võivad mõjutada salvestatud protseduur, kui soovite pukseerida salvestatud protseduur.


Kohalikke mooduli peate kasutama ühe suur [ATOMIC plokid](http://msdn.microsoft.com/library/dn452281.aspx) tehingu haldamiseks. Ei ole rolli konkreetsete alustada tehingu või tehingu TAGASIPÖÖRAMINE. Kui teie kood tuvastab business reegli rikkumise, see võib lõpetada [põhjustada](http://msdn.microsoft.com/library/ee677615.aspx) lause atomic Aadressiplokk.


### <a name="typical-create-procedure-for-natively-compiled"></a>Tüüpilised loomine protseduur algupäraselt koostatud

Tavaliselt T-SQL algupäraselt koostatud salvestatud protseduuri loomine sarnaneb järgmist malli.

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- TRANSACTION_ISOLATION_LEVEL, on kõige enam esineva väärtuse jaoks üldjuhul kompileeritud salvestatud protseduur HETKTÕMMIS. Muude väärtuste Alamhulk on samuti toetatud:
 - KORRATAVAD READ
 - SARJADESSE JAOTATAV


- KEELE väärtus peab olema praeguse vaate sys.languages.


### <a name="how-to-migrate-a-stored-procedure"></a>Kuidas migreerida salvestatud protseduur

Migreerimise toimingud on.


1. Hankige protseduuri luua skripti tavaline tõlgendada salvestatud protseduur.

2. Kirjutada oma päise eelmisele malli vastavaks.

3. Kindlaks teha, kas salvestatud protseduur T-SQL-koodi kasutab kõik funktsioonid, mis ei toeta algupäraselt koostatud salvestatud toimingute jaoks. Vajadusel, rakendada lahendusi.
 - Üksikasju vt [Algupäraselt koostatud salvestatud toimingute migreerimise probleemid](http://msdn.microsoft.com/library/dn296678.aspx).

4. Nimetage vana salvestatud protseduur SP_RENAME abil. Või lihtsalt KUKUTAGE see.

5. Käivitage oma redigeeritud protseduuri loomine T-SQL-i skripti.


## <a name="step-6-run-your-workload-in-test"></a>Samm 6: Käivitada oma töökoormus test

Käivitage test andmebaasi, mis sarnaneb töökoormus, mis käivitatakse andmebaasi tootmise on töökoormus. See jõudluse kasu saavutada tabelite ja salvestatud toimingute In Memory funktsiooni kasutamisel tuleks esile.

Peamised atribuutide töökoormus on:

- Samaaegseid ühenduste arv.

- Lugemis-suhte.


Oluliste ja käivitage test töökoormus, kaaluge mugav ostress.exe tööriista, mis on näidatud [siin](sql-database-in-memory.md).


Võrgu latentsuse minimeerimiseks käivitada oma katse Azure geograafilised piirkonna kui andmebaas on olemas.


## <a name="step-7-post-implementation-monitoring"></a>Juhis 7: Pärast rakendamise jälgimise

Võtke arvesse, et teie-mälu rakendusi valmistamisel jõudluse mõju kontrollimisel.

- [Kuvari - mälu salvestusruumi](sql-database-in-memory-oltp-monitoring.md).

- [Azure'i SQL-andmebaasi dünaamilise halduse vaadete abil jälgimine](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Seotud lingid

- [Mälu OLTP (-mälu optimeerimine)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Üldjuhul kompileeritud salvestatud toimingute tutvustus](http://msdn.microsoft.com/library/dn133184.aspx)

- [Advisor mälu optimeerimine](http://msdn.microsoft.com/library/dn284308.aspx)

