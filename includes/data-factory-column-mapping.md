## <a name="column-mapping-with-translator-rules"></a>Veeru vastendamise translator reeglite abil
Veeru vastenduse saab määrata, kuidas veergude määratud "struktuuri" kaardil allikas tabeli veergude määratud valamu tabeli "struktuuri". **Veeruvastenduse** atribuut on saadaval Kopeeri tegevuse **typeProperties** osas.

Veeru vastendamise toetab järgmistel juhtudel:

- Kõik veerud tabelisse "struktuur" on vastendatud valamu tabeli "struktuur" kõik veerud.
- Kõigi veergude valamu tabeli "struktuur" veergu lähtetabeli "struktuur" Alamhulk on vastendatud.

Järgmised on tõrge tingimused ja tulemuseks erandi:

- Veergu või rohkem veerge "struktuuri" valamu tabeli kui määratud vastendust.
- Vastenduse dubleerida.
- SQL-päringu tulem ei saa veeru nimi, mis on määratud vastendust.

## <a name="column-mapping-samples"></a>Veeru vastendamise näidised
> [AZURE.NOTE] Proovide allpool on Azure SQL-i ja Azure'i bloobimälu, kuid mis tahes andmesalve, mis toetab ristküliku andmekomplektide rakenduvad. On teil kohandada andmekomplekti ja lingitud teenuse määratlused näidetes all osutamiseks oluline andmeallika andmed. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Näide 1 – veeru vastendus kaudu Azure SQL Azure'i bloobimälu
Selles näites sisendtabel on struktuur ja selle Sihtaadress SQL Azure'i SQL-andmebaasi tabelisse.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
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

Selles valimis väljund tabel on struktuur ja see osutab on bloobimälu Azure'i bloobimälu sisse.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

JSON tegevuse jaoks on näidatud allpool. Veerud vastendada veergude valamu (**columnMappings**), kasutades **Translator** atribuut allikast.

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Veeru vastendamise flow:**

![Veeru vastendamise meilivoo](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Näide 2 – veeru abil päringu SQL-i kaudu Azure SQL Azure'i bloobimälu kaardistamine
Selles näites kasutatakse SQL-päringu andmete eraldamiseks Azure SQL-i asemel lihtsalt "struktuur" jaotises tabeli nimi ja veerunimesid määratlemine. 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

Sel juhul vastendada päringutulemite esmalt veerud, mis on määratud "struktuur" allikas. Järgmiseks allikast "struktuur" veerud on vastendatud veergude valamu "struktuur" määratud columnMappings reeglite abil.  Oletame, et päring tagastas 5 veergu, täiendavad kaks veergu ja seejärel nimetatud "struktuur" allikas.

**Veeru vastendamise meilivoo**

![Veeru vastendamise meilivoo-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







