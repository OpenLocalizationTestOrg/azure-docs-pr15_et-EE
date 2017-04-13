<properties 
    pageTitle="Teisaldage andmed MongoDB abil andmete Factory | Microsoft Azure'i" 
    description="Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory MongoDB andmebaasist." 
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
    ms.date="08/04/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Andmete teisaldamine kaudu MongoDB Azure'i andmed Factory abil

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory kohapealse MongoDB andmebaasist andmete teisaldamiseks teise andmesalve. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri tegevuste ja allika/valamu andmete poe kombinatsioonid Kopeeri tegevuse toetab.

Andmete Factory teenuse toetab ühenduse Andmehalduslüüsi kasutamine asutusesisestest MongoDB allikatest. Vaadake teemat [Andmehalduslüüsi](data-factory-data-management-gateway.md) artikli Andmehalduslüüsi kohta ja üksikasjalikud juhised artiklist [asutusesisesest Cloud andmete teisaldamiseks](data-factory-move-data-between-onprem-and-cloud.md) häälestamise lüüsi andmete müügivõimaluste andmete teisaldamiseks. 

> [AZURE.NOTE] Peate kasutama lüüsi ühenduse MongoDB isegi juhul, kui see on majutatud Azure'i IaaS VM. Kui soovite ühenduse MongoDB majutatud pilveteenuses eksemplari, saate installida ka lüüsi eksemplari IaaS VM.

Andmete Factory toetab praegu ainult jooksva andmete MongoDB muid andmeid talletab, kuid mitte muid andmeid salvestab andmete teisaldamine MongoDB.

## <a name="prerequisites"></a>Eeltingimused
Teenuse Azure'i andmed Factory saaksid oma kohapealse MongoDB andmebaasiga ühenduse loomiseks peate installima järgmised komponendid: 

- Andmehalduslüüsi 2.0 või ülaltoodud samasse arvutisse, mis hostib andmebaasi või eraldi arvutisse omavahelise ressursid andmebaasi. Andmehalduslüüs on tarkvara, mis ühendab kohapealse andmeallikad pilveteenustega turvaline ja hallatavate. Vt [Andmehalduslüüsi](data-factory-data-management-gateway.md) artikkel Andmehalduslüüsi üksikasjad.
  
    Lüüsi installimisel installitakse automaatselt mõne Microsoft MongoDB ODBC-draiver MongoDB ühendamiseks. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete MongoDB andmebaasi toetatud valamu andmete poe on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Sektordiagrammis kuvatakse Azure'i bloobimälu MongoDB andmebaasi andmete kopeerimine. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>Näide: Andmete kopeerimine MongoDB Azure'i bloobimälu
See näide näitab, kuidas kohapealse MongoDB andmebaasis asuvate andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesMongoDb](#linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [MongoDbCollection](#dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [MongoDbSource](#copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete päringu tulem MongoDB andmebaasis on bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

Esimese sammuna häälestamise juhiseid artiklis [Andmehalduslüüsi](data-factory-data-management-gateway.md) andmehalduslüüsi. 

**Lingitud MongoDB teenus**

    { 
        "name": "OnPremisesMongoDbLinkedService", 
        "properties": 
        { 
            "type": "OnPremisesMongoDb", 
            "typeProperties": 
            { 
                "authenticationType": "<Basic or Anonymous>", 
                "server": "< The IP address or host name of the MongoDB server >",  
                "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
                "username": "<username>", 
                "password": "<password>",
               "authSource": "< The database that you want to use to check your credentials for authentication. >",
                "databaseName": "<database name>",
                "gatewayName": "<mygateway>"
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

**MongoDB Sisestuskeel andmekomplekti** Säte "välise": "true" teatab andmete Factory teenus, et tabelis on väliste andmete factory ja ei esitata andmete factory toimingu.
    
    {
         "name":  "MongoDbInputDataset",
        "properties": { 
            "type": "MongoDbCollection", 
            "linkedServiceName": "OnPremisesMongoDbLinkedService", 
            "typeProperties": { 
                "collectionName": "<Collection name>"   
            }, 
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        } 
    }



**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Torujuhe Kopeeri tegevus**

Tulemas sisaldab Kopeeri tegevust, mis on konfigureeritud kasutada ülaltoodud sisend ja väljund andmekomplektide ja plaanitud käivitamiseks tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **MongoDbSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu jaoks **päringu** atribuudi määratud valib andmete kopeerimiseks tunnis.
    
    {
        "name": "CopyMongoDBToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "MongoDbSource",
                            "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MongoDbInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MongoDBToAzureBlob"
                }
            ],
            "start": "2016-06-01T18:00:00Z",
            "end": "2016-06-01T19:00:00Z"
        }
    }


## <a name="linked-service-properties"></a>Lingitud teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud **OnPremisesMongoDB** lingitud teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesMongoDb** | Jah |
| Server | IP-aadress või hosti MongoDB serveri nimi. | Jah | 
| Port | TCP-pordi MongoDB server kasutab kuulata, klientrakenduse kaudu. | Valikuline, vaikeväärtus: 27017 |
| authenticationType | Tavaline või anonüümse. | Jah | 
| kasutajanimi | Kasutajakonto MongoDB juurdepääsu. | Jah (kui kasutatakse elementaarautentimine).|
| parooli | Kasutaja parooli. |   Jah (kui kasutatakse elementaarautentimine). | 
| authSource | MongoDB andmebaas, mida soovite kontrollida mandaat autentimise kasutada nimi. | Valikuline (kui kasutatakse elementaarautentimine). vaikimisi: kasutab administraatori konto ja määratud databaseName atribuudi andmebaasi. |  
| databaseName | Nimi MongoDB andmebaas, mida soovite juurde pääseda. | Jah |
| gatewayName | Lüüsi, mis kasutab andmesalve nimi. | Jah | 
| encryptedCredential | Krüptitud lüüsi mandaati. | Valikuline. |


Üksikasjad kohapealse MongoDB andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="dataset-type-properties"></a>Andmekomplekti atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **MongoDbCollection** on järgmised atribuudid.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| collectionName | Saidikogumi MongoDB andmebaasi nimi. | Jah | 

## <a name="copy-activity-type-properties"></a>Tegevuse tüüp atribuutide kopeerimine

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitika atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse **typeProperties** jaotise saadaolevad atribuudid erinevad Teisalt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allika tüüp **MongoDbSource** järgmised atribuudid on saadaval jaotises typeProperties:

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| päringu | Kohandatud päringu abil saate andmeid lugeda. | SQL-92 päringustringi. Näide: valige * MyTable kaudu. | Ei, (kui **collectionName** **andmekogumi** on määratud) | 

## <a name="schema-by-data-factory"></a>Skeemi järgi andmete Factory
Azure'i andmed Factory teenuse tuletab skeemi MongoDB kollektsioonist abil uusimate 100 dokumentide kogumist. Kui need 100 dokumendid ei sisalda täielik skeemi, võib eirata veerge käigus. 

## <a name="type-mapping-for-mongodb"></a>Tippige vastenduse MongoDB

Nagu [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis mainitud, sooritab Kopeeri tegevuse automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised 2-samm lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamine MongoDB järgmised vastendused MongoDB tüüpi kasutatakse .NET tüübid.

| MongoDB tüüp | .NET framework tüüp |
| ------------------- | ------------------- | 
| Binaarsed | Byte] |
| Kahendmuutuja | Kahendmuutuja |
| Kuupäev | Kuupäev ja kellaaeg |
| NumberDouble | Kahekordne |
| NumberInt | Int32 |
| NumberLong | Int64 |
| ObjectId väärtuse | String |
| String | String |
| UUID | GUID | 
| Objekti | Renormalized üheks Ühenda veerud "_" pesastatud eraldaja |

> [AZURE.NOTE]  
> Tugi virtuaalse tabelite kasutamine massiivides kohta lisateabe saamiseks vaadake [toetavad jaoks keeruline virtuaalse tabelite kasutamine](#support-for-complex-types-using-virtual-tables) jaotist allpool. 

Praegu ei toetata MongoDB järgmised andmetüübid: DBPointer, JavaScripti, Min ja Max võti Lihtavaldise, sümbol, ajatempli määratlemata

## <a name="support-for-complex-types-using-virtual-tables"></a>Tugiteenuste jaoks keeruline virtuaalse tabelite abil
Azure'i andmed Factory kasutab sisseehitatud ODBC-draiver ühenduse ja kopeerige andmed andmebaasi MongoDB. Keerukate tüüpi nagu massiivid või objektide erinevate andmetüüpidega üle dokumentide, juht uuesti normaliseerib andmete vastavate virtuaalse tabelitesse. Eelkõige sellest, kui tabel sisaldab selliste veergude, juht loob virtuaalse järgmistes tabelites:

-   **Põhiline tabeli**, mis sisaldab samad andmed otse tabelina, välja arvatud keerukas tüüp veerud. Põhiline tabeli kasutab real tabel, mis tähistab see sama nimi.
-   **Virtuaalse tabeli** iga keerukas tüüp veeru laieneb pesastatud andmed. Virtuaalne tabelid nimega nimi real tabel, eraldaja "_" ja massiivi- või objekti nimi.

Virtuaalne tabelid viitavad andmed otse tabelis draiver Denormaliseeritud andmetele juurdepääsu lubamine. Vt näide lõik all üksikasjad. Pääsete MongoDB massiivi sisu päringu- ja liitumise virtuaalse tabelid.

[Kopeeri viisardi](data-factory-data-movement-activities.md#data-factory-copy-wizard) abil intuitiivselt vaadata tabelite loendist MongoDB andmebaasi, sh virtuaalse tabelid ja asuvate andmete eelvaade. Saate viisardis Kopeeri päringu koostamiseks ja kinnitage tulemuse kuvamiseks.

### <a name="example"></a>Näide

Näiteks "ExampleTable" all on MongoDB tabel, milles on üks veerg objektide array igas lahtris – arveid ja üks veerg skalaarse tüüpi – hinnangute array. 

_id | Kliendi nimi | Arved | Teenusetaseme | Hinnangute
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{invoice_id: "123", üksuse: "röster", hind: "456", allahindlus: "0,2"}, {invoice_id: "124" kirje: "ahi", hind: "1235" diskonto: "0,2"}] | Hõbe | [5,6]
2222 | XYZ | [{invoice_id: "135" kirje: "külmkapp", hind: "12543", allahindlus: "0,0"}] | Kuld | [1,2]

Juhi tekitaks mitme virtuaalse tabeli tähistada selle ühe tabeli. Esimese virtuaalse tabel on põhiline tabeli nimega "ExampleTable" allpool näidatud. Base tabel sisaldab algse tabeli kõik andmed, kuid andmed massiivides on välja jäetud ja virtuaalse tabelites on laiendatud.

_id | Kliendi nimi | Teenuse tase
--- | ------------- | -------------
1111 | ABC | Hõbe
2222 | XYZ | Kuld

Järgmistes tabelites on virtuaalse tabelid, mis esindab algse massiivi näites. Järgmised tabelid sisaldavad järgmist: 

- Viide tagasi välja algse primaarvõtme veerg vastava rea algse massiivi (kaudu _id veerg)
- Algse massiivi sees olevate andmete asukoha märge 
- Laiendatud andmed massiivi iga elemendi jaoks

Tabel "ExampleTable_Invoices":

_id | ExampleTable_Invoices_dim1_idx | invoice_id | üksuse | Hind | Allahindlus
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | röster | 456 | 0,2
1111 | 1 | 124 | ahju | 1235 | 0,2
2222 | 0 | 135 | külmkapp | 12543 | 0,0

Tabel "ExampleTable_Ratings":

_id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.

## <a name="next-steps"></a>Järgmised sammud
Lugege teemat [andmete kohapealse ja pilveteenuse vahel liikumine](data-factory-move-data-between-onprem-and-cloud.md) artiklis üksikasjalike juhiste saamiseks loomine andmete kohaletoimetamisel, mis teisaldab andmed kohapealse andmete poest on Azure andmesalve. 

