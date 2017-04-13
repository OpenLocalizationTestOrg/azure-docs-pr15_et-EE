<properties
    pageTitle="Asutusesisese SQL Serverist andmete teisaldamine SQL Azure'i koos Azure'i andmed Factory | Azure'i"
    description="Saate häälestada ADF kohaletoimetamisel, mis komponeerib kaks andmete migreerimise tegevused, mida koos andmete teisaldamiseks igapäevaselt andmebaaside kohapealne ja pilve vahel."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Kohapealne SQL Serverist andmete teisaldamine SQL Azure'i Azure andmete Factory abil

Selles teemas kirjeldatakse teisaldamise andmete kohapealne SQL serveri andmebaasi SQL Azure'i andmebaasi kaudu Azure'i bloobimälu Azure'i andmed Factory (ADF) abil.

**Menüü** viivat teemadele, kus kirjeldatakse, kuidas andmeid neelata suunata keskkonnas, kus saate andmeid talletada ja töödelda meeskonnatöö teadus protsessi ajal.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Sissejuhatus: Mis on ADF ja millal seda kasutada andmete migreerimiseks?

Azure'i andmed Factory on täielikult hallatud pilvepõhist andmete integreerimine teenus, mis orchestrates ja automatiseerib liikumine ja andmete teisendus. Võtme ADF mudeli on kohaletoimetamisel. Müügivõimaluste on loogiline rühmitamine tegevusi, mis määratleb toimingute sooritamiseks andmekomplektide sisalduvate andmete kohta. Lingitud teenuste abil saate määratleda andmete Factory ühendamiseks andmeid ressursse vajalik teave.

ADF, olemasoleva töötlemise teenuste võivad koosneda andmed on tugevalt saadaval ja hallatavate pilveteenuses torustikud. Need andmed torujuhtmete saab ajastada neelata, koostada, muuta, analüüsida ja avaldada andmeid ja ADF haldab ja orchestrates keerukate andmete ja töötlemine sõltuvused. Lahenduste kiiresti ehitatud ja kohapealse üha ühenduse pilves, juurutada ja Pilveteenusest andmeallikad.

Kaaluge ADF:

- Kui andmeid on vaja migreerida pidevalt hübriidjuurutuse stsenaariumi mis pääseb juurde nii kohapealne ja pilveteenuse ressursid 
- Kui andmed on tehingu või muuta või lisada, kui seda ei viida äriloogika on vaja. 

ADF võimaldab soovitud plaanimine ja jälgimine abil lihtsa JSON skripte, et hallata andmeid perioodiliselt liikumine. ADF on ka muid võimalusi, nagu keerukate toimingute tugi. ADF kohta leiate lisateavet teemast [Azure Factory (ADF)](https://azure.microsoft.com/services/data-factory/)dokumentatsioonis.


## <a name="scenario"></a>Stsenaarium

Me loodud ADF kohaletoimetamisel, mis komponeerib kaks andmete migreerimise tegevusi. Koos need andmed iga päev vahel liikumine kohapealne SQL-andmebaasi ja Azure SQL-andmebaasi pilveteenuses. Kahe tegevused on järgmised.

* andmete kopeerimine kohapealne SQL serveri andmebaasi konto Azure'i bloobimälu
* Azure'i bloobimälu konto Azure'i SQL-andmebaasi andmeid kopeerida.

>[AZURE.NOTE] Näidatud siin on kohandatud kaudu sätestatud ADF meeskond üksikasjalikumat õpetuse samme: [asutusesisestest allikatest ja pilve Andmehalduslüüsi andmete teisaldamine](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) viiteid selle teema jaotised on esitatud vajaduse korral.


## <a name="prereqs"></a>Eeltingimused
Selle õpetuse eeldab, et teil on:

* On **Azure tellimust**. Kui olete tellinud, te saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
* **Azure storage konto**. Kasutage andmete talletamiseks selles õpetuses Azure storage konto. Kui teil pole kontot Azure storage, leiate artiklist [Loo konto salvestusruumi](storage-create-storage-account.md#create-a-storage-account) . Kui olete loonud salvestusruumi konto, peate hankima konto võti talletamist juurdepääsuks kasutada. [Vaade, kopeerimine ja uuesti luua salvestusruumi Pääsuklahvide](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)kuvamine
* Juurdepääs **Azure'i SQL-andmebaas**. Kui häälestate peab Azure'i SQL-andmebaasi, tpoic, [Microsoft Azure'i SQL-andmebaasiga alustamine](../sql-database/sql-database-get-started.md) annab teavet ettevalmistamise Azure'i SQL-andmebaasi uue eksemplari.
* Installitud ja konfigureeritud **Azure PowerShelli** kohalikult. Juhiste saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

> [AZURE.NOTE] Seda toimingut kasutatakse [Azure portaali](https://portal.azure.com/).


##<a name="upload-data"></a>Teie asutusesisese SQL serveri andmeid üleslaadimine

Kasutame [NYC takso andmekomplekti](http://chriswhong.com/open-data/foil_nyc_taxi/) migreerimisprotsessi näitamiseks. Andmekomplekti NYC takso on saadaval, kui märkida, et postituse Azure Bloobivahemälu salvestusruumi [NYC takso andmed](http://www.andresmh.com/nyctaxitrips/). Andmed on kaks faili, trip_data.csv fail, mis sisaldab reisi üksikasjad, ja trip_far.csv faili, mis sisaldab iga reisi makstud üksikasju. Valimi ja kirjeldust need failid on toodud [NYC takso reisi andmekomplekti kirjeldus](machine-learning-data-science-process-sql-walkthrough.md#dataset).


Saate kohandada oma andmete käivad või NYC takso andmekomplekti abil kirjeldatud juhised. Üleslaadimiseks NYC takso andmekomplekti asutusesisese SQL Serveri andmebaasiga, toimige kirjeldatud [Hulgi andmete importimine SQL serveri andmebaasi](machine-learning-data-science-process-sql-walkthrough.md#dbload). Need juhised on mõeldud SQL serveri kohta on Azure virtuaalse masina, kuid üleslaadimise asutusesisese SQL serveriga on samad.


##<a name="create-adf"></a>Azure'i andmed on Factory loomine

Juhised uue Azure'i andmed Factory ja ressursirühma [Azure portaali](https://portal.azure.com/) loomine pakutakse [loomine mõne Azure'i andmed Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Uue ADF eksemplari *adfdsp* ja ressursside rühma, mis on loodud *adfdsprg*nimi.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Installima ja konfigureerima Andmehalduslüüsi häälestamine

Teie asutusesisese SQL serveriga töötamiseks on Azure andmete factory torujuhtmetes lubamiseks peate lisada selle lingitud teenuse andmete factory. Kohapealne SQL serveri lingitud teenuse loomiseks peate tegema järgmist:

- Laadige alla ja installige Microsoft Andmehalduslüüsi kohapealne arvutisse. 
- lingitud teenuse kohapealse andmeallika kasutada lüüsi konfigureerimine. 

Andmehalduslüüsi serializes ja deserializes arvutis, kuhu see on majutatud andmete allikas ja valamu.

Häälestusjuhiseid ja Andmehalduslüüsi kohta leiate teemast [andmete asutusesisestest allikatest ja Andmehalduslüüsi pilve vahel liikumine](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Lingitud teenuste ressursid andmetega ühenduse loomine

Lingitud teenuse määratleb Azure'i andmed Factory ühendamiseks andmeid ressursi jaoks vajalik teave. Samm-sammult protseduur loomine lingitud teenused on toodud [luua lingitud teenused](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

Selle stsenaariumi, mille jaoks on vaja lingitud teenused on meil kolm ressursse.

1. [Lingitud kohapealne SQL Serveri teenuse](#adf-linked-service-onprem-sql)
2. [Lingitud teenuse Azure'i bloobimälu](#adf-linked-service-blob-store)
3. [Lingitud teenuse Azure SQL-andmebaas](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Lingitud teenuse kohapealne SQL Serveri andmebaasiga

Lingitud kohapealne SQL Serveri teenuse loomiseks tehke järgmist.

- Klõpsake **Andmesalve** ADF sihtleht Azure'i klassikaline portaalis 
- Valige **SQL-i** ja sisestage *kasutajanimi* ja *parool* asutusesisese SQL serveri. Peate selle serveri nimi Sisestage **nõuetele täielikult vastav serverinimi kurakriipsu eksemplari nimi (servername\instancename)**. Lingitud teenuse *adfonpremsql*nimi.

###<a name="adf-linked-service-blob-store"></a>Lingitud teenuse bloobimälu

Lingitud teenuse Azure'i bloobimälu konto loomiseks tehke järgmist.

- Klõpsake **Andmesalve** ADF sihtleht Azure'i klassikaline portaalis
- Valige **Azure Storage konto** 
- Sisestage Azure'i bloobimälu konto võti ja container nimi. Lingitud teenuse *adfds*nimi.

###<a name="adf-linked-service-azure-sql"></a>Lingitud teenuse Azure SQL-andmebaas

Lingitud teenuse SQL Azure'i andmebaasi loomiseks tehke järgmist.

- Klõpsake **Andmesalve** ADF sihtleht Azure'i klassikaline portaalis
- Valige **Azure SQL-i** ja sisestage *kasutajanimi* ja *parool* Azure'i SQL-andmebaasi. *Kasutajanimi* peab olema määratud *user@servername*.   


##<a name="adf-tables"></a>Ülesande määratlemine ja määrata, kuidas kogumid juurdepääsu tabelite loomine

Saate luua tabeli struktuuri, asukoha ja andmekogumite kättesaadavus koos järgmist skripti alusel määratud. JSON-faili abil saate määratleda tabelid. Nende failide struktuuri kohta leiate lisateavet teemast [andmekomplektide](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Täidate peaks selle `Add-AzureAccount` cmdlet-käsu [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet kinnitada, et paremale Azure'i tellimus on valitud käsu täitmise enne. Selle cmdlet-käsu dokumendid, vaadake [Lisa-AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx).

Tabelite JSON-põhiste määratlused kasutada järgmisi nimesid:

* kohapealne SQL serveri **tabeli nimi** on *nyctaxi_data*
* Azure'i bloobimälu konto **container nimi** on *ümbrisenimi*  

Kolme tabeli määratlused on vaja see ADF müügivõimaluste:

1. [SQL-i kohapealne tabel](#adf-table-onprem-sql)
2. [Tabeli bloobimälu](#adf-table-blob-store)
3. [SQL Azure'i tabeli](#adf-table-azure-sql)

> [AZURE.NOTE]  Neid toiminguid Azure PowerShelli abil saate määrata ja luua ADF tegevusi. Kuid järgmised toimingud võimalik teha ka Azure'i portaalis. Lisateavet leiate teemast [sisendit loomine ja andmekomplektide väljund](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>SQL-i kohapealne tabel

Tabeli määratlus asutusesisese SQL Server on määratud järgmised JSON-failis:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Veeru nimed siin ei kaasata. Saate valida sub veerunimede kohta, sh neid siin (üksikasjad sisse [ADF dokumentide](../data-factory/data-factory-data-movement-activities.md ) teema.

Kopeerige tabeli JSON määratluse faili nimega *onpremtabledef.json* fail ja salvestage see tuntud asukohta (siin eeldatakse, et *C:\temp\onpremtabledef.json*). Loo tabel ADF järgmine Azure PowerShelli cmdlet-käsk:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>Tabeli bloobimälu
Tabeli jaoks bloobimälu Väljastuskoht määratlus on (kaardid sissevõetava andmed kohapealne Azure'i bloobimälu) järgmist:

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Kopeerige tabeli JSON määratluse faili nimega *bloboutputtabledef.json* fail ja salvestage see tuntud asukohta (siin eeldatakse, et *C:\temp\bloboutputtabledef.json*). Loo tabel ADF järgmine Azure PowerShelli cmdlet-käsk:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure'i tabeli
SQL Azure'i tabeli määratlus väljund on järgmine (seda skeemi kaardid andmete toomiseks soovitud bloobimälu):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Kopeerige tabeli JSON määratluse faili nimega *AzureSqlTable.json* fail ja salvestage see tuntud asukohta (siin eeldatakse, et *C:\temp\AzureSqlTable.json*). Loo tabel ADF järgmine Azure PowerShelli cmdlet-käsk:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Ülesande määratlemine ja tulemas loomine

Määrake tegevusi, mis kuuluvad tulemas ja luua tulemas skripti alusel järgmistest toimingutest. JSON faili kasutatakse atribuutide müügivõimaluste määratlemine.

* Skripti eeldab, et **torujuhtme nimi** on *AMLDSProcessPipeline*.
* Samuti võtke arvesse, et me määrata ajavahemik, müügivõimaluste igapäevaselt täita ja kasutada vaikimisi täitmisaeg töö (00; UTC).

> [AZURE.NOTE]Järgmistest toimingutest Azure PowerShelli abil saate määrata ja luua ADF kohaletoimetamisel. Kuid see toiming ka suunamist Azure'i portaalis. Lisateavet leiate teemast [loomine ja käivitamine müügivõimaluste](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Kasutades tabelit kasutatakse varem, müügivõimaluste definitsiooni ADF on määratletud järgmiselt:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Kopeerige see JSON määratlus tulemas fail nimega *pipelinedef.json* fail ja salvestage see tuntud asukohta (siin eeldatakse, et *C:\temp\pipelinedef.json*). Loo tulemas ADF järgmine Azure PowerShelli cmdlet-käsk:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Veenduge, et näete müügivõimaluste ADF Azure'i klassikaline portaali sisse, kuvatakse järgmine (kui klõpsate diagrammil)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Tulemas käivitamine
Tulemas saate nüüd käivitage järgmine käsk:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*Alguskuupäev* ja *lõppkuupäev* parameetrite väärtused vaja tegelikke kuupäevi, mille vahel soovite müügivõimaluste käivitamiseks asendada.

Kui tulemas aktiveeritakse, peaks olema vaadata andmeid, kuvataks bloobimälu, päevas ühe faili jaoks valitud ümbrises.

Pange tähele, et saaksime on kasutada funktsioonid ADF Triibu andmetega sammhaaval. Kuidas see ja muude võimaluste esitatud ADF kohta leiate lisateavet teemast [ADF dokumentatsiooni](https://azure.microsoft.com/services/data-factory/).
