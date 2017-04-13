<properties 
    pageTitle="Teisaldage andmed Cassandra abil andmete Factory | Microsoft Azure'i" 
    description="Lisateavet asutusesisese Cassandra andmebaasist Azure'i andmed Factory abil andmete teisaldamiseks." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Andmete teisaldamine kohapealse Cassandra andmebaasist Azure'i andmed Factory abil
See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory kohapealse Cassandra andmebaasis asuvate andmete kopeerimine mis tahes [Toetatud andmeallikate ja valamud](data-factory-data-movement-activities.md#supported-data-stores) jaotise valamu veeru loendis andmesalve. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri aktiivsusest ja toetatud andmete poe kombinatsioone.

Andmete factory toetab praegu ainult jooksva Cassandra andmebaasi toetatud [valamu andmete salvestab](data-factory-data-movement-activities.md#supported-data-stores)andmed, kuid not libiseva muud andmed salvestab Cassandra andmebaasi andmeid.

## <a name="prerequisites"></a>Eeltingimused
Teenuse Azure'i andmed Factory saaksid oma kohapealse Cassandra andmebaasiga ühenduse loomiseks peate installima järgmist: 

- Andmehalduslüüsi 2.0 või ülaltoodud samasse arvutisse, mis hostib andmebaasi või eraldi arvutisse omavahelise ressursid andmebaasi. Andmehalduslüüs on tarkvara, mis ühendab kohapealse andmeallikate pilveteenustega turvaline ja hallatavate. Vt artikli [andmete kohapealse ja pilveteenuse vahel liikumine](data-factory-move-data-between-onprem-and-cloud.md) Andmehalduslüüsi üksikasjad.
  
    Lüüsi installimisel installitakse automaatselt Microsoft Cassandra ODBC draiver Cassandra andmebaasiga ühenduse loomiseks kasutada. 

> [AZURE.NOTE] Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete Cassandra andmebaasi toetatud valamu andmete poe on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Sektordiagrammis kuvatakse Azure'i bloobimälu Cassandra andmebaasi andmete kopeerimine. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Näide: Andmete kopeerimine Cassandra bloobimälu
Valimi kopeerib andmete andmebaasist Cassandra mõne Azure'i bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. Otse mis tahes valamud, mis on märgitud [Andmete liikumine tegevuste](data-factory-data-movement-activities.md#supported-data-stores) artikli Azure'i andmed Factory Kopeeri tegevuse abil saate andmeid kopeerida. 

- Lingitud teenuse tüüpi [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [CassandraTable](#cassandratable-properties).
- Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [CassandraSource](#cassandrasource-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Lingitud Cassandra teenus**

Selles näites kasutatakse **Cassandra** lingitud teenus. Vaadake [Cassandra lingitud teenuse](#onpremisescassandra-linked-service-properties) osa selle lingitud teenuse toetatud atribuudid.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
            }
        }
    }


**Azure'i lingitud salvestusteenus**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
        "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

**Cassandra Sisestuskeel andmekomplekti**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Andmete Factory teenuse säte **välise** **true** teatab, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.

**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Torujuhe Kopeeri tegevus**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **CassandraSource** ja **valamu** tüüp väärtuseks **BlobSink**. 

Vaadake selle RelationalSource ei toeta atribuutide loendi [RelationalSource atribuudid](#cassandrasource-type-properties) . 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Lingitud OnPremisesCassandra teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud Cassandra lingitud teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesCassandra** | Jah |
| Host | Üks või mitu IP-aadresside või hostinimed Cassandra serverite.<br/><br/>Määrake IP-aadresside või hostinimed ühenduse kõikides serverites samaaegselt komaga eraldatud loend. | Jah | 
| Port | TCP-pordi Cassandra server kasutab kuulata, klientrakenduse kaudu. | Ei, vaikimisi väärtus: 9042 |
| authenticationType | Tavaline või Anonüümsetelt | Jah |
| kasutajanimi |Määrake kasutajanimi kasutajakonto. | Jah, kui authenticationType on seatud väärtusele Basic. |
| parooli | Määrake kasutaja konto parool.  | Jah, kui authenticationType on seatud väärtusele Basic. |
| gatewayName | Kohapealse Cassandra andmebaasiga ühenduse loomiseks kasutatava lüüsi nimi. | Jah |
| encryptedCredential | Krüptitud lüüsi mandaati. | Ei | 

## <a name="cassandratable-properties"></a>CassandraTable atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **CassandraTable** on järgmised atribuudid

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| keyspace | Keyspace või skeemi Cassandra andmebaasi nimi. | Jah (kui see on **päringu** jaoks **CassandraSource** pole määratletud). |
| tableName | Tabeli Cassandra andmebaasi nimi. | Jah (kui see on **päringu** jaoks **CassandraSource** pole määratletud). |


## <a name="cassandrasource-type-properties"></a>CassandraSource atribuudid
Jaotised ja atribuute määratlemine tegevuste täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisendi ja väljundi tabelite ja poliitika atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allika tüüp **CassandraSource**, järgmised atribuudid on saadaval jaotises typeProperties:

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| päringu | Kohandatud päringu abil saate andmeid lugeda. | Päringu SQL-92 või CQL päring. Vt [CQL viide](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL-päringu kasutamisel saate määrata tähistada tabel, mida soovite päringu **keyspace name.table nimi** . | Ei (kui tableName ja keyspace andmekomplekti kohta on määratletud).  |
| consistencyLevel | Vormindusühtsuse tase määrab, mitu koopiad vastama lugemise taotluse enne andmete sisaldamise klientrakendust. Cassandra kontrollib määratud arvu andmete lugemise taotluse vastavaks koopiad. | ÜKS, KAKS, KOLM, QUORUM, KÕIK, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Üksikasjad leiate [seadistamine andmete järjepidevuse](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | Ei. Vaikeväärtus on üks. |  


### <a name="type-mapping-for-cassandra"></a>Tippige vastendust Cassandra
Cassandra tüüp | .Net-i põhiste tüüp
--------------- | ---------------
ASCII | String 
SUUR TÄISARV | Int64
BLOOBIMÄLU | Byte]
KAHENDMUUTUJA | Kahendmuutuja
DECIMAL | Decimal
KAHEKORDNE | Kahekordne
HÕLJUMINE | Ühe
INET | String
INT | Int32
TEKSTI | String
AJATEMPLI | Kuupäev ja kellaaeg
TIMEUUID | GUID
UUID | GUID
VARCHAR | String
VARINT | Decimal

> [AZURE.NOTE]  
> Saidikogumi tüüpi (kaarti, määramine, loendi jne), lugege jaotist [töötamine Cassandra saidikogumi tüüpi virtuaalse tabeli abil](#work-with-collections-using-virtual-table) . 
> 
> Kasutaja määratletud tüübid ei toetata.
> 
> Pikkus kahendarvu veerg ja stringi pikkus ei tohi olla suurem kui 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Töötamine saidikogumid virtuaalse tabeli abil
Azure'i andmed Factory kasutab sisseehitatud ODBC-draiver ühenduse ja kopeerige andmed andmebaasi Cassandra. Saidikogumi puhul, sh kaarti, Sea ja loend, juht uuesti normaliseerib andmed vastavate virtuaalse tabelitesse. Eelkõige sellest, kui tabel sisaldab iga saidikogumi veergu, juht loob virtuaalse järgmistes tabelites:

-   **Põhiline tabeli**, mis sisaldab samad andmed otse tabelina, välja arvatud saidikogumi veerud. Põhiline tabeli kasutab real tabel, mis tähistab see sama nimi.
-   **Virtuaalse tabeli** iga saidikogumi veeru laieneb pesastatud andmed. Virtuaalne tabelid, mis tähistavad saidikogumid nimega real tabel, eraldaja "_vt_" ja selle veeru nimi nimi.

Virtuaalne tabelid viitavad andmed otse tabelis draiver Denormaliseeritud andmetele juurdepääsu lubamine. Üksikasju vt näide jaotises. Pääsete Cassandra saidikogumite sisu päringu- ja liitumise virtuaalse tabelid.

Saate kasutada [Kopeeri viisardi](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitiivselt vaadata tabelite loendist Cassandra andmebaasi, sh virtuaalse tabelid ja asuvate andmete eelvaade. Saate viisardis Kopeeri päringu koostamiseks ja kinnitage tulemuse kuvamiseks.

### <a name="example"></a>Näide
Näiteks järgmised "ExampleTable" on Cassandra andmebaasi tabeli, mis sisaldab mõnda täisarv primaarvõtme veerg nimega "pk_int", teksti veerg nimega väärtust, loendiveeru, kaardi veeru ja määramine veerg (nimega "StringSet").

pk_int | Väärtus | Loend | Kaardi |   StringSet
------ | ----- | ---- | --- | --------
1 | "valimi väärtus 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "valimi väärtus 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Juhi tekitaks mitme virtuaalse tabeli tähistada selle ühe tabeli. Võõrkeelsed võtme veerud virtuaalse tabelite viide esmane võtme veerud otse tabelis ning näidata, millist virtuaalse tabelirea vastab reaal tabeli rida. 

Esimese virtuaalse tabel on põhiline tabeli nimega "ExampleTable" on näidatud järgmises tabelis. Base tabelis andmete algse andmebaasi tabelina, välja arvatud saidikogumid, mis on välja jäetud sellest tabelist ja laiendatud virtuaalse muudes tabelites.

pk_int | Väärtus
------ | -----
1 | "valimi väärtus 1"
3 | "valimi väärtus 3"

Järgmistes tabelites on virtuaalse tabelid, mis renormalize veerud loendis, kaarti ja StringSet andmeid. Veergude nimed, mis lõppevad "_index" või "_key" näitab algse loendi või kaardi asuvate andmete paigutust. Veergude nimed, mis lõppevad "_value" sisaldavad laiendatud andmete kogumine.

#### <a name="table-exampletablevtlist"></a>Tabel "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabel "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabel "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.
