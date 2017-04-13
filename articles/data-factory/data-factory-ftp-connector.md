<properties 
    pageTitle="Andmete teisaldamine FTP-serverist | Microsoft Azure'i" 
    description="Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory FTP-serverist." 
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
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Andmete teisaldamine FTP server Azure'i andmed Factory abil
See artikkel kirjeldab Kopeeri tegevuse kasutamine Azure'i andmed Factory andmete teisaldamiseks serverist toetatud valamu andmed salvestada. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis, mis esitab ülevaate andmete liikumine Kopeeri tegevuste ja andmete poed Toetatud andmeallikate/heitmetega loendit. 

Andmete factory toetab praegu ainult jooksva andmeid FTP-serverist muid andmeid talletab, kuid mitte muid andmeid salvestab andmete teisaldamine serverist. See toetab asutusesiseselt ja Pilveteenusest FTP-servereid. 

Kui andmete liiguvad **kohapealse** FTP-serverist cloud andmete poe (näide: Azure'i bloobimälu), installida ja kasutada Andmehalduslüüsi. Andmehalduslüüs on kliendi installitud kohapealse arvutisse, mis võimaldab pilveteenustega ühenduse loomiseks kohapealse ressursi. Lüüsi üksikasjad leiate [Andmehalduslüüsi](data-factory-data-management-gateway.md) . Artiklist [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) üksikasjalikud juhised lüüsi häälestamise ja seda kasutama. Lüüsi abil ühenduse serverist isegi juhul, kui server on Azure IaaS virtuaalse masina (VM). 

Saate installida lüüsi kohapealse samasse arvutisse või Azure IaaS VM nimega FTP-serveris. Kuid soovitame lüüsi installida eraldi arvutisse või eraldi Azure IaaS VM vältimiseks ressursi väide ja parema jõudluse. Lüüsi eraldi arvutisse installimisel masina peaks olema juurdepääs FTP-serveris. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmed FTP-serverist on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmised näited annavad valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Näide: Andmete kopeerimine FTP server Azure'i bloobimälu

See näide näitab, kuidas serverist andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

- Lingitud teenuse tüüpi [FtpServer](#ftp-linked-service-properties).
- Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [FileShare](#fileshare-dataset-type-properties).
- Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [FileSystemSource](#ftp-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmed FTP-serverist on Azure'i bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

**FTP lingitud teenus** Selles näites kasutatakse funktsiooni elementaarautentimine kasutajanime ja parooliga lihttekstina. Saate kasutada ühte järgmistest viisidest: 

- Anonüümne autentimine 
- Krüptitud mandaadiga lihttekstautentimine
- FTP üle SSL/TLS (FTPS)

Vaadake [FTP lingitud teenuse](#ftp-linked-service-properties) osa autentimine, saate kasutada erinevat tüüpi. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
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

**FTP Sisestuskeel andmekomplekti** Tuvastatavate viitab FTP kausta `mysharedfolder` ja faili `test.csv`. Tulemas kopeerib faili sihtkohta. 

Säte "välise": "true" teatab andmete Factory teenus, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
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

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **FileSystemSource** ja **valamu** tüüp väärtuseks **BlobSink**. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
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
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>FTP lingitud teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud FTP lingitud teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav | Vaikimisi |
| -------- | ----------- | -------- | ------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud FtpServer | Jah | &nbsp;
| Host | FTP-serveri nimi või IP-aadress | Jah | &nbsp;
| authenticationType | Määrake autentimistüüp | Jah | Tavaline, anonüümne |
| kasutajanimi | Kasutajad, kellel on juurdepääs FTP-server | Ei | &nbsp;
| parooli | Parooli kasutaja (kasutajanimi) | Ei | &nbsp;
| encryptedCredential | Krüptitud mandaati juurdepääsu FTP-server | Ei | &nbsp;
| gatewayName | Ühenduse loomiseks kohapealse serverist Andmehalduslüüsi lüüsi nimi | Ei    | &nbsp;
| Port | Pordi aluseks on listening FTP-server | Ei | 21 |
| enableSsl | Saate määrata, kas kasutada FTP üle SSL/TLS-kanalit | Ei | True | 
| enableServerCertificateValidation | Määrake, kas soovite lubada serveri SSL-i serdi valideerimine kasutamisel FTP üle SSL/TLS-kanalit | Ei | True | 

### <a name="using-anonymous-authentication"></a>Anonüümne autentimine abil

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Kasutajanime ja parooli abil lihtsa autentimise lihttekstina
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Portide, enableSsl, enableServerCertificateValidation abil

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Autentimis- ja lüüsi encryptedCredential abil

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Üksikasjad kohapealse FTP andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>FTP Kopeeri tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, tüüp atribuudid sõltuvad allikate ja neeldajate tüübid.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.

## <a name="next-steps"></a>Järgmised sammud
Lugege järgmisi artikleid: 

- Üksikasjalikud juhised loomine müügivõimaluste kopeerimine tegevuse [Kopeeri tegevuse õpetuse](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) . 
