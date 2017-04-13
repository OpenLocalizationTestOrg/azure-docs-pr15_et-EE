 <properties
    pageTitle="Kuidas kasutada partiide Azure'i SQL-andmebaasi rakenduse jõudluse parandamiseks"
    description="Teema sisaldab tõendusmaterjalide kogumiseks, et partiide andmebaasi toimingud oluliselt imroves kiiruse ja skaleeritavus Azure'i SQL-andmebaasi rakenduste. Kuigi need partiide meetodid mõnda SQL serveri andmebaasi, on see artikkel keskendub Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Kuidas kasutada partiide SQL-andmebaasi rakenduse jõudluse parandamiseks

Partiide toimingute Azure SQL-andmebaasiga oluliselt parandab jõudlus ja skaleeritavus rakenduste. Mõistmaks eeliste esimene osa selles artiklis antakse ülevaade mõne valimi testi tulemused, mis võrdlevad järjestikku ja batched taotlusi SQL-andmebaasiga. Artikli ülejäänud kuvatakse meetodid, stsenaariumid ja peaksite arvesse võtma, et saaksite kasutada partiide edukalt oma Azure rakendused.

**Autorid**: Jason Roth, Silvano Coriani Trent Swanson (sh skaala 180)

**Läbivaatajate**: Conor Cunningham, Michael Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Miks on partiide SQL-andmebaasi jaoks oluline?
Partiide kõned remote teenus on tuntud strateegia suurendamiseks jõudlus ja skaleeritavus. Parandatud mis tahes kasutusviisid remote teenusega, nt sariväljaanne, võrku üle ja vahemäluasukohaga töötlemise kulud. Mitme eraldi tehingud ühe paketi pakendamine minimeeritakse need kulud.

Selles dokumendis tahame uurida erinevate SQL-andmebaasi partiide strateegiaid ja stsenaariumi. Kuigi need strateegiad oluliste asutusesisese SQL serveri kasutavate rakenduste puhul, on mitu põhjust esiletõstmine partiide SQL-i andmebaasi kasutamine.

- On potentsiaalselt suurem Võrgu latentsuse juurdepääs SQL-andmebaasi, eriti siis, kui avate väljaspool sama andmekeskuse Microsoft Azure'i SQL-andmebaasi.
- SQL-andmebaasi rentnikuga omaduste tähendab, et andmed Accessi kiht korreleerub üldise skaleeritavus andmebaasi tõhusust. SQL-andmebaasi tuleb ühe rentniku/kasutaja takistada monopoliseerimise andmebaasi ressursid kahjuks muud rentnikke. Vastuseks kasutus ületab eelmääratletud piirmäära, SQL-andmebaasi läbilaskevõime vähendada või vastamiseks pidurdamise erandid. Tõhusust, nt partiide, võimaldavad rohkem SQL-andmebaasi need piirangud enne tööd teha. 
- Partiide kehtib ka arhitektuurides, mida kasutada mitut andmebaasi (sharding). Teie andmebaasi iga üksuse suhtlus tõhusust on endiselt oluline tegur oma üldine skaleeritavus. 

Üks SQL-andmebaasi kasutamise eeliseid on, et teil pole andmebaasi majutavad serverid haldamine. Siiski selle hallatava taristu ka tähendab, et peate andmebaasi Optimeerimised teisiti mõelda. Saate vaadata enam andmebaasi riistvara või võrguühenduse taristu parandamiseks. Microsoft Azure'i juhtelemendid nendes keskkondades. Peamised ala, kus saate määrata on, kuidas rakenduse suhtleb SQL-andmebaasi. Partiide on üks neid Optimeerimised. 

Raamatu esimene osa uurib erinevate partiide võtteid .net-i rakendusi, mis kasutavad SQL-andmebaasi. Kaks viimast jaotiste katta partiide juhised ja stsenaariumid.

## <a name="batching-strategies"></a>Partiide strateegiad

### <a name="note-about-timing-results-in-this-topic"></a>Pange tähele ajastuse tulemused selle teema kohta
>[AZURE.NOTE] Tulemused pole kriteeriumid, kuid on mõeldud **suhteline jõudluse**kuvamiseks. Ajastuste põhinevad keskmiselt vähemalt 10 testi töötab. Lisab tühja tabelisse on toimingud. Katsed olid mõõdetud eel-V12 ja need vasta läbilaskevõime, võib ilmneda V12 andmebaasi abil uue [teenuse astme](sql-database-service-tiers.md). Suhteline kasu partiide tehnika peaks olema samasugune.

### <a name="transactions"></a>Tehinguid
Tundub kummaline alustada partiide käsitletakse tehingud ülevaade. Kuid kliendipoolne tehingud kasutamisel on väike serveripoolne partiide kohta, et parandab jõudlust. Ja tehingud saab lisada ainult mõne rida koodi, nii, et need võimaldavad kiiresti järjestikku toimingute jõudluse parandamiseks.

Võtke arvesse järgmist C# koodi sisaldava lisa jada ja toiminguid lihtsat tabelit värskendada.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Järgmine kood ADO.net-i järjest neid tehete.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Parim viis optimeerimine järgmine kood on mingi kliendipoolne partiide need kõned rakendada. Kuid on lihtne viis suurendada lihtsalt pakkimine sequence kõnede tehingu järgmine kood jõudlus. Siin on sama mis kasutab tehingu kood.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Nii nendes näidetes kasutatakse tegelikult tehingud. Esimese näites on iga üksiku kõne peidetud toimingu. Teine näide konkreetsete toimingu murtakse kõigi kõnede. Kohta, [kirjutage edasi tehingulogi](https://msdn.microsoft.com/library/ms186259.aspx)dokumentatsioonist Logi kirjed on kettale kui tehingu sooritab. Nii, sh veel kõned tehing, kirjuta tehingulogi viivitada kuni tehingu on kinnitatud. Tegelikult on lubamine partiide jaoks soovitud kirjutab serveri tehingulogi.

Järgmises tabelis on mõned erakorralist testimise tulemused. Katsed läbi sama järjestikku lisatakse ka ilma tehingud. Lisateavet perspektiivi, kontrollib esimeste parandusfunktsiooni kaugühenduse teel sülearvutit andmebaasi Microsoft Azure. Katsete teise kogumi parandusfunktsiooni pilveteenuses ja andmebaas, et mõlemad elanud sama Microsoft Azure'i andmekeskuse (Lääne USA) jooksul. Järgmine tabel näitab kestuse millisekundites järjestikku lisab ja ilma tehingud.

**Kohapealse Azure**:

| Toimingud | Pole tehingu ([ms) | Tehingu ([ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure'i abil Azure (sama andmekeskuse)**:

| Toimingud | Pole tehingu ([ms) | Tehingu ([ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Tulemused ei ole kriteeriumid. Vt [Märkus ajastuse tulemused selle teema kohta](#note-about-timing-results-in-this-topic).

Eelmise kontrolltöö tulemuste põhjal, mähkimine ühekordne toimingu tegelikult väheneb jõudlus. Kuid piires ühe toimingu arvu suurendamisel jõudluse parandamiseks muutub veel märgitud. Jõudluse vahe on rohkem märgatav ka siis, kui kõik toimingud, mis ilmnevad Microsoft Azure'i andmekeskuse. Suurem latentsus kasutamine väljaspool andmekeskuse Microsoft Azure'i SQL-andmebaasi varju jõudluse gain kasutamise tehingud.

Kuigi tehinguid kasutamine võib jõudlust suurendada, jätkake [jälgige tehinguid ja ühendused põhitõed](https://msdn.microsoft.com/library/ms187484.aspx). Tehingu võimalikult vähe hoida ja sulgege andmebaasiühenduse pärast töö lõpulejõudmist. Funktsiooni eelmise näite lause kasutamine tagab, et ühendus on suletud, kui edasiste kood plokk on lõpule jõudnud.

Eelmise näite näitab, et kohalik tehing saate lisada mis tahes ADO.net-i kood on kaks rida. Tehingud pakkuda kiiresti kood, mis toodab järjestikku lisada, värskendada ja kustutada toimingute jõudluse parandamiseks. Siiski parima jõudluse, kaaluge muutmist ära kliendipoolne partiide, nt tabelis hinnatud parameetreid pärast koodi.

Tehingute ADO.net-i kohta leiate lisateavet teemast [Kohalik tehingute ADO.net-i](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Tabelis hinnatud parameetrid
Tabelis hinnatud parameetreid toetada Transact-SQL-lausete, Salvestatud toimingute ja funktsioonide parameetrite tabeli kasutaja määratletud tüübid. Selle kliendipoolne partiide tehnika võimaldab teil saada mitme rea andmete tabelis hinnatud parameetri. Tabelis hinnatud parameetreid kasutamiseks esmalt määrama tabeli tüüp. Järgmine Transact-SQL-lause loob tabeli tüüp, nimega **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

Koodi, saate luua on **Saidihaldur** täpselt sama nime ja tüüpi tabeli tüüp. Liigu **Saidihaldur** see parameeter päringu teksti või salvestatud protseduur kõne. Järgmises näites on kujutatud selle meetodi:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

Eelmises näites **SqlCommand** objekti lisab rida tabelis hinnatud parameetri **@TestTvp**. Varem loodud **Saidihaldur** objekt on määratud parameeter **SqlCommand.Parameters.Add** meetod. Lisab ühe kõne märkimisväärselt partiide suurendab jõudlust järjestikku lisab üle.

Eelmise näite edasiseks parandamiseks kasutada salvestatud protseduur asemel tekstipõhine käsk. Järgmine Transact-SQL-i käsk loob salvestatud protseduur, mis võtab **SimpleTestTableType** tabelväljundiga parameeter.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Muutke **SqlCommand** objekti deklareerimise kood eelmises näites järgmine.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

Enamikul juhtudel tabelis hinnatud parameetreid on samaväärne võrreldes teiste partiide meetodite abil parema tulemuse. Tabelis hinnatud parameetreid sageli soovitatav, kuna need on paindlikumad muid võimalusi. Näiteks võimaldab teiste meetodite abil, nt SQL-i hulgi kopeerimine, üksnes uue rea lisamine. Kuid tabelis hinnatud parameetriga saate loogika salvestatud protseduur määrata, millised read on värskendused ja mis on lisab. Tabeli tüüp saate muuta ka sisaldama "Seda toimingut" veerg, mis näitab, kas määratud rea peaks olema lisada, värskendada või kustutada.

Järgmine tabel näitab erakorralist kontrolltöö tulemuste tabelis hinnatud parameetreid kasutamiseks millisekundites.

| Toimingud | Kohapealse Azure ([ms)  | Azure'i sama andmekeskuse ([ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Tulemused ei ole kriteeriumid. Vt [Märkus ajastuse tulemused selle teema kohta](#note-about-timing-results-in-this-topic).

Jõudluse kasu partiide ilmneb kohe. Eelmise järjestikuse katse, 1000 toimingute võttis 129 sekundit väljaspool andmekeskuse ja 21 sekundi andmekeskuse sees. Kuid parameetriga tabelväärtusega 1000 toiminguid ainult 2.6 sekundi väljaspool andmekeskuse ja sekundit andmekeskuse sees.

Tabeliväärtusega parameetrite kohta leiate lisateavet teemast [Table-Valued parameetrid](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL-i hulgi kopeerimine
SQL-i hulgi Kopeeri on teine võimalus sisestage suurte andmehulkade target andmebaasi. .Net-i rakendusi saate kasutada **SqlBulkCopy** hulgi lisa toimingute tegemiseks. **SqlBulkCopy** on sarnane funktsioon käsurea tööriista, **Bcp.exe**või Transact-SQL-lauset, **Hulgi lisada**. Koodi järgmises näites kujutatakse hulgi koopia read **Saidihaldur**, tabel, SQL serveri MyTable sihttabel allikas.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

On juhtumeid, kus Kopeeri hulgilisamine eelistatakse tabelis hinnatud parameetreid. Vaadake võrdlustabelit tabelis hinnatud parameetrite võrreldes teemas [Table-Valued parameetrite](https://msdn.microsoft.com/library/bb510489.aspx)hulgi lisada toiminguid.

Järgmised erakorralist testi tulemused näitavad jõudlus partiide koos **SqlBulkCopy** millisekundites.

| Toimingud | Kohapealse Azure ([ms)  | Azure'i sama andmekeskuse ([ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Tulemused ei ole kriteeriumid. Vt [Märkus ajastuse tulemused selle teema kohta](#note-about-timing-results-in-this-topic).

Väiksema paketi väärtusega, kasutage tabelis hinnatud parameetrid edestas **SqlBulkCopy** klassi. **SqlBulkCopy** sooritatakse 12 – 31% kiiremini tabelis hinnatud parameetreid testide 1000 kuni 10 000 rida. Tabelis hinnatud parameetreid, nt **SqlBulkCopy** on hea võimalus batched lisab, eriti siis, kui võrrelduna-batched toimingute.

Hulgi koopia ADO.net-i kohta leiate lisateavet teemast [Hulgi Kopeeri toimingute SQL serveris](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Mitme rea lisamine parameetriga laused
Ühe alternatiiv väikeste pakettidena on ehitada suur parameetritega lisa lause, mis lisab mitu rida. Järgmine kood näide näitab selle meetodi.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

Selles näites on mõeldud põhimõttel kuvamiseks. Suurema stsenaarium oleks pidev nõutav üksuste ehitada päringu string ja käsk parameetrid korraga kaudu. Olete piiratud 2100 päringu parameetrite, kokku nii, et see piirab arv ridu, mida saab töödelda sel viisil.

Järgmised erakorralist testi tulemused näitavad seda tüüpi lisa lause jõudlus millisekundites.

| Toimingud | Tabelis hinnatud parameetreid ([ms) | Ühe-lause INSERT ([ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Tulemused ei ole kriteeriumid. Vt [Märkus ajastuse tulemused selle teema kohta](#note-about-timing-results-in-this-topic).

Seda moodust saab veidi kiiremini töölehed, mis on väiksem kui 100 ridade jaoks. Kuigi parandamine on väike, see meetod on mõni muu suvand, mis ei pruugi töötada ka teie konkreetse rakenduse stsenaariumis.

### <a name="dataadapter"></a>DataAdapter
Klassi **DataAdapter** võimaldab teil muuta **andmekomplekti** objekti ja seejärel esitada muudatusi nagu lisada, värskendada ja kustutada. Kui kasutate **DataAdapter** sel viisil, on oluline märkida, et eraldi tehakse iga erinevate toimingu. Jõudluse parandamiseks kasutada toimingute kohta, mida tuleks batched haaval arvu atribuuti **UpdateBatchSize** . Lisateavet leiate teemast [Läbimiseks paketi toimingute abil DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Üksuse raames
Üksuse raames ei toeta praegu partiide. Erinevate arendajate ühenduse proovinud näidata lahendusi, nt alistamine **SaveChanges** meetod. Kuid lahendused on tavaliselt keerukas ja kohandatud rakenduse ja andmemudelit. Üksuse raames codeplex projekt on praegu arutelu lehe funktsiooni taotluse kohta. Arutelu, Kuva [Kujundus koosolekumärkmete – 2 .augusti 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML-I
Teiseks on tundub on oluline XML-i partiide strateegia rääkida. XML-i kasutamine on siiski ole teiste meetodite eeliseid ja puudusi. Lähenemine on sarnane tabelis hinnatud parameetreid, kuid XML-faili- või stringi edastatakse salvestatud protseduur asemel tabeli kasutaja määratletud. Salvestatud protseduur sõelub salvestatud protseduur käsud.

Seda moodust puudusi on:

- XML-i töötamise võib olla tülikas ja vigu.
- XML-i andmebaasi sõelumine võib olla Protsessori jaoks mahukat.
- Enamasti on see meetod aeglasem kui tabelis hinnatud parameetreid.

Nende huvides on soovitatav kasutada XML paketi päringuid.

## <a name="batching-considerations"></a>Partiide kaalutlused
Järgmistes jaotistes on veel abi kasutuse partiide SQL-andmebaasi rakendustes.

### <a name="tradeoffs"></a>Kompromissidega
Olenevalt teie arhitektuur partiide kaasata on Miinuseks jõudlus ja paindlikkust vahel. Näiteks, võiksite seda stsenaariumi, kus teie roll ootamatult läheb alla. Ühe rea andmete unustamisel mõju on väiksem kui mõju kaotada suure partii esitamine read. On suurem kui te puhvri ridade enne saatmist määratud ajaakna andmebaasi.

Kuna see Miinuseks hinnata tegevuse selle paketi. Partii agressiivsemalt (suurem pakettidena ja rohkem aega Windowsi) andmetega, mis on väiksem kriitilise tähtsusega.

### <a name="batch-size"></a>Paketi suurus
Meie katsed on tavaliselt pole kasutada katkestamine suurte pakettidena väiksem tükkideks. Tegelikult on see osa sageli tulemuseks aeglasem kui ühe suure paketi esitamine. Näiteks arvesse võtta stsenaariumi, kuhu soovite lisada 1000 ridu. Järgmine tabel näitab, kui kaua võtab tabelväärtusega parameetrite abil saate lisada 1000 ridu kui jaotatud väiksemaks pakettidena.

| Paketi suurus | Iteratsiooni | Tabelis hinnatud parameetreid ([ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Tulemused ei ole kriteeriumid. Vt [Märkus ajastuse tulemused selle teema kohta](#note-about-timing-results-in-this-topic).

Näete, et parima jõudluse 1000 read on esitada need kõik korraga. Muude katsete (joonisel pole) oli väike jõudluse gain kahe ja vana 5000 10000 rea paketi murda. Kuid tabeli skeemi katsed on üsna lihtne, kontrollib tuleks sooritada oma konkreetsed andmed ja paketi suurused kinnitamaks, et need tulemused.

Teine tegur kaaluda on, et kui kokku paketi paisub liiga suureks, SQL-andmebaasi võib throttle ja kinnita paketi keelduda. Parimate tulemuste saamiseks Testige oma konkreetse stsenaariumi, kui on olemas ka paketi optimaalne suurus. Veenduge paketi suurus konfigureeritav käitusajal lubamiseks jõudluse või vigade põhjal kiiresti muuta.

Lõpuks saldo paketi suurust koos partiide seotud riske. Kui siirdamiseks vigu või roll nurjub, kaaluda tagajärgi toimingu uuesti proovimist või kaotada paketi andmed.

### <a name="parallel-processing"></a>Paralleelne töötlemine
Mida teha, kui võttis lähenemine paketi mahu vähendamine aga kasutada mitme lõime töö käivitada? Uuesti meie kontrollib ilmnes, et väiksemate multithreaded mitme töölehe tavaliselt läbi halvemaks ühe suurema paketi. Järgmised test püüab ühe või mitme paralleelse pakettidena 1000 ridade lisamine. Selle testi näitab, kuidas veel samaaegne pakettidena tegelikult jõudlus väheneb.

| Paketi suurus [iteratsiooni] | Kaks Teemad ([ms) | Nelja teemad ([ms) | Kuus teemad ([ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Tulemused ei ole kriteeriumid. Vt [Märkus ajastuse tulemused selle teema kohta](#note-about-timing-results-in-this-topic).

On mitu võimalikku põhjust tõttu paralleelsus jõudluse vähenemine.

- On mitme võrgu samaaegne kõned ühe asemel.
- Argument ja blokeerimine võib põhjustada mitme toimingute suhtes üheks tabeliks.
- Puuduvad seotud üldkulusid multithreading.
- Kulude avamine mitme ühendused ületab kasu paralleelne töötlemine.

Kui saate suunata erinevate tabelite või andmebaasidele, on võimalik mõned jõudluse saada selline strateegia kuvamiseks. Andmebaasi sharding või ühendused oleks seda moodust stsenaariumi. Sharding kasutab mitut andmebaasi ja marsruudib erinevaid andmeid iga andmebaasi. Kui mõni muu andmebaas saab iga väike paketi, siis toimingute paralleelselt saab tõhusam. Jõudluse gain pole oluline, andmebaasi sharding kasutamine teie lahendus otsustamiseks alusena kasutada.

Mõned kujundused paralleelne väiksem partii tulemuseks paranenud jõudlus taotluste süsteemi koormuse. Sel juhul on kiirem töötlemine ühe suurema paketi, mitu töölehte paralleelselt töötlemine võib tõhusamalt.

Kui kasutate paralleelne, kaaluge juhtimine töötaja teemad maksimaalne arv. Väiksemat võib kaasa tuua vähem väide ja kiirem täitmisaeg. Pidage silmas ka lisakoormust, mis see paneb nii ühendused ja tehingud Sihtandmebaas.

### <a name="related-performance-factors"></a>Seotud jõudlust mõjutavad tegurid
Tüüpilised juhised andmebaasi jõudlust mõjutab ka partiide. Lisage näiteks jõudluse on vähendatud tabelid, mis on suur primaarvõti või mitme nonclustered registrid.

Kui tabelis hinnatud parameetreid kasutada salvestatud protseduur, saate **määrata NOCOUNT ON** käsk protseduur alguses. See lause tõkestab tagastamise menetluse mõjutatud ridade arvu. Siiski meie katsed **seadmine NOCOUNT ON** kasutamine ei mõjuta või jõudluse vähenemine. Test, mis on salvestatud protseduur on lihtne ühe **Lisa** käsk tabelväljundiga parameeter. On võimalik, et keerukamate salvestatud toimingute saaksid selle lause. Kuid ei Oletagem, et **määrata NOCOUNT ON** lisamine oma salvestatud protseduur automaatselt parandab jõudlust. Mõistmaks efekti, Testige oma salvestatud protseduur ja ilma **seadmine NOCOUNT ON** avaldus.

## <a name="batching-scenarios"></a>Partiide stsenaariumid
Järgmistes jaotistes kirjeldatakse kolme rakenduse stsenaariumi puhul tabelis hinnatud parameetrite kasutamine. Esimene stsenaarium näitab, kuidas puhverdusvõime ja partiide saavad koos töötada. Teine stsenaarium parandab jõudlust ühe salvestatud protseduur kõne juhtslaidi-üksikasjalike toimingute tegemiseks. Lõplik stsenaarium näitab, kuidas kasutada tabelis hinnatud parameetreid toimingu "UPSERT".

### <a name="buffering"></a>Puhverdamine
Kuigi on mõned stsenaariumid, mis on selge testversiooni partiide, on palju stsenaariumi, mis võiks ära partiide, hilinemise töötlemine. Hilinenud töötlemine tegeleb ka suurem risk, et andmed on kadunud ootamatute tõrgete korral. See on oluline mõista selle riski ja kaaluda tagajärgi.

Oletame näiteks, veebirakenduse, mis jälgib iga kasutaja navigeerimise ajalugu. Iga lehe soovi korral rakenduse võiksid kõne salvestamise lehe vaate andmebaasi. Kuid suurem jõudlus ja skaleeritavus saavutamiseks puhverdusvõime kasutajate navigeerimise tegevuste ja seejärel saata andmed andmebaasi pakettidena. Võite käivitada andmebaasi värskenduse kulunud aeg ja/või puhvri suurus. Näiteks võib reegli määrata, et paketi töödeldakse pärast 20 sekundit või kui puhvri jõuab 1000 üksused.

Järgmine kood näide kasutab [Reaktiivne laiendid - Rx](https://msdn.microsoft.com/data/gg577609) puhverdatud tõstatatud jälgimisega seotud klassi sündmused. Kui puhvri täidab või ajalõpp on jõudnud, andmebaasi tabelis hinnatud parameetriga saadetakse paketi kasutaja andmed.

Järgmised NavHistoryData klassi mudelite kasutaja navigeerimise üksikasjad. See sisaldab põhiteave, nt kasutaja ID, URL-i juurde ja Accessi aeg.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Klassi NavHistoryDataMonitor vastutab puhverdusvõime kasutaja navigeerimise andmed andmebaasi. See sisaldab meetodi, RecordUserNavigationEntry, mis vastab tõstmise **OnAdded** sündmus. Järgmine kood kuvatakse ehitaja loogika, mis kasutab Rx märgata kogumine sündmuse põhjal luua. See siis tellib see jälgitav kogu puhvri meetod. Ülekoormuse saate määrata, et iga 20 sekundit või 1000 kirjed tuleb saata puhvri.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Funktsiooni sündmuseohjuri teisendab kõik puhverdatud üksused tabelis hinnatud tüüp ja seejärel seda tüüpi salvestatud protseduur, mis töötleb pakett. Järgmine kood kuvatakse täielik määratlus on NavHistoryDataEventArgs nii NavHistoryDataMonitor tunnid.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Selle puhverdamise klassi kasutamiseks rakendus loob staatilise NavHistoryDataMonitor objekti. Iga kord, kui kasutaja pääseb juurde lehel rakendus nõuab NavHistoryDataMonitor.RecordUserNavigationEntry meetod. Puhverdamise loogika tulu need kirjed saata andmebaasi pakettidena eest.

### <a name="master-detail"></a>Juhtslaidi üksikasjad
Tabelis hinnatud parameetrid on abiks lihtsa lisa stsenaariumid. Siiski võib olla keerulisem paketi lisab, et kaasata mitu tabelit. "Juhi/üksikasja" stsenaarium on hea näide. Juhtslaidi tabelis on näidatud esmase olemi. Ühe või mitme üksikasjalike tabelites on talletatud üksus kohta rohkem andmeid. Selle stsenaariumi korral võõrkeelsed olulised seosed Jõusta üksikasjad kordumatu juhtslaidi üksusele seost. Kaaluge võimalust oma seostuvat OrderDetail tabelit ja PurchaseOrder tabeli lihtsustatud versioon. Transact-SQL loob PurchaseOrder tabeli neli veergu: TellimuseID, TellimuseKuupäev, tellijaid ja olek.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Iga tellimus sisaldab ühe või mitme toote ostud. See teave on jäädvustatud PurchaseOrderDetail tabel. Transact-SQL loob PurchaseOrderDetail tabeli viis veergu: TellimuseID, OrderDetailID, ProductID, ühikuhind ja OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

Tabeli PurchaseOrderDetail TellimuseID veerus peab viidata tellimuse PurchaseOrder tabelist. Võõrvõti järgmised määratlus jõustab see piirang.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Tabelis hinnatud parameetreid kasutamiseks peab teil olema üks iga sihttabelisse tabeli kasutaja määratletud tüüp.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Määratlege salvestatud toiming, mida aktsepteerib järgmist tüüpi tabeleid. Selle toimingu rakendusel kohalikult partii tellimused ja Tellimuse üksikasjad ühe kõne. Transact-SQL pakub täielikku salvestatud protseduur deklareerimise ostu näites.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

Selles näites kohalikult määratletud @IdentityLink tabeli talletab TellimuseID tegelike väärtuste äsja lisatud ridadest. Nende järjestus identifikaatorid erinevad ajutine TellimuseID väärtused on @orders ja @details tabelis hinnatud parameetreid. Seetõttu on @IdentityLink tabeli seejärel loob sellega ühenduse TellimuseID väärtused on @orders TellimuseID tegeliku väärtuse jaoks uued read tabelis PurchaseOrder-parameeter. Selles etapis tuleb pärast selle @IdentityLink tabel aitab lisamise Tellimuse üksikasjad, kus tegelik TellimuseID, mis vastab võõrkeelse võtme piirangu.

See salvestatud protseduur saab koodi või muude Transact-SQL-i kõnede kaudu. Selles näites kood paberi jaotisest tabelis hinnatud parameetreid. Transact-SQL näitab, kuidas funktsiooni sp_InsertOrdersBatch helistada.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Lahendus võimaldab iga paketi kasutada TellimuseID väärtused, mis algavad 1. Nende ajutine TellimuseID väärtuste kirjeldatakse paketi seoseid, kuid tegelik TellimuseID väärtused määratakse ajal lisa toiming. Saate käivitada sama laused eelmises näites korduvalt ja kordumatute tellimuste andmebaasi loomiseks. Seetõttu soovi korral lisada rohkem koodi või andmebaasi loogika, mis takistab dubleeritud tellimused, kasutades seda partiide meetod.

Selles näites näitab, et isegi keerukamaid andmebaasi toiminguid, näiteks põhi-üksikasjalike toimingute, saate batched tabelis hinnatud parameetritega.

### <a name="upsert"></a>UPSERT
Teine partiide stsenaarium hõlmab korraga värskendamine olemasolevate ridade ja lisada uued read. See toiming on mõnikord edaspidi toimingu "UPSERT" (Värskenda + lisa). Asemel saate lisada ja värskendada eraldi helistamist, on kõige paremini selle toimingu Ühenda lause. Kirjakooste aruande saate teha nii lisamine ja värskendamine toimingute ühe kõne.

Kirjakooste aruande saab teha värskendused ja lisab tabelis hinnatud parameetreid. Oletame näiteks, lihtsustatud töötaja tabeli, mis sisaldab järgmisi veerge: Tabeliseose, eesnimi, perekonnanimi, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
Selle näite puhul saate kasutada funktsiooni SocialSecurityNumber on kordumatu ühendada mitu töötajate. Esmalt looge tabeli kasutaja määratletud tüüp:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Järgmiseks luua salvestatud toimingu või kirjutada koodi, mis kasutab kirjakooste aruande värskendamine ja lisada. Järgmises näites kasutatakse kirjakooste aruande tabelis hinnatud parameetrit @employees, tüüpi EmployeeTableType. Sisu on @employees tabel on näidatud siin.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Lisateabe saamiseks vt dokumentatsiooni ja Ühenda lause näited. Kuigi võiks sama tööd teha mitme etapiga salvestatud protseduur kõne eraldi lisa ja UPDATE toimingud, Ühenda lause on tõhusam. Andmebaasi koodi saate luua ka Transact-SQL-i kõned, mida kasutada kirjakooste aruande otse nõudmata kaks andmebaasi kõned lisamine ja värskendamine.

## <a name="recommendation-summary"></a>Soovitus Kokkuvõte

Järgnev loend annab ülevaate selles teemas kirjeldatud partiide soovitusi:

- Kasutage puhverdusvõime ja partiide jõudlus ja skaleeritavus SQL-andmebaasi rakenduste suurendamiseks.
- Mõista kompromissidega partiide/puhverdamine ja paindlikkust vahel. Roll ajal, võib riski kaotada mõne töötlemata paketi business kriitiliste andmete üles jõudluse huvides partiide.
- Üritab hoida kõnede vähendada latentsus andmebaasi ühe andmekeskuse sees.
- Kui valite ühe partiide tehnika, tabelis hinnatud parameetreid pakkuda head jõudlust ja paindlikkust.
- Parima jõudluse lisa, järgige järgmisi üldiseid juhiseid, kuid testimiseks stsenaariumist:
    - < 100 read, kasutage ühte parameetriga käsku Lisa.
    - < 1000 read, kasutage tabelis hinnatud parameetreid.
    - Jaoks > = 1000 read, kasutage SqlBulkCopy.
- Jaoks värskendada ja kustutada, tabelväärtusega parameetrite abil salvestatud protseduur loogika, mis määratleb töö parameetri tabeli iga rea kohta.
- Paketi suurus juhised:
    - Saate kasutada suurima paketi suurused jaoks teie taotlus ja ettevõtte nõuetele.
    - Saldo jõudluse kasum suurte pakettidena ajutised või katastroofiline tõrgete ohtu. Mis on selle tulemusena korduskatsed või andmete paketi? 
    - Testige suurim suurusega SQL-andmebaasi kõnest selle kinnitamiseks.
    - Looge konfiguratsioonisätted selle juhtelemendi partiide, nt partii suurust või puhverdamise ajaakna. Nende sätete paindlikkuse. Saate muuta partiide käitumine valmistamisel ilma ümbersuunamine pilveteenusesse.
- Vältige paralleelne, töölehti, mis töötavad ühe tabeli ühe andmebaasi. Kui otsustate jagada ühe paketi üle mitme töötaja teemad, käivitage kontrollib optimaalne arvu määramiseks. Pärast on määramata lävi veel Teemad jõudluse pigem väheneb kui suurendada.
- Kaaluge puhverdusvõime suurus ja ajal, kui viis, kuidas veel stsenaariumid partiide kohta.

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis värskendustest kohta, kuidas andmebaasi kujunduse ja kodeerimise seotud partiide saate parandada oma rakenduse jõudlus ja skaleeritavus. Kuid see on ainult üks tegur üldine strateegia kavandamine. Rohkem võimalusi parandada jõudlus ja skaleeritavus, leiate [Azure'i SQL-andmebaasi jõudluse juhiseid ühe andmebaasid](sql-database-performance-guidance.md) ja [hinna ja jõudluse kaalutluste kohta kohapeal on elastne andmebaasi](sql-database-elastic-pool-guidance.md).
