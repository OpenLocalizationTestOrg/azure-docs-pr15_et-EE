<properties
    pageTitle="Seadme õ Studio online andmeallikast pärinevate andmete importimine | Microsoft Azure'i"
    description="Kuidas importida koolitus andmete Azure seadme õ Studio online erinevatest allikatest."
    keywords="andmete vormindamine, andmetüübid, andmeallikate, koolitus andmete importimine"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Andmete importimine Azure seadme õ Studio online andmeid erinevatest allikatest mooduli andmete importimine

Selles artiklis kirjeldatakse tugi võrgus andmete importimine erinevatest allikatest ja liikuda andmeid neist allikatest Azure seadme õ katse vajalik teave.

> [AZURE.NOTE] Selles artiklis kirjeldatakse [Andmete importimine] [ import-data] mooduli. Üksikasjalikumat teavet pääsete andmetüübid vormingud, parameetrid ja vastused tavalisematele küsimustele, lugege teemat mooduli viide, [Andmete importimine] [ import-data] mooduli.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Sissejuhatus

Pääsete Azure seadme õ Studio andmeid mitu online andmeallikat oma katse töötamise abil [Andmete importimine] [ import-data] mooduli:

- Web URL-i HTTP kaudu
- Hadoopi HiveQL abil
- Azure'i bloobimälu
- Azure'i tabeli
- Azure'i SQL-andmebaasi või SQL Server Azure'i VM
- Asutusesisese SQL Serveri andmebaasiga
- Pakkuja praegu OData andmekanalist
 
Tegemise katsete Azure seadme õ Studio töövoo koosneb lohistades-langetamine komponendid peale lõuend. Veebis andmeallikatele juurdepääsu lisada [Andmete importimine] [ import-data] oma katse mooduli **andmeallika**valimine ja seejärel andke andmetele juurdepääsemiseks vajalikud parameetrid. Järgmises tabelis on loetletud online andmeallikad, mis on toetatud. Selles tabelis on kokkuvõte ka toetatud failivormingud ja andmetele juurdepääsemiseks kasutatavate parameetrite.

Pange tähele, et kuna koolitus andmed on juurdepääs teie katse töötamise, see on saadaval ainult katse. Võrdluse kaudu andmekomplekti moodulis talletatud andmed on saadaval katse oma tööruumis.

> [AZURE.IMPORTANT] Praegu [Andmete importimine] [ import-data] ja [Andmete eksportimine] [ export-data] moodulid saab andmete lugemine ja kirjutamine ainult klassikaline juurutamise mudeli abil loodud Azure hoidlast. Teisisõnu, uue Azure'i bloobimälu konto tüüp, mis pakub kuum salvestusruumi Accessi taseme või lahedad salvestusruumi Accessi taseme veel ei toetata. 

> Üldiselt kontodest Azure storage ei tohiks mõjutada võib loonud enne, kui see suvand saadaval. Kui soovite luua uue konto, valige **klassikaline** juurutamise mudeli, või kasutada ressursihaldur ja **konto tüüpi**, valige **Üldine otstarve** , mitte **bloobimälu**. 

> Lisateavet leiate teemast [Azure'i bloobimälu: kuum ja lahedad Storage astme](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Toetatud andmeallikate veebis
Azure'i õ arvuti **Impordi andmed** mooduli toetab järgmiste andmeallikatega:

Andmeallikas | Kirjeldus | Parameetrid |
---|---|---|
Veebisaidi URL HTTP kaudu |Loeb andmeid komaga eraldatud väärtuste (CSV), tabeleraldusega väärtuste (TSV), atribuut-suhte failivorming (Hädaabiteabe) ja tugiteenuste vektorkuju masinad (SVM – hele) vormingus, mis tahes web URL-i, mis kasutab HTTP kaudu | <b>URL</b>: saate määrata, sh saidi URL ja faili nimi, mis tahes laiendiga faili täielik nimi. <br/><br/><b>Andmete vormindamine</b>: saate määrata ühe toetatud andmete vormingud: CSV, TSV, Hädaabiteabe või SVM-lihtversioon. Kui andmed on päiserida, kasutatakse selle määrata veergude nimed.|
Hadoopi/HDFS|Hadoopi jaotatud talletusruumi loeb andmeid. Määrake soovitud HiveQL, SQL-like päringukeele abil andmeid. Andmeid ja andmete filtreerimise enne andmete lisamine seadme õ Studio tegemiseks saate kasutada ka HiveQL.|<b>Andmebaasi omapäringute taru</b>: määrab taru päringu abil saate luua andmed.<br/><br/><b>HCatalog serveri URI</b> : määratud nimi, kasutades vormingut *klaster &lt;oma kobar nime&gt;. azurehdinsight.net.*<br/><br/><b>Hadoopi konto kasutajanimi</b>: määrab kasutatud ettevalmistamise klaster Hadoopi kasutajanime.<br/><br/><b>Hadoopi kasutajakonto parooli</b> : saate määrata, kui ettevalmistamise klaster mandaadid. Lisateabe saamiseks vt [loomine Hadoopi kogumite Hdinsightiga sisse](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Väljundi andmete asukoht</b>: saate määrata, kas andmed on salvestatud Hadoopi fail hajusfailisüsteemiga (HDFS) või Azure. <br/><ul>Kui salvestate väljundi andmete HDFS, määrake HDFS serveri URI. (Kasutage kindlasti Hdinsightiga kobar nimi ilma eesliide HTTPS://). <br/><br/>Kui salvestate oma väljundi andmete Azure, määrake Azure'i salvestusruumikonto nimi, salvestusruumi kiirklahv ja salvestusruumi container nimi.</ul>|
SQL-andmebaas |Loeb andmeid, mis on talletatud Azure SQL andmebaasis või SQL serveri andmebaasi Azure virtuaalne arvutis töötab. | <b>Andmebaasiserveri nimi</b>: määrab nime andmebaasi serveris.<br/><ul>Azure'i SQL-andmebaasi korral sisestage serveri nimi, mis on loodud. See on tavaliselt vormi * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>SQL server Azure'i virtuaalse arvutisse majutatud korral sisestage *tcp:&lt;virtuaalse masina DNS-i nimi&gt;, 1433*</ul><br/><b>Andmebaasi nimi </b>: saate määrata andmebaasi nimi serveris. <br/><br/><b>Serveri konto kasutajanimi</b>: määrab kasutaja kontoga, millel on juurdepääsuõigused andmebaasi nimi. <br/><br/><b>Kasutajakonto parooli server</b>: kasutaja parooli saate määrata.<br/><br/><b>Mis tahes serveri serdi aktsepteerida</b>: Kasutage seda suvandit (vähem turvaline), kui soovite vahele läbivaatamise saidi serdi enne oma andmeid lugeda.<br/><br/><b>Andmebaasi omapäringute</b>: sisestage SQL-lause, mis kirjeldab andmeid, mida soovite lugeda.|
Asutusesisese SQL-andmebaas |Loeb andmeid, mis on talletatud asutusesisese SQL-andmebaasis. | <b>Andmete lüüsi</b>: määrab Andmehalduslüüsi installitud arvutisse, kus see on juurdepääs SQL serveri andmebaasi nime. Lüüsi häälestamise kohta leiate teavet teemast [Perform Täpsemad analytics Azure seadme õ abil andmete asutusesisese SQL Serverist](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Andmebaasiserveri nimi</b>: määrab nime andmebaasi serveris.<br/><br/><b>Andmebaasi nimi </b>: saate määrata andmebaasi nimi serveris. <br/><br/><b>Serveri konto kasutajanimi</b>: määrab kasutaja kontoga, millel on juurdepääsuõigused andmebaasi nimi. <br/><br/><b>Kasutajanime ja parooli</b>: andmebaasi mandaadi sisestamiseks klõpsake <b>Sisestage väärtused</b> . Saate kasutada Windowsi integreeritud autentimist või SQL serveri autentimine, sõltuvalt sellest, kuidas teie asutusesisese SQL Server on konfigureeritud.<br/><br/><b>Andmebaasi omapäringute</b>: sisestage SQL-lause, mis kirjeldab andmeid, mida soovite lugeda.|
Azure'i tabeli|Loeb Azure Storage teenusest tabeli andmeid.<br/><br/>Kui loete suurte andmehulkade harva, Azure'i tabeli teenust kasutada. Paindlik, pakub-relatsiooniline (NoSQL), oluliselt scalable, odav ja väga kättesaadav salvestamise lahendus.| **Andmete importimine** suvandid muuta sõltuvalt sellest, kas kasutate avaliku teabe või privaatne salvestusruumi konto, mis nõuab sisselogimisteave. Seda määrab <b>Autentimistüüp</b> mis võib olla väärtus "PublicOrSAS" või "Konto", mis on oma seatud parameetrite. <br/><br/><b>Avalik või ühiskasutusse antud juurdepääs allkirja (SAS) URI</b>: parameetrid on:<br/><br/><ul><b>Tabeli URI</b>: määrab avalik või SAS URL-i tabeli.<br/><br/><b>Määrab read skannimiseks atribuutide nimesid</b>: väärtused on <i>TopN</i> määratud arvu ridade haaramiseks või <i>ScanAll</i> saada kõik read tabelis. <br/><br/>Kui andmed on ühtlane ja hõlpsalt prognoosida, on soovitatav valige *TopN* ja sisestage number, N. Suured tabelid, see võib põhjustada kiiremini lugemine korda.<br/><br/>Kui andmed on struktureeritud koos komplekti atribuute, et erinevad sügavus ja asendit, valige *ScanAll* suvandi kõik read skannimiseks. See tagab tagamine teie tulemuseks atribuudi ja metaandmete teisendus.<br/><br/></ul><b>Privaatne salvestusruumi konto</b>: parameetrid on: <br/><br/><ul><b>Konto nimi</b>: saate määrata tabelis, et lugeda sisaldava konto nimi.<br/><br/><b>Konto võti</b>: määrab kontoga seotud salvestusruumi võti.<br/><br/><b>Tabeli nimi</b> : määrab lugeda andmeid sisaldava tabeli nimi.<br/><br/><b>Ridade skannimiseks atribuutide nimesid</b>: väärtused on <i>TopN</i> määratud arvu ridade haaramiseks või <i>ScanAll</i> saada kõik read tabelis.<br/><br/>Kui andmed on ühtlane ja hõlpsalt prognoosida, soovitame teil valige *TopN* ja sisestage number, N. Suured tabelid, see võib põhjustada kiiremini lugemine korda.<br/><br/>Kui andmed on struktureeritud koos komplekti atribuute, et erinevad sügavus ja asendit, valige *ScanAll* suvandi kõik read skannimiseks. See tagab tagamine teie tulemuseks atribuudi ja metaandmete teisendus.<br/><br/>|
Azure'i bloobimälu | Loeb bloobimälu teenuses Azure Storage, sh pildid, struktureerimata tekst või binaarandmete talletatud andmed.<br/><br/>Saate bloobimälu teenuse andmete esitamist avalikult või privaatselt rakenduse andmete talletamiseks. Pääsete oma andmetele igalt poolt, kasutades HTTP- või HTTPS-ühendused. | **Andmete importimine** mooduli suvandid muuta sõltuvalt sellest, kas kasutate avaliku teabe või privaatne salvestusruumi konto, mis nõuab sisselogimisteave. Seda määrab <b>Autentimistüüp</b> , mis võib olla väärtust "PublicOrSAS" või "Konto".<br/><br/><b>Avalik või ühiskasutusse antud juurdepääs allkirja (SAS) URI</b>: parameetrid on:<br/><br/><ul><b>URI</b>: avalik või SAS URL-i määratledes salvestusruumi bloobimälu.<br/><br/><b>Failivorming</b>: saate määrata bloobimälu teenuse andmete vorming. Toetatud vormingud on CSV, TSV ja Hädaabiteabe.<br/><br/></ul><b>Privaatne salvestusruumi konto</b>: parameetrid on: <br/><br/><ul><b>Konto nimi</b>: saate määrata, mis sisaldab bloobimälu, mida soovite lugeda konto nimi.<br/><br/><b>Konto võti</b>: määrab kontoga seotud salvestusruumi võti.<br/><br/><b>Container, kataloogi või bloobimälu tee</b> : määrab nime bloobimälu, mis sisaldab andmeid lugeda.<br/><br/><b>Bloobimälu failivorming</b>: saate määrata bloobimälu teenuse andmete vorming. Toetatud andmevormingute on CSV, TSV, Hädaabiteabe CSV määratud kodeering ja Excelis. <br/><br/><ul>Kui vorming on CSV- või TSV, siis veenduge, et näidata, kas fail sisaldab päiserida.<br/><br/>Exceli suvandi abil saate andmeid lugeda Exceli töövihikutest. <i>Exceli andmete vormindamine</i> suvandi, näitab, kas andmed on Exceli töölehe vahemikust või Exceli tabeli. Määrake suvandi <i>Exceli leht või manustatud tabeli </i>nimi lehe või tabel, mida soovite lugeda.</ul><br/>|
Andmekanali pakkuja | Toetatud kanali pakkuja loeb andmeid. Hetkel on toetatud ainult Open Data protokolli (OData) vormingus. | <b>Andmete sisutüüp</b>: saate määrata OData vorming.<br/><br/><b>Lähte-URL</b>: määrab andmekanali täielik URL-i. <br/>Näiteks järgmine URL loeb näidisandmebaas: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
