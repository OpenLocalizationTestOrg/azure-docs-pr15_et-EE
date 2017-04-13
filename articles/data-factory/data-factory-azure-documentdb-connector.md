<properties 
    pageTitle="Andmete teisaldamine/DocumentDB | Microsoft Azure'i" 
    description="Siit saate teada, kuidas andmete teisaldamine/Azure'i DocumentDB kollektsioonist Azure'i andmed Factory abil" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Andmete teisaldamine ja sealt DocumentDB Azure'i andmed Factory abil

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmeid Azure'i DocumentDB teise andmete talletamiseks ja DocumentDB andmete teisaldamine teise andmesalve liikuda. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri tegevuste ja toetatud andmete poe kombinatsioone.

Järgmised näidet kujutavad andmete kopeerimine ja Azure DocumentDB ja Azure'i bloobimälu. Siiski andmete võib olla kopeeritud **otse** mis tahes allikatest mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  

> [AZURE.NOTE] Kohta – ruumide/Azure'i IaaS andmete kopeerimine andmeid talletab, et Azure'i DocumentDB ja vastupidi toetatud Andmehalduslüüsi versioon 2.1 ja kohal.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Näide: Andmete kopeerimine DocumentDB Azure'i bloobimälu

Allpool näidis kuvatakse:

1. Lingitud teenuse tüüpi [DocumentDb](#azure-documentdb-linked-service-properties).
2. Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Mõne sisendi [andmekomplekti](data-factory-create-datasets.md) tüüpi [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete Azure'i DocumentDB Azure'i bloobimälu abil. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised.

**Azure'i DocumentDB lingitud teenus:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure'i bloobimälu lingitud teenuse:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure'i dokumendi DB Sisestuskeel andmekomplekti:**

Valimi eeldab, et teil on nimega **isiku** Azure'i DocumentDB andmebaasi kogumi.
 
Säte "välise": "true" ja milles externalData poliitika info Azure'i andmed Factory teenus, et tabelis on väliste andmete factory ja ei tooda andmete factory toimingu.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure'i bloobimälu väljund andmekomplekti:**

Andmed kopeeritakse uue bloobimälu bloobimälu, mis kajastab kindla kuupäeva ja kellaaja koos tund granulaarsus teega kord tunnis.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Valimi JSON dokumendi isiku saidikogumi DocumentDB andmebaasi: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB toetab päringu dokumentide SQL süntaks nagu hierarhilise JSON dokumentide üle. 

Näide: Valige Person.PersonId Person.Name.First AS eesnimi, Person.Name.Middle MiddleName Person.Name.Last AS perekonnanimi isiku

Järgmised müügivõimaluste kopeerib andmete andmebaasis DocumentDB kogumisest isik on Azure'i bloobimälu. Kopeeri tegevuse sisendi ja väljundi määratud andmekomplektide on.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Näide: Andmete kopeerimine Azure'i bloobimälu Azure'i DocumentDB

Allpool näidis kuvatakse:

1. Lingitud teenuse tüüpi [DocumentDb](#azure-documentdb-linked-service-properties).
2. Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Valimi kopeerib andmete Azure'i bloobimälu Azure'i DocumentDB abil. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised.

**Azure'i bloobimälu lingitud teenuse:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure'i DocumentDB lingitud teenus:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure'i bloobimälu Sisestuskeel andmekomplekti:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure'i DocumentDB väljund andmekomplekti:**

Valimi kopeerib andmete kogumise nimega "Kelle".

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Järgmised müügivõimaluste kopeerib andmete Azure'i bloobimälu soovitud DocumentDB inimese kogumist. Kopeeri tegevuse sisendi ja väljundi määratud andmekomplektide on. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Kui on valimi bloobimälu sisend 

    1,John,,Doe

Väljundi JSON DocumentDB rakenduses kuvatakse olema nii:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB on NoSQL poe JSON dokumentide, kus pesastatud struktuurid on lubatud. Azure'i andmed Factory võimaldab kasutajal tähistamiseks hierarhia kaudu **nestingSeparator**, mis on "." selles näites. Eraldaja, mille koopia tegevuse luua kolm alla elementidega "Nimi" objekti esmalt teise eesnime ja perekonnanime, "Name.First", "Name.Middle" ja "Name.Last" tabeli määratlus.

## <a name="azure-documentdb-linked-service-properties"></a>Azure'i DocumentDB lingitud teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud Azure'i DocumentDB lingitud teenuse kirjeldus. 

| **Atribuut** | **Kirjeldus** | **Nõutav** |
| -------- | ----------- | --------- |
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **DocumentDb** | Jah |
| connectionString | Määrake Azure'i DocumentDB andmebaasiga ühenduse loomiseks vajalik teave. | Jah |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure'i DocumentDB andmekomplekti tüüp atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate vaadake artiklis [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotisi, sh struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).
 
Jaotise typeProperties erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **DocumentDbCollection** on järgmised atribuudid.

| **Atribuut** | **Kirjeldus** | **Nõutav** |
| -------- | ----------- | -------- |
| collectionName | DocumentDB dokumendi saidikogumi nimi. | Jah |


Näide:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Skeemi järgi andmete Factory
Skeemi tasuta andmed poe nagu DocumentDB puhul andmete Factory teenuse tuletab skeemiga ühel järgmistest viisidest:  

1.  Kui määrate atribuudi **struktuuri** andmekomplekti määratluse abil andmete struktuuri, andmete Factory teenuse kiitusega selle struktuuri skeem. Sel juhul, kui rida ei sisalda veeru väärtuse, antakse tühiväärtus seda.
2.  Kui te ei määra, kasutades atribuudi **struktuuri** andmekomplekti määratluse andmete struktuuri, tuletab andmete Factory teenuse skeemi abil esimese rea andmeid. Sel juhul kui esimene rida ei sisalda täielik skeemiga, veerge on puudu kopeerimisel tulemus.

Seetõttu skeemi tasuta andmeallikate jaoks parim on andmete **struktuuri** atribuudi struktuuri määrata.

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure'i DocumentDB Kopeeri tegevuse tüüp atribuudid

Jaotised ja atribuute määratlemine tegevuste täieliku loendi leiate vaadake [Loomise torujuhtmete](data-factory-create-pipelines.md) artikkel. Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitika atribuudid on saadaval kõigi tegevuse tüüp.
 
**Märkus:** Kopeeri tegevuse võtab ainult üks sisendit ja annab ainult ühe väljundi.

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad Teisalt iga tegevuse tüüp ja Kopeeri tegevuste korral need sõltuvad allikate ja neeldajate tüübid.

Kopeeri tegevuste korral kui allika tüüp **DocumentDbCollectionSource** järgmised atribuudid on saadaval jaotises **typeProperties** :

| **Atribuut** | **Kirjeldus** | **Lubatud väärtus** | **Nõutav** |
| ------------ | --------------- | ------------------ | ------------ |
| päringu | Määrake päringu andmeid lugeda. | Päringustringi DocumentDB ei toeta. <br/><br/>Näide:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Ei <br/><br/>Kui pole määratud, käivitatakse SQL-lause:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Erimärk näitamaks, et dokument on pesastatud | Suvaline märk. <br/><br/>DocumentDB on NoSQL poe JSON dokumentide, kus pesastatud struktuurid on lubatud. Azure'i andmed Factory võimaldab kasutajal tähistamiseks hierarhia kaudu nestingSeparator, mis on "." eespool toodud. Eraldaja, mille koopia tegevuse luua kolm alla elementidega "Nimi" objekti esmalt teise eesnime ja perekonnanime, "Name.First", "Name.Middle" ja "Name.Last" tabeli määratlus. | Ei

**DocumentDbCollectionSink** toetab järgmised atribuudid.

| **Atribuut** | **Kirjeldus** | **Lubatud väärtus** | **Nõutav** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Erimärgi andmeallika veeru nimi tähistamiseks pesastatud dokument on vaja. <br/><br/>Näiteks kohal: `Name.First` väljund tabel annab tulemiks järgmised JSON struktuuri DocumentDB dokumenti:<br/><br/>"Nimi": {}<br/>  "First": "John"<br/>}, | Pesastustasemete eraldamiseks kasutatav märk.<br/><br/>Vaikeväärtus on `.` (punkt). | Pesastustasemete eraldamiseks kasutatav märk. <br/><br/>Vaikeväärtus on `.` (punkt). | Ei | 
| writeBatchSize | Paralleelsetes taotluste DocumentDB teenusega luua dokumentide arv.<br/><br/>Jõudlus saate täpsustada, kui kopeerite andmeid DocumentDB selle atribuudi abil. Parema jõudluse võib juhtuda, kui teil suurendada writeBatchSize, kuna rohkem paralleelselt taotluste DocumentDB saadetakse. Aga peate vältimiseks pidurdamise, mis võivad põhjustada kuvatakse tõrketeade: "Päring on suur".<br/><br/>Pidurdamise otsustab hulk tegureid, sh dokumentide arv dokumentide terminite mahu, indekseerimine poliitika target saidikogumi jne. Kopeeri toimingute puhul saate kasutada parema kogumi (nt S3) on saadaval kõige läbilaskevõime (2 500 taotlemine üksused sekundis). | Täisarv | Ei (vaikimisi: 10000) |
| writeBatchTimeout | Oodake toimingu lõpuleviimiseks enne ilmneb ajalõpp aega. | kuuline ajavahemik<br/><br/> Näide: "00: 30:00" (30 minuti). | Ei |
 
## <a name="appendix"></a>Lisa
1. **Küsimus:**  
    ei kopeeri tegevuse tugi update olemasolevate kirjete?

    **Vastus:**  
    ei.

2. **Küsimus:**  
    kuidas saab uuesti koopia DocumentDB toimida juba kopeeritud kirjete?

    **Vastus:**  
    kui kirjed on välja "ID" ja koopia toiming proovib sama ID-ga kirje lisamiseks, kopeerimine põhjustab ilmnes tõrge.  
 
3. **Küsimus:** 
    ei toeta andmete Factory [vahemikus või räsi-i põhiste andmete eraldamine](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Vastus:** 
    ei. 
4. **Küsimus:** 
    saate määrata, mitu DocumentDB saidikogumi tabeli?
    
    **Vastus:** 
    ei. Praegu saab määrata ainult ühe saidikogumi.
     
## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.
