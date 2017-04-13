<properties
    pageTitle="Azure'i SQL-andmebaasi ja jõudluse ühe andmebaaside jaoks | Microsoft Azure'i"
    description="See artikkel aitab teil otsustada, milline teenuse kiht rakenduse valimiseks. Samuti soovitab, kuidas häälestada oma rakenduse toomine kõige Azure'i SQL-andmebaasi."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure'i SQL-andmebaasi ja jõudluse ühe andmebaaside jaoks

SQL Azure'i andmebaas pakub kolme [teenuse astme](sql-database-service-tiers.md): Basic, Standard, ja. Iga teenuse kiht tingimata isolaatide ressursid, et SQL-andmebaasi abil saate ja tagab prognoositavad täitmise selle teenusetaseme. Selles artiklis pakume juhiseid, mis aitavad teil teenuse kiht rakenduse valimine. Käsitleme ka võimalust, et saate häälestada rakenduse kõige Azure SQL-i andmebaasist.

>[AZURE.NOTE] See artikkel keskendub jõudluse juhiseid ühe andmebaaside Azure SQL-andmebaasis. Juhised jõudlusega seotud elastne andmebaasi kaustu, leiate teemast [hinna ja jõudluse kaalutluste kohta elastne andmebaasi kaustu](sql-database-elastic-pool-guidance.md). Pange tähele, aga, et saate rakendada paljude häälestamise soovitused selle artikli andmebaasidega kohapeal on elastne andmebaasi ja sarnase jõudluse kasu saada.

Need on kolmest Azure'i SQL-andmebaasi teenuse tasemest, mida saate valida (taseme mõõtmiseks andmebaasi läbilaskevõime üksused või [DTUs](sql-database-what-is-a-dtu.md):

- **Tavaline**. Põhilised teenuse taseme pakutakse head jõudlust paremat teavet iga andmebaasi, tund tunni jooksul. Põhilised andmebaasis piisavalt ressursse toetavad small andmebaasi, mis ei sisalda mitme samaaegseid taotlusi head jõudlust.
- **Standardne**. Standard teenuse kiht pakub täiustatud jõudlus prognoositavuse ja tõstab andmebaasidele, mis on mitme samaaegseid taotlusi, nagu töörühm ja web rakendused. Standard teenuse taseme andmebaasi valimisel suuruse põhjal hõlpsalt prognoosida jõudluse tagamiseks andmebaasi rakenduse minute minuti jooksul.
- **Premium**. Premium teenuse kiht pakub prognoositavad jõudluse tagamiseks üle teise Teiseks iga Premium andmebaasi jaoks. Kui valite suvandi lisatasu teenuse kiht, suuruse andmebaasi rakenduse tippväärtus laadi selle andmebaasi põhjal. Leping eemaldab juhtudel, mida täitmisel dispersioon võib põhjustada small päringute võtta oodatust rohkem aega latentsus tundliku toimingutes. See mudel võib oluliselt lihtsustada arendamise ja toote valideerimise tsüklit rakendusi, mis on vaja teha tugev laused tippväärtus ressursi vajadustele, jõudluse dispersioon või päringulatentsus kohta.

Veebisaidil iga teenuse kiht, määrate jõudluse taseme, nii, et teil on ainult maksma peate võimsus paindlikkust. Saate [reguleerida võimsus](sql-database-scale-up.md), üles või alla, kui töökoormus muutmist. Näiteks kui teie andmebaasi töökoormus on suur tagasi kooli uudised jooksul, teie määratud aja jooksul, kuni mai juuli suurendada jõudlusega andmebaasi. Saate vähendada seda oma tippväärtus season lõppemisel. Saate minimeerida maksate optimeerides oma cloud keskkonnas hooajalisuse teie ettevõttest. See mudel töötab ka ka tarkvara toote väljaanne tsüklit. Testi meeskonnatöö võib võimsus eraldada, kuigi see käivitatakse testimiseks ja seejärel vabastage läbilaskevõimet, kui nad lõpetanud testimine. Võimsus taotluse mudelis maksate võimsus seda vaja, kui kulutama sihtotstarbeline ressursid, mida võib kasutada harva.

## <a name="why-service-tiers"></a>Miks teenuse astme?

Kuigi iga andmebaasi töökoormus võib erineda, teenuse astme eesmärk on jõudluse teavitamiseks jõudluse erinevatel tasanditel. Pühendunud IT-keskkond, saate töötada klientide suuremahuliste andmebaasi ressursi nõuetele.

### <a name="common-service-tier-use-cases"></a>Levinud teenuse kiht kasutamine juhtudel

#### <a name="basic"></a>Tavaline

- **Te alles alustate koos Azure'i SQL-andmebaasi**. Rakendusi, mis on sageli väljatöötamisel ei pea suure jõudlusega tasemed. Tavaline andmebaasid on optimaalne keskkonnas andmebaasi arengu, samal ajal madal hind.
- **Teil on ühe kasutaja andmebaasi**. Rakendusi, mis tavaliselt andmebaasi ühe kasutaja seostada pole kõrge kokkulangevus ja jõudluse nõuetele. Need rakendused saavad lihtsa teenuse kiht;

#### <a name="standard"></a>Standard

- **Andmebaasil on mitu samaaegseid taotlusi**. Rakendusi, mis tavaliselt korraga mitmele kasutajale teenuse vaja kõrgem jõudlus. Näiteks veebilehed, et leida keskmise liikluse või osakonna rakendusi, mis nõuavad veel ressursse on hea leviloendite puhul Standard teenuse kiht.

#### <a name="premium"></a>Premium

Premium teenuse taseme kasutamine enamasti on üks või mitu need omadused.

- **Kõrge tippväärtus laadimine**. Rakendus, mis nõuab palju CPU, mälu või sisend/väljund (I/O) ning selle jaoks on vaja pühendunud, suure jõudlusega. On teada, et kasutada mitme protsessorituuma pikemat aega andmebaasitoiming on näiteks võidakse Premium teenuse kiht.
- **Mitme samaaegseid taotlusi**. Mõned andmebaasirakendusi teenuse palju samaaegseid taotlusi, näiteks kui serveeritakse veebisait, mis on kõrge liikluse draiv. Põhi- ja teenuse astme võimalikult vähe samaaegseid taotlusi andmebaasi kohta. Rakendusi, mis nõuavad rohkem ühendusi oleks vaja reageerimine vajadusele taotlusi maksimumarv vastav broneerimiseks suuruse valimine.
- **Madal latentsus**. Mõned rakendused on vaja selleks, et tagada minimaalsete ajas vastuse andmebaasist. Kui kindla salvestatud toimingu nimi on osa laiemast kliendi toiming, peate nõue tagasi, et kõne on rohkem kui 20 millisekundites 99% ajast. Seda tüüpi rakendus kasu Premium teenuse kiht, veenduge, et nõutavad arvuti võimsus on saadaval.

SQL-andmebaasi allkirjastamiseks teenusetaseme sõltub tippväärtus laadi nõuded iga ressursi dimensiooni. Mõned rakendused kasutada ressurss triviaalne summa, kuid on oluline nõuded muud ressursid.

## <a name="service-tier-capabilities-and-limits"></a>Teenuse taseme funktsioonid ja piirangud
Iga teenuse taseme- ja jõudluse taseme on seostatud erinevate piirangud ja parameetritele. Selles tabelis kirjeldatakse neid omadusi ühe andmebaasi.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

Järgmises jaotises on lisateavet selle kohta, kuidas vaadata need piirangud seotud kasutamine.

### <a name="maximum-in-memory-oltp-storage"></a>Suurim lubatud mälu-OLTP salvestusruum

Vaate **sys.dm_db_resource_stats** abil saate jälgida oma Azure-mälu salvestusruumi kasutada. Jälgimise kohta leiate lisateavet teemast [kuvari-mälu OLTP salvestusruumi](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Praegu Azure-mälu online tehingu töötlemise (OLTP) eelvaade on toetatud ainult ühe andmebaasi. Seda ei saa kasutada andmebaaside elastne andmebaasi kaustadesse.

### <a name="maximum-concurrent-requests"></a>Suurim lubatud samaaegseid taotlusi

Samaaegseid taotlusi arvu kuvamiseks käivitage selle Transact-SQL-päringu SQL-andmebaasi kohta:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Analüüsi töökoormus asutusesisese SQL Serveri andmebaas, muuta selle päringu filtreerida teatud andmebaasidega, mida soovite analüüsida. Näiteks, kui teil on mõne kohapealse andmebaasi nimega MyDatabase, see Transact-SQL-i päring tagastab arvu samaaegseid taotlusi, andmebaasi:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

See on lihtsalt hetktõmmise ühest kohast. Töövooge ja samaaegseid taotluse nõuded parema ülevaate saamiseks peate palju proovide kogumise aja jooksul.

### <a name="maximum-concurrent-logins"></a>Suurim lubatud samaaegseid logimine

Saate oma kasutaja- ja rakenduse mustrite ja saada aimu, milline sisselogimise sagedus analüüsida. Samuti saate käivitada tegelike laadimise veenduge, et teil on pihta see või muud piirangud, mida me selles artiklis käsitletakse testimiskeskkonnas. Pole ühe päringu või dünaamilise juhtimise view (DMV) saate kuvada samaaegseid login loendab või ajalugu.

Kui sama ühendusstringi mitme kliendi kasutamiseks teenuse autendib iga Logi sisse. Kui 10 kasutajat korraga andmebaasiga ühenduse loomiseks sama kasutajanime ja parooli abil, oleks 10 samaaegseid sisselogimise. Piirang kehtib ainult kasutajanime ja autentimise kestus. Kui sama 10 kasutajat andmebaasiga ühenduse loomiseks järjest, samaaegseid sisselogimise arvu kunagi oleks suurem kui 1.

>[AZURE.NOTE] Praegu selle piirmäära ei kehti andmebaasidele elastne andmebaasi kaustadesse.

### <a name="maximum-sessions"></a>Suurim lubatud seansid

Praeguse aktiivsete seansside arv kuvamiseks käivitage see Transact-SQL-päringu SQL-andmebaasi kohta:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Kui olete analüüsimine on asutusesisese SQL serveri töökoormus, muutke päringut keskenduda teatud andmebaasi. Selle päringu abil saate andmebaasi võimalike seansi vajadustele kindlaks teha, kui kaalute teisaldamine Azure'i SQL-andmebaasi.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Klõpsake uuesti need päringud tagastada punkti /-kellaaja arv. Kui te kogute mitme näidised aja jooksul, tuleb teil parim ülevaade seansi kasutada.

SQL-andmebaasi analüüsiks, saate ajalooliste statistika seansid. Päringu **sys.resource_stats**ja kasutage **active_session_count** veergu. Lisateavet selles vaates järgmisest jaotisest.

## <a name="monitor-resource-use"></a>Ressursikasutuse jälgimine
Kahes vaates aitavad teil jälgida ressursikasutuse SQL-i andmebaasi selle teenuse kiht suhtes.

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Saate iga SQL-andmebaasis [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) vaade. Kuvatakse **sys.dm_db_resource_stats** tehtud ressursi kasutamine andmete teenuse kiht suhtes. Keskmine protsentide CPU, andmete I/O, log kirjutab ja mälu salvestatakse iga 15 minutit ja säilitatakse 1 tund.

Kuna see vaade annab täpsema ressursikasutuse, kasutage **sys.dm_db_resource_stats** esimese iga praeguse olekuga analüüsi või probleemide tõrkeotsinguks ja lahendamiseks. Näiteks on kuvatud selle päringu keskmise ja maksimaalse ressursi kasutamine praeguse andmebaasi üle tunnis.

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Muude päringute näited [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats

**Juhtslaidi** andmebaasis [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) vaade on täiendavat teavet, mis aitavad teil kindla teenuse taseme- ja jõudluse tasemel SQL-i andmebaasi jõudluse jälgimist. Andmete kogumine iga 5 minuti ja säilib ligikaudu 35 päeva. See vaade on kasulik pikaajalise ajalooliste analüüsi, kuidas SQL-andmebaasi kasutab vahendid.

Järgneval joonisel on CPU ressursi kasutamise Premium andmebaasi P2 jõudluse taseme iga nädal. See graafik algab Esmaspäev, näitab 5 tööpäevade ja seejärel nädalavahetuse, kui palju vähem juhtub rakenduse.

![SQL-i andmebaasi ressursi kasutamine](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Andmete, see andmebaas on praegu tippväärtus protsessorit üle 50 protsenti CPU kasutamine võrreldes P2 jõudlusega (keskpäevaks Teisipäev). Kui CPU on peamine tegur, mis rakenduse ressursi profiil, siis võib otsustada, et P2 on õige jõudlusega selleks, et tagada, et töökoormus alati sobib. Kui plaanite rakenduse kasvab aja jooksul, on hea mõte on ka eest ressursi puhvri nii, et rakendus pole kunagi jõuda jõudluse taseme limiit. Kui tulemuslikkuse suurendamiseks saate aidata vältida klientide nähtav tõrkeid, mis võivad ilmneda andmebaasi pole piisavalt töödelda taotlusi tõhusalt, latentsus tundliku keskkonnas power. Näide on andmebaasi, mis toetab rakendus, mis värvid andmebaasi kõned tulemuste veebilehtedel.

Pange tähele, et muud tüüpi rakenduse võidakse tõlgendada sama graafik teisiti. Näiteks kui rakendus proovib palgad andmete töötlemiseks iga päev ja on sama diagrammi, seda tüüpi "paketi töö" mudeli võib teha viimistlemiseks P1 jõudluse tasemel. Jõudlusega P1 on 100 DTUs võrrelduna 200 DTUs P2 jõudluse tasemel. P1 jõudlusega pakub pool P2 jõudlusega jõudlus. Jah, 50 protsenti CPU kasutamist P2 võrdub 100% CPU kasutamist P1. Kui rakendus pole ajalõpud, võib see pole oluline kui töö kulub 2 tundi või 2,5 tundi Lõpeta, kui seda saab teha täna. Selle kategooria rakendus ilmselt saate kasutada P1 jõudluse tase. Saate ära asjaolu, et on aega, kui ressursikasutuse on väiksem, et mis tahes "suur tipp" võib hõlmab ka üks soovitud künad hiljem päeva jooksul. Jõudlusega P1 võib olla hea sellist rakenduse (ja säästa) seni, kuni tööd lõpule kellaaja iga päev.

Azure'i SQL-andmebaasi seab tarbitud iga aktiivse andmebaasi **sys.resource_stats** vaates **põhi** andmebaasi iga serveri teave. Andmete tabelis on kokku liita 5 minuti tagant. Basic, Standard, ja teenuse astme, mis andmeid võtta rohkem kui 5 minutit nii, et andmed on abiks ajaloolisi analüüsi asemel lähedal reaalajas analüüsi tabelis kuvada. Päringu **sys.resource_stats** andmebaasi tehtud ajaloo kuvamiseks ning et kontrollida, kas valisite reserveerimine kohale toimetatud jõudlus soovite vastavalt vajadusele.

>[AZURE.NOTE] Teil peab olema ühendatud teie loogika SQL-andmebaasi server **sys.resource_stats** järgmistes näidetes päringu **põhi** andmebaasi.

Selles näites kuvatakse, kuidas selles vaates andmeid on avatud.

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sys.resource_stats kataloogi vaates](./media/sql-database-performance-guidance/sys_resource_stats.png)

Järgmises näites kuvatakse võimalust, mille abil saate teavet selle kohta, kuidas SQL-andmebaasi kasutab vahendid **sys.resource_stats** kataloogi vaates.

1. Vaadata möödunud nädalal ressursi kasutamine andmebaasi userdb1, saate käivitada selle päringu:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Hindamaks, kui hästi oma töökoormus sobib tulemuslikkuse, peate iga osa ressursside mõõdikute süvitsiminek: CPU, loeb, kirjutab, töötajate ja seansside arv. Siin on muudetud päringu abil **sys.resource_stats** nende ressursside mõõdikute väärtuste keskmise ja maksimaalse teatada.

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Väärtuste keskmise ja maksimaalse seda teavet iga ressursi meetermõõdustik, saate hinnata, kuidas oma töökoormus sobib teie valitud jõudluse taseme. Tavaliselt Keskmine väärtused **sys.resource_stats** teile hea võrdlusalus kasutada vastu target suurus. Peaks olema teie peamine mõõtühikute mälupulgale. Näiteks võite kasutada Standard teenuse kiht S2 jõudluse tase. Keskmise kasutamine protsentide CPU ja i/o loeb ja kirjutab on alla 40%, töötajate keskmine arv on alla 50 ja seansside keskmine arv on alla 200. Teie töökoormus võib sobi S1 jõudlusega. On lihtne, et näha, kas andmebaasi sobib töötaja ja seansi piirangud. Et näha, kas andmebaasi sobib jõudluse madalama taseme seoses CPU, loeb ja kirjutab, jagada jõudluse oma praeguse jõudlusega DTU arvu võrra väiksem DTU arvu ja seejärel korrutage tulem väärtusega 100.

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Tulem on kaks jõudluse taset protsentides suhteline jõudluse vahe. Kui teie ressursikasutuse ei ületa see summa, võib teie töökoormus sobi jõudlus väiksem. Siiski peate vaadata kõik vahemikud on ressursi kasutamine väärtuste ja protsendiga, määrata sageduse sobiks teie andmebaasi töökoormus jõudluse madalamale tasemele. Järgmine päring väljundid sobitamine ressursside dimensiooni, aluseks on 40%, mida me selles näites arvutatud kohta protsent:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Vastavalt oma andmebaasi teenuse eesmärk (SLO), saate otsustada, kas teie töökoormus sobib jõudluse madalamale tasemele. Kui teie andmebaasi töökoormus SLO on 99,9 protsenti ja eelmise päring tagastab väärtuste 99,9 protsenti kõigi kolme ressursi dimensioonide suurem, mahub teie töökoormus tõenäoliselt jõudluse madalamale tasemele.

    Vaadates sobitamine protsent annab teile ülevaate, kas teil on liikumiseks suurema jõudlusega oma SLO täita. Näiteks on kuvatud userdb1 järgmised CPU kasutamine viimase nädala jooksul.

  	| Keskmine CPU % | Suurim lubatud CPU % |
  	|---|---|
  	| 24,5 | 100.00 |

    Keskmine CPU on jõudluse tasemel, mis sobiks ka jõudlusega andmebaasi limiit veerand kohta. Kuid suurima väärtuse näitab, et andmebaasi jõuab tulemuslikkuse limiit. Kas vajate liikumiseks järgmise kõrgema taseme jõudlus? Teil on vaadata, kuidas mitu korda oma töökoormus jõuab 100 protsenti, ja seejärel võrrelge seda oma andmebaasi töökoormus SLO.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Kui see päring tagastab väärtuse väiksem kui 99,9 protsenti iga kolme ressursi mõõtmed kas teisaldada suurema jõudlusega või vähendada SQL-andmebaasi rakenduste häälestamine meetodite abil.

4. Selle ülesande leiab ka oma plaanitud töökoormus suurendada tulevikus.

## <a name="tune-your-application"></a>Rakenduse häälestamine

Traditsiooniline asutusesisese SQL serveri, algse võimsus kavandamise protsess sageli on eraldatud valmistamisel rakendus protsessi. Litsentside riistvara ja toode on ostetud esmalt ja jõudluse parandamine on lõpule jõudnud hiljem. Azure'i SQL-andmebaasi kasutamisel on hea mõte põimuvad protsessi rakendus ja selle häälestamine. Maksma võimsus nõudmisel mudeliga saate häälestada rakenduse kasutamiseks vajalikud nüüd asemel riistvara põhjal arvailtaisiin kasvu tulevikus lepingutest rakendus, mis on sageli vale overprovisioning minimaalne ressursid. Mõned kliendid võivad ei soovi häälestada rakenduse ja selle asemel valida overprovision riistvara ressursse. Seda moodust võib olla hea mõte, kui te ei soovi muuta võtme rakenduse hõivatud aja jooksul. Kuid häälestamine rakenduse saate minimeerida ressursi nõuded ja alumise igakuised arved Azure'i SQL-andmebaasi astme teenuste kasutamisel.

### <a name="application-characteristics"></a>Rakenduse omadused

Kuigi Azure'i SQL-andmebaasi teenuse astme eesmärk on parandada jõudlust stabiilsuse ja paremat teavet rakenduse, parimaid tavasid abil saate häälestada rakenduse paremini ära ressursside jõudluse tasemel. Kuigi paljudes rakendustes on oluline jõudluse tõstmiseks lihtsalt, saate aktiveerida jõudluse kõrgema või teenuse kiht, mõned rakendused vajate täiendavaid häälestada teenuse kõrgema taseme. Parema jõudluse, on soovitatav täiendavate rakenduste häälestamine rakendusi, mis on need omadused.

- **Rakendusi, mis on aeglane jõudlus "jutukas" käitumise tõttu**. Jutukas rakendused teha liigse andmed Accessi toiminguid, mis on tundlik võrgu latentsus. Võib juhtuda, muuta nende tüüpi rakendusi andmete Accessi toimingute SQL-andmebaasiga arvu vähendamiseks. Näiteks võib rakenduse jõudluse parandamiseks tehnoloogiate nagu partiide erakorralist päringute või teisaldada päringute salvestatud toimingute abil. Lisateavet leiate teemast [paketi päringud](#batch-queries).
- **Andmebaasid on intensiivse töökoormus, mis ei toeta mõnda kogu ühe masina järgi**. Andmebaaside, mis ületavad Premium jõudluse kõrgeima ressursside võib kasu skaala läbi töökoormus. Lisateavet leiate teemast [risti andmebaasi sharding](#cross-database-sharding) ja [funktsionaalne eraldamine](#functional-partitioning).
- **Rakenduste optimaalsetesse päringud, mis on**. Rakenduste, eriti need andmed Accessi kiht, mis on halvasti häälestatud päringute võib ei kasu jõudluse kõrgemal tasemel. See hõlmab päringud, mis puudub WHERE-klauslis, on puudu indeksid või statistika on aegunud. Nende rakenduste kasu standard query jõudluse parandamine meetodite abil. Lisateabe saamiseks vt [puuduvate registrid](#missing-indexes) ja [päringu seadistus ja vihjab](#query-tuning-and-hinting).
- **Rakenduste optimaalsetesse andmeid, mis on juurdepääs kujundus**. Rakendusi, mis on omased andmed Accessi kokkulangevus küsimusi, näiteks deadlocking, võib kasu jõudluse kõrgemal tasemel. Kaaluge vähendada vahemällu andmete vahemällu Azure'i teenus või mõne muu vahemällu tehnoloogia kliendipoolse edasi-tagasi Azure SQL-i andmebaasist. Vt [rakenduse taseme vahemällu](#application-tier-caching).

## <a name="tuning-techniques"></a>Tehnika häälestamine
Selles jaotises vaatame mõningaid viise, mille abil saate häälestada rakenduse parimaid tulemusi saada ja käivitage see võimalik jõudluse minimaalselt Azure'i SQL-andmebaasi. Mõned nende meetodite abil vastavad traditsiooniline SQL serveri häälestamine head tavad, kuid teised on teatud Azure SQL-andmebaasiga. Mõnel juhul saate uurida tarbitud ressursside andmebaasi leida täpsemalt häälestada ja laiendamine traditsiooniline SQL serveri tehnikate Azure'i SQL-andmebaasi töötada.

### <a name="azure-portal-tools"></a>Azure'i portaali tööriistad
Leiate Azure'i portaalis, mis aitavad teil kahe tööriistad analüüsimiseks ja jõudlusega seotud probleemide lahendamiseks ja SQL-andmebaasi.

- [Päringu täitmise ülevaade](sql-database-query-performance.md)
- [SQL-i andmebaasi Advisor](sql-database-advisor.md)

Azure'i portaalis on mõlemad nende tööriistade ja kuidas neid kasutada kohta lisateavet. Tõhus diagnoosimine ja parandamine probleeme, soovitame proovige esmalt tööriistad Azure'i portaalis. Soovitame kasutada käsitsi häälestamine lähenemisel, mida me arutada järgmiseks registrid ja päringu häälestamine on puudu.

### <a name="missing-indexes"></a>Puuduvate registrid
Levinud OLTP andmebaasi jõudluse probleem on seotud füüsilise andmebaasi kujunduse. Sageli loodud andmebaasi skeemid ja ilma testimist skaala (kas laadi või andmete maht). Kahjuks päringu lepingu täitmise võib olla small skaalal aktsepteeritav, kuid halvendada oluliselt jaotises tootmise tase andmemahtudega. Selle probleemi levinumaid allika on sobiv registrid vastavad filtrid või muude piirangutega päringu puudumine. Sageli puuduvad registrite manifestid tabelina skannida, kui mõni registri seek piisab.

Selles näites kasutab valitud päringu plaan skannimise kui otsida piisaks:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Päringu lepingu puuduvad indeksid](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

SQL Azure'i andmebaas aitab teil leidmine ja parandamine levinud puuduvad indekseerida tingimused. Azure'i SQL-andmebaasi sisse ehitatud DMVs vaadata päringu kompileeritud registri oleks oluliselt vähendada prognoositud kulud päringu käivitamiseks. Päringu täitmisel, SQL-andmebaasi jälitab sageduse iga päringu plaan käivitatakse ning jälgib hinnanguline vahe täidesaatva päringu plaan ja kujutada ühe, kus see indeks on olemas. Saate neid DMVs kiiresti mõelda milliseid muudatusi füüsilise andmebaasi kujunduse võib parandada töökoormus kogukulu andmebaasi ja oma reaal töökoormus.

Selle päringu abil saate hinnata võimalike puuduvate registrid:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

Selles näites päringu tulemuseks selle päringusoovituste:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Pärast selle loomist sama SELECT-lause huvitavat mõnda muud lepingut, mis kasutab skannimise asemel otsida, ja seejärel käivitab leping tõhusamalt:

![Päringu plaan parandatud indeksid](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Võtme ülevaate on ühiskasutuses, kaup süsteemi I/O võimsus rohkem piiratud kui spetsiaalne server kohapeal. Seal on lisatasu minimeerimine mittevajalike I/O ära maksimaalne süsteemi DTU iga jõudluse taseme Azure'i SQL-andmebaasi teenuse astme. Sobiv füüsilise andmebaasikujunduse valikuid võib oluliselt parandada latentsus üksikuid päringuid, parandada samaaegseid taotlusi käsitleda skaala ühiku jõudlus ja minimeerida kulusid, mis rahuldab päring. Puuduvad registri DMVs kohta leiate lisateavet teemast [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Päringu seadistus ja vihjab
Päringu optimeerija Azure SQL-andmebaasis on sarnane traditsiooniline SQL serveri päringu optimeerija. Kõige paremini häälestamine päringud ja mõistmine arutluskäik mudeli piirangud päringu optimeerija rakendada ka Azure'i SQL-andmebaasi. Kui saate häälestada päringute Azure'i SQL-andmebaasi, võidakse kuvada kasulik vähendada liitväärtuse ressursi nõudmistele. Rakenduse võib olla võimalik käivitada untuned samaväärne madalama hinnaga, sest see saab käitada jõudlus väiksem.

Näide, mis on levinud SQL serveri ja mis on olemas ka Azure SQL-andmebaas on kuidas päringu optimeerija "sniffs" parameetrid. Päringu optimeerija hindab koostamise, määratlemaks, kas seda saab luua optimaalne päringu plaan parameetri praegust väärtust. Kuigi see strateegia sageli võib põhjustada päringu leping, millele on oluliselt kiirem kui kompileeritud ilma teadaolevate parameetrite väärtused leping, praegu see toimib puudulikult nii SQL Server ja Azure SQL-andmebaasis. Mõnikord on pole nuusutamiseks parameetri, ja mõnikord parameeter on nuusutamiseks, kuid parameetrite väärtused on töökoormus täiskomplekti jaoks on loodud leping. Microsoft sisaldab päringu vihjeid (suuniseid), nii et saate määrata mudeli mitme tahtlikult ja parameeter vahevõrguruuterit vaikekäitumise alistada. Sageli, kui kasutate näpunäiteid, saate lahendada SQL serveri või Azure'i SQL-andmebaasi vaikekäitumine on puudulik jaoks kindla kliendi töökoormus.

Järgmises näites näitab, kuidas päringu protsessor saate luua leping, millele on nii jõudluse ja ressursside nõuetele. Selles näites on ka näha, et päringu vihje kasutamisel saab vähendada aja- ja käivitage päring nõuded SQL-i andmebaasi:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Häälestamise koodi loob tabeli, mis on moonutatud andmete jaotuse. Optimaalne päringu plaan erineb põhjal parameeter, mis on valitud. Kahjuks ei vahemällu käitumise leping alati Kompileeri päringu põhjal parameetri kõige enam esineva väärtuse. Jah, on võimalik optimaalsetesse plaani vahemällu ja kasutada mitme väärtuse, isegi siis, kui mõnda muud lepingut võib olla parem leping valik keskmiselt. Klõpsake päringu plaan loob kaks salvestatud toimingute, mis on täpselt ühesugused, välja arvatud siis, kui üks on teisiti päringu vihje.

**Näide 1 osa**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Näiteks osa 2**

(Soovitatav oodata vähemalt 10 minutit enne alustamist osa 2, näiteks nii, et tulemused erinevad tulemuseks telemeetria andmeid.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Selles näites iga osa püüab käitada parameetritega lisa 1000 (loomiseks piisavalt laadi kasutamiseks andmehulgas test). Kui see käivitab salvestatud toimingute, päringu protsessor uurib parameetri väärtuse, mis protseduuri esimese koostamise (parameeter "vahevõrguruuterit"). Protsessori vahemälu tulemuseks leping ja kasutab seda hiljem manamine, isegi juhul, kui parameetri väärtus on erinev. Optimaalse leping, võib igal juhul ei tohi kasutada. Mõnikord on vaja juhend optimeerija valida leping, millele on parem konkreetsel asemel Keskmine juhul, kui päring on esmalt koostada. Selles näites esialgse kava genereeritud "skannimine" leping, mis loeb kõik read iga väärtuse, mis vastab parameetri leidmiseks:

![Päringu abil skannimine leping häälestamine](./media/sql-database-performance-guidance/query_tuning_1.png)

Kuna me täidetud toimingu abil väärtuse 1, tulemuseks leping oli optimaalne väärtuse 1, kuid on optimaalsetesse kõik muud väärtused tabeli jaoks. Tõenäoliselt tulemus ei ole, mida soovite valida iga leping juhusliku ID-ga, kuna leping sooritab aeglasemalt ja kasutab rohkem ressursse oleksite.

Kui käivitate katse `SET STATISTICS IO` määratud `ON`, loogiline skannimine töö selles näites on valmis taustal. Näete, on olemas 1,148 loeb teha leping (mis on ebaefektiivne, kui keskmine on ainult ühe rea tagastamiseks):

![Päringu abil loogilise skannimine häälestamine](./media/sql-database-performance-guidance/query_tuning_2.png)

Teine osa näide kasutab päringu vihje öelda optimeerija kasutada kindla väärtuse koostamise ajal. Sel juhul selle päringu protsessor ignoreerida väärtuse, kui parameeter, mis jõustab ja selle asemel endale `UNKNOWN`. See viitab väärtus, mis on keskmine sagedus tabeli (ignoreerides skew). Tulemuseks plaan on seek põhinev leping, millele on kiirem ja kasutab vähem ressursse, keskmise, kui leping 1 osa selles näites:

![Päringu abil päringu vihje häälestamine](./media/sql-database-performance-guidance/query_tuning_3.png)

Saate vaadata tabelis **sys.resource_stats** efekti (on aeg, et käivitada test ja millal andmed kuvab tabeli viivitus). Selles näites, osa 1 22:25:00 ajaakna jooksul täidetud ja osa 2 täidetud 22:35:00. Pange tähele, et varasemates ajaakna kasutatakse selle ajaakna (tõttu lepingu tõhusust täiustused) hiljem kui veel ressursse.

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Päringu häälestamine näidistulemid](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Kuigi helitugevuse selles näites on teadlikult väike, optimaalsetesse parameetrite mõju võib olla olulisi, eriti suuremate andmebaaside kohta. Vahe, äärmiselt juhul võib olla vahemikus kiiresti juhtudel ja tunni aeglane juhtudel.

Saate uurida **sys.resource_stats** määratlemaks, kas ressursi testi kasutab rohkem või vähem kui teise testi ressursid. Kui võrrelda andmeid, eraldi kontrollib ajastuse nii, et nad ei ole sama 5-minutilise aknas **sys.resource_stats** vaates. Kasutamise eesmärk on kasutatud ressursside kogusumma minimeerimiseks ja pole minimeerimiseks tippväärtus ressursid. Üldiselt optimeerimine koodi latentsus tükk vähendab ressursside tarbimine. Veenduge, et rakendus tehtud muudatused on vajalikud ja muudatusi ei negatiivselt klientide programmikasutuskogemuse keegi, kes võivad kasutada päringu näpunäiteid rakenduse.

Kui on töökoormus on korduv päringute kogumi, sageli on mõistlik jäädvustada ja kinnitada optimaalsuse valikut leping, sest see määrab minimaalse ressursi suurus üksuse majutada andmebaasi nõutav. Pärast kinnitamist muuta, aeg-ajalt vaatab plaanib aitab teil tagada, et need on halvenenud. Saate lisateavet [päringu vihjeid (Transact-SQL-i)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Rist-andmebaasi sharding
Kuna Azure'i SQL-andmebaasi käivitatakse kaup riistvaralise, võimsuse piiranguid ühe andmebaasi on väiksemad kui traditsiooniline asutusesisese SQL Serveri install. Mõned kliendid kasutada mitut andmebaasi andmebaasi toimingud jaotada, kui need toimingud ei mahu piirides Azure'i SQL-andmebaasi ühe andmebaasi sharding meetodite abil. Enamik klientidele, kes sharding tehnikate kasutamine Azure'i SQL-andmebaasi tükeldamine nende andmete ühe mõõtme üle mitme andmebaasi. Seda moodust, peate aru, et OLTP rakenduste sageli teha tehingud, mis rakenduvad ainult üks rida või read skeemi väikese rühma.

>[AZURE.NOTE] SQL-andmebaasi nüüd pakub teegi abistamiseks sharding. Lisateavet leiate teemast [elastne andmebaasi kliendi teegi ülevaade](sql-database-elastic-database-client-library.md).

Näiteks kui andmebaas on kliendi nimi, tellimuse ja Tellimuse üksikasjad (nt traditsiooniline näide põhjatuul andmebaasi, mis on SQL Server), võib tükeldamist andmed mitme andmebaasi jaotistesse rühmitada seotud tellimuse ja Tellimuse üksikasjad teabe kliendile. Saate tagada, et kliendiandmed jääb ühe andmebaasi. Rakendus oleks split erinevate klientide üle andmebaaside tõhusalt levitada laadi üle mitme andmebaasid. Sharding, mitte ainult kliendid saate vältida maksimaalne andmebaasi suurus piiratud, kuid Azure'i SQL-andmebaasi saab töödelda ka töökoormus, mis on märkimisväärselt suurem kui piirangud erinevate jõudluse taset, kui selle DTU sobib iga üksiku andmebaasi.

Kuigi andmebaasi sharding ei vähenda liitväärtuse Ressursi võimsus lahenduse, on väga tõhus on väga suurte lahendusi, mis on ulatub üle mitme andmebaasi toetavad. Iga andmebaasi saab käitada erinevatele tasemel toetama väga mahukad, kõrge ressursi nõuded "tõhus" andmebaasid.

### <a name="functional-partitioning"></a>Otstarbekas jagamine
SQL Serveri kasutajad ühendada sageli palju funktsioone ühe andmebaasi. Näiteks, kui rakendus on loogika lisamine poe varude haldamiseks, andmebaasi on seostatud laoseisu jälgimine tellimuste, Salvestatud toimingute ja indekseeritud või materialiseerunud vaateid, mis kuu lõpu aruandluse haldamine loogika. Selle meetodi abil on lihtsam teha näiteks varundamise toimingute andmebaasi haldamine, kuid seda ka nõuab teilt riistvara käsitlema tippväärtus laadi üle kõik funktsioonid rakenduse suuruse muutmiseks.

Kui kasutate skaala-out arhitektuur Azure SQL-andmebaasis, on hea mõte erinevaid funktsioone rakenduse muu andmebaasi tükeldamine. Selle meetodi abil konkreetse rakenduse skaala sõltumatult. Kui rakenduse muutub kuvab rohkem teatisi (ja andmebaasi koormus suureneb) administraator saate valida sõltumatu jõudluse taset iga funktsiooni rakenduse. Piiri, selle arhitektuuri rakendus võib olla suurem kui ühe kauba kohapeal saab hakkama, sest laadi on levinud mitme arvutites.

### <a name="batch-queries"></a>Paketi päringud
Rakendusi, mis pääsevad andmetele juurde, kasutades suure mahu, sagedased sihtotstarbeline päringud, palju aega kulub võrguühenduse rakenduse taseme- ja Azure'i SQL-andmebaasi taseme vahel. Isegi siis, kui rakendamist ja Azure SQL-andmebaas on samas andmekeskuse, kahe võrgu latentsusaeg võib suurendatud, suure hulga andmete Accessi toimingud. Võrgu vähendamiseks round reisi andmete Accessi toimingute, kaaluge võimalust kas partii kohapealseid päringuid ja koostada neile nii salvestatud toimingute. Kui te partii kohapealseid päringuid, saate mitme päringu saatmine ühe suure paketi ühe reisi Azure SQL-andmebaasiga. Kui teil kompileerida kohapealseid päringuid salvestatud protseduuri, võib saavutada sama tulemuse nagu siis, kui te neid partii. Salvestatud toimingu abil annab teile kasu suurendada võimalusi vahemällu Azure SQL-andmebaasis lepingud päringu abil saate salvestatud toimingu uuesti.

Mõned rakendused on kirjutamine mahukat. Mõnikord võib andmebaasi I/O koormust vähendada arutate, kuidas partii kirjutab koos. Sageli, on sama lihtne kui konkreetsete tehingud asemel automaatne-Kinnita tehingud salvestatud toimingute ja sihtotstarbelise pakettidena. Hindamise meetodit saate kasutada, leiate [Azure'i SQL-andmebaasi rakenduste Batching võtteid](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Proovige oma töökoormus partiide leida õige mudel. Kindlasti, et aru saada, et mudeli võib olla veidi teistsugune selgituseks järjepidevuse garantiid. Õige töökoormus, mis vähendab ressursikasutuse otsimise jaoks on vaja leida järjepidevuse ja jõudluse kompromisse õige kombinatsioon.

### <a name="application-tier-caching"></a>Rakendus-astme vahemällu talletamine
Mõned andmebaasirakendusi on raske lugeda töökoormus. Vahemällu kihid võib vähendada andmebaas ja võib potentsiaalselt vähendada jõudluse nõutav andmebaasi Azure'i SQL-andmebaasi abil. [Azure'i Redis vahemälu](https://azure.microsoft.com/services/cache/), kui teil on raske lugeda töökoormus, saate lugeda andmed üks kord (või ehk üks kord rakendus-astme kohapeal, olenevalt sellest, kuidas see on konfigureeritud), ja seejärel talletamine väljaspool SQL-andmebaasi andmeid. See on viis vähendada andmebaasi laadimine (CPU ja lugege I/O), kuid on selgituseks järjepidevuse mõju, sest vahemälust lugeda andmed võivad olla sünkroonitud andmed andmebaasis. Kuigi paljudes rakendustes teatud vastuolu on aktsepteeritav, mis pole täidetud kõigi töökoormus. Rakenduse nõuded peaksid täielikult aru, enne kui saate rakendada rakendus-astme vahemällu strateegia.

## <a name="next-steps"></a>Järgmised sammud

- Teenuse astme kohta leiate lisateavet teemast [SQL-andmebaasi suvandid ja jõudlus](sql-database-service-tiers.md)
- Elastne andmebaasi kaustu kohta leiate lisateavet teemast [mis on Azure elastne andmebaasi kohapeal on?](sql-database-elastic-pool.md)
- Lisateabe saamiseks jõudlus ja elastne andmebaasi kaustu, saate teada, [Millal kohapeal on elastne andmebaasi silmas pidada?](sql-database-elastic-pool-guidance.md)
