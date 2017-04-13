## <a name="invoking-stored-procedure-for-sql-sink"></a>SQL-i valamu asutusesisestes salvestatud protseduur

Andmete kopeerimisel SQL serveri või Azure SQL-i ja SQL serveri andmebaasi kasutaja määratud salvestatud protseduur võib konfigureeritud ja kasutada täiendavaid parameetritega. 

Salvestatud protseduur saate kasutada, kui sisseehitatud Kopeeri menetlustele eesmärki. See on tavaliselt kasutada, kui lisatasu töötlemine (ühendamise veerud, kas soovite täiendavaid väärtuste sisestamine mitme tabeli...) vaja teha enne lähteandmete sihttabelis lõplik lisamist. 

Võite kasutada salvestatud toimingu valik. Järgmises näites kujutatakse salvestatud toimingu abil saate teha lihtne sisestamist andmebaasi tabelisse. 

**Väljundi andmekomplekti**

Selles näites on seatud tüüp: SqlServerTable. Määrake selleks AzureSqlTable Azure SQL-andmebaasi kasutamiseks. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Määratleda SqlSink jaotis Kopeeri tegevuse JSON järgmiselt. Salvestatud protseduur ajal insert andmete helistamiseks on vaja SqlWriterStoredProcedureName nii SqlWriterTableType atribuudid.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

Andmebaasi, määratleda SqlWriterStoredProcedureName salvestatud protseduur sama nimega. Käsitlemisel sisendandmete teie määratud allikas ja lisa tabelisse väljundi. Pange tähele, et parameetri nimi salvestatud protseduur peaks olema sama, mis on määratletud tabeli JSON faili tableName.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

Andmebaasi, määratleda SqlWriterTableType tabeli tüüp sama nimega. Pange tähele, et tabeli tüüp skeemi peaks olema sama, mis teie sisendandmete tagastatud skeemiga.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Salvestatud protseduur funktsioon ära [Table-Valued parameetrid](https://msdn.microsoft.com/library/bb675163.aspx).