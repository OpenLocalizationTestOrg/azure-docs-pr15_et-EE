### <a name="type-conversion-sample"></a>Tüübi teisendamine näidis
Järgmises näites on kopeerimise andmete lisamine bloobimälu Azure SQL-i tüüp teisendused.

Oletame, et bloobimälu andmekomplekti on CSV-vormingus ja 3 veerud. Üks neist on kuupäeva ja kellaaja veerg kohandatud kuupäeva ja kellaaja vormingu abil lühendatud Prantsuse nädalapäev nimede abil.

Määratlege bloobimälu allika andmekomplekti järgmiselt koos tüüp veergude määratlusi.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Antud SQL oleks tüübi .NET tüüp vastendamine tabelis kohal saate määratleda SQL Azure'i tabeli järgmine skeem.

| Veeru nimi | SQL-i tüüp |
| ----------- | -------- |
| kasutajanimi | suur täisarv |
| Nimi | teksti |
| lastlogindate | kuupäev ja kellaaeg |

Järgmisena Määratlege Azure SQL-i andmekomplekti järgmiselt. Märkus: Pole vaja määramiseks "struktuur" jaotis tüüp teabega, kuna tüüpi teavet on juba määratud päringu aluseks andmesalve.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Sel juhul automaatselt ei andmete factory teisenduste, sh kohandatud kuupäeva ja kellaaja väljale kuupäev ja kellaaeg vormindamine abil fr-fr kultuuri SQL Azure'i bloobimälu andmete teisaldamisel tüüp.


