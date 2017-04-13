<properties 
    pageTitle="Vabastage märkmete jaoks Andmehalduslüüsi | Azure'i andmed Factory" 
    description="Data Management Gateway tory väljalaskemärkmed" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Andmehalduslüüsi väljalaskemärkmed
Üks probleeme tänapäevane andmete integreerimiseks on sujuvalt liikuda andmete ja kohapealsete Cloud. Andmete Factory teeb selle integreerimine sujuvalt koos Andmehalduslüüsi, mis on agent, et saate installida lubamiseks hübriid andmete liikumist kohapealse.

Lugege üksikasjalikku teavet Andmehalduslüüsi ja kuidas seda kasutada järgmisi artikleid: 

- [Andmehalduslüüs](data-factory-data-management-gateway.md)
- [Andmete teisaldamine kohapealsesse ja Pilveteenusest, kasutades Azure'i andmed Factory](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Praegune versioon (2.2.6072.1)

- Toetab säte HTTP-puhverserver lüüsi Andmehalduslüüsi Konfigureerimishalduri abil. Kui konfigureeritud, pääseb Azure'i bloobimälu, Azure'i tabelist, Azure'i andmed Lake ja dokumendi DB kaudu HTTP-puhverserver.
- Toetab päise käsitsemise tekstivorming Azure'i bloobimälu Azure'i andmesalve Lake, et andmete kopeerimisel kohapealse failisüsteemi ja kohapealse HDFS.
- Andmete kopeerimine bloobimälu lisamine ning lehe BLOOBI koos juba toetatud Blokeeri bloobimälu toetab.
- Lüüsi olek **Online (piiratud)**, mis näitab, et lüüsi peamisi funktsioone töötab peale kopeerimise viisard interaktiivsed toiming tugi tutvustab.
- Suurendab töökindluse lüüsi registreerimise abil registreerimise võti.

## <a name="earlier-versions"></a>Varasemates versioonides

## <a name="2160401"></a>2.1.6040.1

- DB2 draiver on kaasatud lüüsi installipakett kohe. Teil pole vaja eraldi installida. 
- DB2 draiver toetab nüüd z/OS ja DB2 i (AS / 400) koos juba Toetatud platvormid (Linux, Unix ja Windows). 
- Nii lähte- või sihtkoha DocumentDB kasutamise kohapealse andmete poed toetab
- Andmete kopeerimine/külma/sooja toetab Bloobivahemälu salvestusruumi koos juba toetatud üldotstarbeline salvestusruumi konto. 
- Võimaldab asutusesisese SQL serveriga kaugarvuti sisselogimine privileegidega lüüsi kaudu.  

## <a name="2060131"></a>2.0.6013.1

- Valige keel ja kultuur kasutavad lüüsi käsitsi installimise ajal.
- Kui lüüs ei tööta oodatud viisil, saate saata lüüsi logid seitsme viimase päeva Microsofti hõlbustamiseks probleemi tõrkeotsingut. Kui lüüs pole ühendatud pilveteenusesse, saate salvestada ja arhiivida lüüsi logid.  
- Lüüsi configuration manager kasutajaliidese täiustused:
    - Tehke menüü Avaleht jaotises rohkem nähtavaks lüüsi olek.
    - Lihtsustatud ja reorganiseeriti juhtelemendid.
- Saate kopeerida andmete storage [kood häirimatu Kopeeri eelvaade tööriista](data-factory-copy-data-wizard-tutorial.md)abil. Teemast [Etapiviisilise kopeerimine](data-factory-copy-activity-performance.md#staged-copy) üldiselt selle funktsiooni kohta. 
- Abil saate Andmehalduslüüsi sissepääsu andmetele otse asutusesisese SQL serveri andmebaasist Azure seadme õ.
- Jõudlustäiustused
    - Kopeeri kood tasuta eelvaade tööriistas skeemi/eelvaade vastu SQL serveri vaatamise kohta jõudluse parandamiseks.



## <a name="11259531"></a>1.12.5953.1
- Veaparandusi

## <a name="11159181"></a>1.11.5918.1

- Lüüsi sündmuste logi maksimaalne maht on kasvanud 40 MB 1 MB.
- Hoiatus kuvatakse juhul, kui ajal lüüsi automaatne uuendamine on vaja uuesti. Saate valida uuesti paremale siis või uuem versioon. 
- Kui automaatne uuendamine ei, lüüsi Installeri uued katsed, automaatne värskendamine kolm korda veebisaidil maksimum.
- Jõudlustäiustused
    - Kopeeri kood tasuta stsenaariumi kohapealse serverist suurte tabelite laadimise jõudluse parandamiseks.
- Veaparandusi

## <a name="11058921"></a>1.10.5892.1

- Jõudlustäiustused
- Veaparandusi

## <a name="1958652"></a>1.9.5865.2

- Null puutefunktsioonide automaatse värskendamise funktsioon
- Uue ikooni lüüsi olek näitajad
- Võimalus kliendi "Värskenda kohe"
- Võimalus määrata värskendamise ajakava aeg
- PowerShelli skripti automaatne uuendamine sisse/välja lülitamine
- JSON-vormingu tugi  
- Jõudlustäiustused
- Veaparandusi

## <a name="1858221"></a>1.8.5822.1

- Tõrkeotsingu parendamiseks
- Jõudlustäiustused
- Veaparandusi

### <a name="1757951"></a>1.7.5795.1

- Jõudlustäiustused
- Veaparandusi

### <a name="1757641"></a>1.7.5764.1

- Jõudlustäiustused
- Veaparandusi

### <a name="1657351"></a>1.6.5735.1

- Kohapealse HDFS allika/valamu tugi
- Jõudlustäiustused
- Veaparandusi

### <a name="1656961"></a>1.6.5696.1

- Jõudlustäiustused
- Veaparandusi

### <a name="1656761"></a>1.6.5676.1

- Tugiteenuste diagnostikatööriistu Configuration Manager
- Tabeli veergude tabelina esitatud andmed allikate kasutajatugi Azure'i andmed Factory
- Azure'i andmed Factory DW SQL-i tugi
- Azure'i andmed Factory Reclusive BlobSource ja FileSource tugi
- Azure'i andmed Factory CopyBehavior – MergeFiles, PreserveHierarchy ja FlattenHierarchy BlobSink ja FileSink kahendarvu koopia tugi
- Kopeeri tegevuse aruande Azure'i andmed Factory tugi
- Tugiteenuste andmete allikas ühenduvuse valideerimine Azure'i andmed Factory
- Veaparandusi


### <a name="1656721"></a>1.6.5672.1

- Azure'i andmed Factory tabeli nime, ODBC-andmeallikas kasutajatugi
- Jõudlustäiustused
- Veaparandusi

### <a name="1656581"></a>1.6.5658.1

- Azure'i andmed Factory valamu faili tugi
- Azure'i andmed Factory kahendarvu Kopeeri säilitamine hierarhia tugi
- Azure'i andmed Factory Kopeeri tegevuse Idempotency kasutajatugi
- Veaparandusi

### <a name="1656401"></a>1.6.5640.1

- 3 rohkemate andmeallikatega kasutajatugi Azure'i andmed Factory (ODBC, OData, HDFS)
- Azure'i andmed Factory hinnapakkumise märk on csv-parseri tugi
- Tihendamise tugi (BZip2)
- Veaparandusi

### <a name="1556121"></a>1.5.5612.1

- Azure'i andmed Factory (MySQL-i, PostgreSQL-i, DB2, Teradata ja Sybase'i) viis relatsiooniandmebaasid kasutajatugi
- Tihendamise tugi (Gzip ja Deflate)
- Jõudlustäiustused
- Veaparandusi


### <a name="1455491"></a>1.4.5549.1

- Oracle'i andmete allikas tugi Azure andmete Factory lisamine
- Jõudlustäiustused
- Veaparandusi

### <a name="1454921"></a>1.4.5492.1

- Ühendatud kahendarvuks, mis toetab Microsoft Azure'i andmed Factory-ja Office 365 Power BI
- Täpsem piiritlemine konfiguratsiooni Kasutajaliidese ja registreerimise protsess
- Azure'i andmed Factory – Azure sissepääsu ja sealt kasutajatugi SQL serveri andmeallikas

### <a name="1253031"></a>1.2.5303.1

-   Rohkem aeganõudev andmeallikaühendused toetamiseks ajalõpp probleemi lahendamiseks. 
    
### <a name="1155268"></a>1.1.5526.8

- Nõuab .NET Frameworki 4.5.1 tingimusena installimise käigus.

### <a name="1051442"></a>1.0.5144.2

- Muudatused, mida mõjutavad stsenaariumid Azure'i andmed Factory. 
