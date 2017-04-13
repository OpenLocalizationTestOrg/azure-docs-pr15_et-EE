<properties
    pageTitle="SQL-i ühenduse tõrge, siirdamiseks tõrke lahendamiseks | Microsoft Azure'i"
    description="Saate teada, kuidas tõrkeotsing, diagnoosimine ja vältida siirdamiseks tõrge Azure SQL andmebaasis või SQL-i ühenduse tõrge. "
    keywords="SQL-i ühendus ühendusstring, ühenduvusprobleemide, siirdamiseks tõrge, ühenduse tõrge"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Tõrkeotsing, diagnoosimine ja SQL-i andmebaasi SQL-i ühenduse tõrgete ja siirdamiseks tõrgete vältimiseks

Selles artiklis kirjeldatakse, kuidas vältida, tõrkeotsing, diagnoosimine ja ühenduse tõrgete ja siirdamiseks tõrgete klientrakenduse tekib siis, kui see suhtleb Azure'i SQL-andmebaasi leevendada. Saate teada, kuidas konfigureerida uuesti loogika, koostada ühendusstringi ja muude ühenduse sätete muutmine.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Siirdamiseks tõrgete (ajutine vigade)

Siirdamiseks tõrge – ka siirdamiseks vea - on põhjuseks mis lahendab kiiresti ise. On aeg-ajalt põhjustab siirdamiseks vigu on kui Azure süsteemi kiiresti Nihutab ressursid parem laadi – Topeltdegressiivse erinevate töökoormus. Kõige nende ümberkonfigureerimine sündmuste täielik sageli vähem kui 60 sekundi järel. Ümberkonfigureerimine aja jooksul, peate ühenduvusprobleemide Azure SQL-andmebaasiga. Rakenduste Azure SQL-i andmebaasiga ühenduse loomisel peaks ehitatud nende siirdamiseks vigade oodata, teeb nendega uuesti loogika rakendades oma koodi asemel kaetud need kasutajad rakenduse tõrked.

Kui teie klient programm kasutab ADO.net-i, räägitakse programmi, mis **SqlException**visata siirdamiseks tõrke kohta. Atribuudi **arvu** saate võrreldakse teema ülaosas siirdamiseks tõrgete loendit: [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Ühenduse võrreldes käsk

Saate proovige ühenduse SQL-i või luua selle uuesti, olenevalt järgmist:

* **Siirdamiseks tõrge ilmneb proovige ühendus**: ühenduse tuleb uuesti proovida pärast mitu sekundit edasi lükata.

* **Siirdamiseks tõrge ilmneb SQL-päringu käsu**: käsk peaks uuesti kohe proovida. Selle asemel pärast viivitust ühenduse värskelt tuleks. Seejärel saate käsu uuesti proovida.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Uuestiproovimise loogikale siirdamiseks tõrkeid


Klientprogrammide, mis aeg-ajalt ilmneda siirdamiseks tõrge on tugevam, kui need sisaldavad uuesti loogika.


Kui teie programmi suhtleb Azure'i SQL-andmebaasi kuni 3 tootja vahevara, päringu hankija, kas selle vahevara sisaldab proovi uuesti loogika siirdamiseks tõrkeid.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Proovi uuesti põhimõtted


- Ühenduse loomine katse peaks uuesti proovida, kui tõrke on ajutine.


- Valige SQL-i lause, mis nurjub siirdamiseks viga peaks olema otse uuesti proovida.
 - Selle asemel värske ühendust luua, ja proovige uuesti valimine.


- Kui SQL-i UPDATE-lause nurjub siirdamiseks viga, tuleks enne värskenduse uuesti proovida uue ühenduse.
 - Proovi uuesti loogika, veenduge, et kogu andmebaasitehinguna, mis on lõpetatud või mis kogu tehing on tagasi pöörata.


#### <a name="other-considerations-for-retry"></a>Muude kaalutluste kohta proovi uuesti


- Paketi programmi, mis käivitatakse automaatselt pärast töötundide ja mis on lõpule jõuaks, enne hommikul saate endale väga kaua aega vahedega vahel selle proovi uuesti üritab patsientide.


- Et anda pärast liiga pikk ootama inimeste suuremas arvestama kasutaja kasutajaliidese programmi.
 - Siiski lahendus peab olema uuesti iga paari sekundi järel, kuna poliitika saate üleujutuste taotlusi süsteem.


#### <a name="interval-increase-between-retries"></a>Korduskatsed vaheline intervall kasv



Soovitame, et te viivitus 5 sekundit enne oma esimese proovi uuesti. Proovitakse uuesti pärast lühem sekundi riskid suur pilveteenusesse viivitust. Jaoks iga järgmise proovi uuesti viivituse peaks kasvada, kuni 60 sekundi järel.

Arutelu *blokeerimine perioodi* klientides ADO.net-i jaoks on saadaval [SQL Serveri ühenduse ühiskasutus (ADO.net-i)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Samuti võite seadmine suurim lubatud arv korduskatsed enne programmi omas lõpeb.


#### <a name="code-samples-with-retry-logic"></a>Proovi uuesti loogika koodinäiteid


Proovi uuesti loogika, erinevaid programmeerimise keeled, koodinäiteid on saadaval.

- [Andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testige oma proovi uuesti loogika


Testige oma proovi uuesti loogikat, peate simuleerida või põhjustada tõrke, kui saate parandada oma programmi käigus on.


##### <a name="test-by-disconnecting-from-the-network"></a>Katsetage võrgu ühenduse katkestamine


Üks võimalus saate testida oma proovi uuesti loogika on võrgust teie klientarvuti programmi töötamise ajal. Viga on järgmised:

- **SqlException.Number** = 11001
- Teade: "sellist hosti on tuntud"


Proovi uuesti katsel osana programmi saate vea valesti kirjutatud sõna ja seejärel proovite ühendust.


Et muuta see on praktiline, eraldate arvuti võrgust enne alustamist programmi. Klõpsake oma programm tuvastab käitusaja parameeter, mis põhjustab programmi:

1. Ajutiselt lisada 11001 selle nagu mööduv silmas pidada tõrgete loend.
2. Proovige oma esimese ühenduse nagu tavaliselt.
3. Pärast tõrke püütakse, 11001 loendist eemaldada.
4. Kuva teadet kasutajal palutakse ühendage arvuti võrku.
 - Viige hiirekursor täpsemaks täitmise kasutades kas **Console.ReadLine** meetodit või dialoogiboksis nuppu OK. Kostaks sisestusklahvi (Enter) pärast arvuti on võrku ühendatud.
5. Proovige uuesti ühendust luua, edu ootate.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Katsetage õigekirjaviga andmebaasi nime, kui ühendate


Teie programmi saate tahtlikult kirjutate enne esimest ühenduse loomise katse kasutajanimi. Viga on järgmised:

- **SqlException.Number** = 18456
- Teade: "Logi sisse kasutaja"WRONG_MyUserName"nurjus."


Proovi uuesti katsel osana programmi saate vea valesti kirjutatud sõna ja seejärel proovite ühendust.


Et muuta see on praktiline, võib teie programmi tuvasta käitusaja parameeter, mis põhjustab programmi:

1. Ajutiselt lisada 18456 selle nagu mööduv silmas pidada tõrgete loend.
2. Tahtlikult lisada 'WRONG_' kasutajanimi.
3. Pärast tõrke püütakse, 18456 loendist eemaldada.
4. Eemaldage 'WRONG_' kasutajanimi.
5. Proovige uuesti ühendust luua, edu ootate.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Ühenduse uuesti .NET SqlConnection parameetrid


Kui teie klient programmi seotud Azure SQL-andmebaasiga, kasutades .NET Frameworki klassi **System.Data.SqlClient.SqlConnection**, kasutage .NET 4.6.1 või uuem versioon nii, et saate kasutada oma ühenduse uuesti funktsioon. Funktsiooni üksikasjad on [siin](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Kui koostate [ühendusstringi](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) oma **SqlConnection** objekti, mida peaks koordineerimine vahel järgmiste parameetrite väärtused:

- ConnectRetryCount &nbsp; &nbsp; *(vaikeväärtus on 1. Vahemik on 0 kuni 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(vaikeväärtus on 1 teise. Vahemik on 1 kuni 60.)*
- Ühenduse ajalõpp &nbsp; &nbsp; *(vaikimisi 15 minutit. Vahemik on 0 kuni 2147483647)*


Täpsemalt teie valitud väärtused peaksid olema järgmised võrdse true:

- Ühenduse ajalõpp = ConnectRetryCount * ConnectionRetryInterval

Näiteks kui arvu = 3 ja intervalli = 10 sekundi, ajalõpp, ainult 29 sekundit ei tundu hästi annaks süsteemi piisavalt aega oma 3 ja lõplik uuesti veebisaidil ühenduse: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Ühenduse võrreldes käsk


Parameetrite **ConnectRetryCount** ja **ConnectRetryInterval** lasta toimingut ühenduse loomine ilma teade või häirib oma programm, näiteks juhtelemendi naasevad programmi **SqlConnection** objekti. Funktsiooni korduskatsed võib ilmneda järgmistel juhtudel:

- kõne mySqlConnection.Open meetod
- kõne mySqlConnection.Execute meetod

Ei saanud teravmeelsus. Kui siirdamiseks tõrge ilmneb juhul, kui *päring* käivitatakse, **SqlConnection** objekti ei toimingut ühenduse loomine ja selle kindlasti uuesti oma päringu. **SqlConnection** väga kiiresti kontrolli enne saatmist päringu täitmise ühendus. Kiire kontroll tuvastab Interneti-ühenduse probleemi **SqlConnection** uued katsed ühendamine toiming. Kui uuesti õnnestub, saadetakse teile päringu täitmise.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Tuleks ConnectRetryCount kombineerida rakenduse uuesti loogika?

Oletame, et teie taotlus on töökindlad kohandatud uuesti loogika. See võib toimingut korrata ühendamine 4 korda. Kui lisate **ConnectRetryInterval** ja **ConnectRetryCount** = 3 oma ühendusstring, et te suurendada uuesti arvu 4 * 3 = 12 korduskatsed. Kavatsete võib suure arvu korduste arv.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Ühenduste Azure SQL-andmebaasiga

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Ühendus: Ühendusstringi


Azure'i SQL-andmebaasi liitumiseks vajaliku ühendusstring on veidi teistsugune Microsoft SQL Serveri ühenduse string. Saate kopeerida andmebaasi [Azure portaali](https://portal.azure.com/)ühendusstring.


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Ühendus: IP-aadress


SQL-andmebaasi server arvuti, mis majutab programmi kliendi IP-aadress teatis kinnitamiseks tuleb konfigureerida. Tehke seda, redigeerides tulemüürisätteid [Azure portaali](https://portal.azure.com/)kaudu.


Kui unustate konfigureerimiseks IP-aadress, nurjub programmi abil on mugav sõnumis vajalikud IP-aadress.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Lisateavet leiate teemast: [kohta: tulemüüri sätete konfigureerimine SQL-andmebaasi](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Ühendus: pordid


Tavaliselt peate ainult veenduge, et port 1433 on avatud väljaminev suhtlemiseks klientprogrammi majutavas arvutis.


Näiteks kui teie klient programm on majutatud Windowsiga arvutist, Windowsi tulemüür hosti võimaldab teil port 1433 avamiseks.


1. Avage juhtpaneel
2. &gt;Kõik Control Panel kirjed
3. &gt;Windowsi tulemüüri
4. &gt;Täpsemad sätted
5. &gt;Väljamineva reeglid
6. &gt;Toimingud
7. &gt;Uus reegel


Kui teie klient programm on majutatud Azure virtuaalse masina (VM), peaksite lugema:<br/>[Lisaks 1433 jaoks ADO.NET 4.5 pordid ja SQL-i andmebaasi V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Lisateavet tausta cofiguration pordid ja IP-aadress, leiate teemast: [Azure'i SQL-andmebaasi tulemüüri](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Ühendus: 4.6.1 ADO.net-i


Kui teie programmi kasutab ADO.net-i tunnid nagu **System.Data.SqlClient.SqlConnection** Azure SQL-i andmebaasiga ühenduse loomiseks, soovitame kasutada .NET Framework 4.6.1 versioon või uuem versioon.


ADO.NET-I 4.6.1:

- Azure'i SQL-andmebaasi on täiustatud töökindlus ühenduse avamisel **SqlConnection.Open** meetodi abil. Nüüd sisaldab **avatud** parim peegeldav uuesti menetlustele vastuse siirdamiseks vead, teatud tõrgete ühenduse ajalõpu jooksul.
- Toetab ühenduse ühiskasutus. See hõlmab tõhusa kontrolli, mis pakub teie programmi ühenduse objekti toimib.



Ühenduse objekti kasutamisel kaustast ühendus, soovitame programmi ajutiselt sulgeda ühenduse kui kohe ei kasuta. Ühenduse uuesti avamine ei maksa palju on uue ühenduse loomise viisi.


Kui kasutate ADO.NET 4.0 või mõnes varasemas versioonis, soovitame versiooniks uusima ADO.net-i.

- November 2015, saate [alla laadida ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostika

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostika: Utiliidid saate luua ühenduse testimiseks


Kui teie programmi ei ole Azure SQL-i andmebaasiga ühenduse loomiseks, diagnostika üheks võimaluseks on proovida kasuliku programmi suhtlemiseks. Ideaalvariandis mitte kasuliku oleks ühendada sama teek, mis kasutab teie programmi abil.


Windowsi arvutis, võite proovida järgmisi Utiliidid.

- SQL Server Management Studio (ssms.exe), mis ühendab ADO.net-i abil.
- sqlcmd.exe, mis ühendab [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)abil.


Kui ühendus on loodud, kontrollige, kas lühike valige SQL-i päring töötab.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostika: Märkige ruut avatud pordid


Oletame, et kahtlustate, et ühenduse katsete ei suuda tõttu pordi probleemid. Teie arvutis saab käitada kasuliku, mis on portide aruanded.


Linux järgmised Utiliidid võib olla kasulik.

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (IP-aadress olevat väärtust näide muuta).


Opsüsteemis Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) kasuliku võib olla kasulik. Siin on näide täitmise, mis esitatakse selle kohta päring Azure'i SQL-andmebaasi serveri pordi olukorda ja mis on käivitada sülearvutis.


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostika: Logige sisse oma tõrked


Mõne vahelduva probleem on mõnikord kõige paremini diagnoosida tuvastamise üldine mustri päeva või nädala jooksul.


Teie klient võib aidata on diagnoosimise Facebooki jõutakse kõik vead. Võimalik, logi kannete vastavus Azure'i SQL-andmebaasi logib ise ettevõttesiseselt tõrge andmeid.


Ettevõtte Raamatukogu 6 (EntLib60) pakub .net-i hallatavate tunnid abistamiseks logimine:

- [5 – sama lihtne kui alla Logi välja: abil logimise rakenduse blokeerimine](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostika: Uurida süsteemi logid tõrkeid


Siin on mõned Transact-SQL-i valige laused selle päringu logid tõrke ja muud teavet.


| Päringu log | Kirjeldus |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Vaate [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) pakub teavet üksikud sündmused, mis võivad põhjustada tõrkeid siirdamiseks või ühenduvuse tõrkeid.<br/><br/>Ideaalvariandis mitte saate **start_time** või **end_time** väärtused oleksid, kui teie klient programmi kogenud probleemide kohta lisateabe saamiseks.<br/><br/>**Näpunäide:** **Juhtslaidi** andmebaasi selle käivitamiseks tuleb ühendada. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Vaate [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) pakub liidetud loendab sündmus tüüpi täiendavad diagnostika.<br/><br/>**Näpunäide:** **Juhtslaidi** andmebaasi selle käivitamiseks tuleb ühendada. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostika: Otsimine probleemi sündmuste logi SQL-andmebaas


Saate otsida kirjed probleemi sündmuste logi Azure'i SQL-andmebaasi kohta. Proovige järgmine Transact-SQL-i valige lause **põhi** andmebaasi.


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Mõned tagastatud ridade sys.fn_xe_telemetry_blob_target_read_file


Järgmine on tagastatud rea võib välja näha umbes. Tühiväärtuste, mis on kuvatud pole sageli teistes ridades null.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Ettevõtte Raamatukogu 6


Ettevõtte Raamatukogu 6 (EntLib60) on raamistiku .NET tunde, mis aitab teil rakendada robustne kliendid pilveteenuste, millest üks on Azure'i SQL-andmebaasi teenuse. Eesmärk on jätkuvalt igale alale, mis EntLib60 aitavad esimese külastades, teemade otsimiseks:

- [Ettevõtte Raamatukogu 6 – aprillis 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Proovi uuesti loogika käsitlemise siirdamiseks tõrgete on ühe ala, kus saate abistamine EntLib60:

- [4 - visadust, salajane kõik triumphs: abil siirdamiseks viga töötlemine rakenduse blokeerimine](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Lähtekoodi EntLib60 on saadaval avalike [alla laadida](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft ei ole kavas teha täiendavaid funktsioon värskendusi või hooldustööd värskendusi EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 tunnid siirdamiseks vead ja proovige uuesti


Järgmised EntLib60 klassid on eriti kasulik uuesti loogika. Kõik need on või on veelgi, nimeruumi **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**.

*Nimeruumi* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- Klassi **RetryPolicy**
 - **ExecuteAction** meetod


- Klassi **ExponentialBackoff**


- Klassi **SqlDatabaseTransientErrorDetectionStrategy**


- Klassi **ReliableSqlConnection**
 - **ExecuteCommand** meetod


Nimeruumi **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- Klassi **AlwaysTransientErrorDetectionStrategy**

- Klassi **NeverTransientErrorDetectionStrategy**


Siin on lingid teabele EntLib60 kohta.

- Tasuta [broneerida allalaadimine: arendaja juhend Microsoft Enterprise teeki, 2. väljaanne](http://www.microsoft.com/download/details.aspx?id=41145)

- Head tavad: [proovige üldised juhised](../best-practices-retry-general.md) on suurepärane põhjalik arutelu proovi uuesti loogikat.

- [Ettevõtte](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) Raamatukogu - siirdamiseks viga töötlemise rakenduse blokeerimine 6.0 Nugeti allalaadimine

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Logimine blokeerimine


- Blokeeri logimine on väga paindlik ja konfigureeritav lahenduse, mis võimaldab teil:
 - Luua ja talletada Logi sõnumid mitmesuguseid asukohad.
 - Kategoriseerida ja filtreerida.
 - Koguda kontekstipõhine teave, mis on kasulik silumine ja jälitamine, samuti auditeerimine ja üldine logimine nõuetele.


- Logimise Blokeeri abstracts logimise funktsiooni log sihtkohta, et rakenduse koodi vastab sõltumata asukoht ja tüüpi target logimine pood.


Üksikasju vt: [5 - nimega lihtne nagu alla välja toodud Logi: abil logimise rakenduse blokeerimine](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>Lähtekoodi EntLib60 IsTransient meetod


Järgmiseks on **SqlDatabaseTransientErrorDetectionStrategy** ainekursuse, C# lähtekoodi **IsTransient** meetod. Lähtekoodi selgitatakse, millised tõrgete peetud siirdamiseks ja väärt uuesti, aprillis 2013.

Mitme **//comment** read on eemaldatud selle koopia loetavuse rõhutada.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Järgmised sammud

- Azure'i SQL-andmebaasi ühendus muude levinud probleemide tõrkeotsinguks külastage [Azure SQL-andmebaasiga ühenduse tõrkeotsing](sql-database-troubleshoot-common-connection-issues.md).

- [SQL serveri Ühendusepark (ADO.net-i)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Retrying* on mõne Apache 2.0 litsentsitud üldotstarbeline proovin uuesti teek, mis on kirjutatud **Python**, lihtsustada tööülesande uuesti käitumine lihtsalt midagi lisada.](https://pypi.python.org/pypi/retrying)
