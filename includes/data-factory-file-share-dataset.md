## <a name="fileshare-dataset-type-properties"></a>FileShare andmekomplekti tüüp atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](../articles/data-factory/data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased andmekomplekti kõigi jaoks. 

Iga tüüpi andmekomplekti erineb jaotise **typeProperties** . Pakub teavet, mis on seotud andmekomplekti tüüp. TypeProperties jaotises andmekomplekt tüüpi **FileShare** andmekomplekti on järgmised atribuudid.

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
folderPath | Sub kausta tee. Kasutada paomärki "\" stringi erimärkide jaoks. Näiteid vt [valimi lingitud teenuseid ja andmekomplekti määratlusi](#sample-linked-service-and-dataset-definitions) .<br/><br/>Selle atribuudi saate kombineerida **partitionBy** on sektori alusel kausta tee algus/lõpp kuupäev-korda. | Jah
faili nimi | Kui soovite kindlat tüüpi failiga kausta viidata tabeli **folderPath** Määrake faili nimi. Kui te ei määra mis tahes väärtus selle atribuudi tabeli osutab kõik failid kaustast.<br/><br/>Kui faili nimi soovitud väljundi andmekogumi pole määratud, loodud faili nimi oleks järgmine järgmises vormingus: <br/><br/>Andmed. <Guid>txt (näide: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Ei
partitionedBy | partitionedBy saab määrata dünaamiline folderPath, filename kellaajaandmete sarja. Näiteks folderPath parameetriga andmete iga tund. | Ei
Vormindamine | Järgmist tüüpi vormingus toetatud: **tekstivorming**, **AvroFormat**, **JsonFormat**ja **OrcFormat**. Need väärtused jaotises Vorming atribuudi **Tüüp** väärtuseks. Vt lisateavet [Täpsustades tekstivorming](#specifying-textformat), [Täpsustades AvroFormat](#specifying-avroformat), [Täpsustades JsonFormat](#specifying-jsonformat)ja [Täpsustades OrcFormat](#specifying-orcformat) lõigud. Kui soovite kopeerida faile-on vahel (kahendarvu koopia) salvestab failipõhiste saate vahele jätta vorming jaotisele nii sisend- ja andmekomplekti määratlusi. | Ei
fileFilter | Määrake filtri, et valida alamhulk on folderPath failide asemel kõik failid kasutada.<br/><br/>Väärtused on lubatud: `*` (mitut märki) ja `?` (üksikmärki).<br/><br/>Näited 1:`"fileFilter": "*.log"`<br/>Näide 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter kehtib ka Sisestuskeel FileShare andmekomplekti. Atribuut HDFS ei toetata.  | Ei
| tihendamine | Määrake tüüp ja taseme tihendamise andmete jaoks. Toetatud tüübid on: **GZip**, **Deflate**, ja **BZip2** ja toetatud tasemed on: **optimaalne** ja **kiireim**. Praegu ei toetata tihendussätted andmete **AvroFormat** või **OrcFormat**. Lisateavet leiate teemast [tihendamise tugiteenuste](#compression-support) jaotises.  | Ei |
| useBinaryTransfer | Määrata, kas kasutada kahendarvu edastamine režiim. Binaarsed režiimis ja false ASCII True. Vaikimisi väärtus: tõene. Selle atribuudi saab kasutada ainult kui tüüp on seostatud lingitud teenuse tüüp: FtpServer. | Ei | 
 

> [AZURE.NOTE] faili nimi ja fileFilter ei saa korraga kasutada.

### <a name="using-partionedby-property"></a>PartionedBy atribuudi abil

Nagu eelmises jaotises, saate määrata dünaamiline folderPath, aeg sarja andmete partitionedBy failinimi. Saate seda teha andmete Factory makrod ja süsteemi muutuja SliceStart, SliceEnd, mis näitavad loogilise tähtaeg antud andmete sektorit. 

Kellaaja sarja andmekomplektide kohta lisateabe saamiseks plaanimine ja sektoritele, vt [Andmekomplektide loomise](../articles/data-factory/data-factory-create-datasets.md), [plaanimine ja täitmise](../articles/data-factory/data-factory-scheduling-and-execution.md)ja [Loomise torujuhtmete](../articles/data-factory/data-factory-create-pipelines.md) artiklite. 

#### <a name="sample-1"></a>Näide 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Selles näites {sektorit} on asendatud funktsiooniga määratud andmete Factory süsteemi muutuja SliceStart vormingus (YYYYMMDDHH) väärtuse. Funktsiooni SliceStart viitab alguskellaaeg sektorit. Funktsiooni folderPath on erinev iga sektorit. Näide: wikidatagateway/wikisampledataout/2014100103 või wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Näide 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Selles näites ekstraktitakse aasta, kuu, päev ja kellaaeg SliceStart üheks eraldi muutujate, mida kasutatakse atribuutide folderPath ja faili nimi.
