<properties 
    pageTitle="Õpetus: Luua müügivõimaluste Kopeeri tegevuse Visual Studio abil | Microsoft Azure'i" 
    description="Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse Visual Studio abil." 
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
    ms.topic="get-started-article" 
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Õpetus: Luua müügivõimaluste Kopeeri tegevuse Visual Studio abil
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopeerige viisard](data-factory-copy-data-wizard-tutorial.md)
- [Azure'i portaal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure'i ressursihaldur Mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-GA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-I API-GA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Selle õpetuse näidatakse, kuidas luua ja jälgida Azure'i andmed factory, mis Visual Studio abil. Müügivõimaluste rakenduses andmete factory kasutab Kopeeri tegevuse Azure'i bloobimälu andmete kopeerimine Azure'i SQL-andmebaasi.

Siin on toimingud, saate teha selle õpetuse osana.

1. Luua kaks lingitud teenuste: **AzureStorageLinkedService1** ja **AzureSqlinkedService1**. 

    Funktsiooni AzureStorageLinkedService1 lingid on Azure salvestusruumi ja AzureSqlLinkedService1 linke Azure SQL-andmebaasi andmete factory: **ADFTutorialDataFactoryVS**. Sisendandmete jaoks tulemas asub bloobimälu ümbrises Azure'i bloobimälu sisse ja väljundi andmed talletatakse SQL Azure'i andmebaasi tabelisse. Seega saate lisada need kaks andmete poed lingitud teenuste andmete factory.
2. Looge kaks andmekomplektide: **InputDataset** ja **OutputDataset**, mis tähistavad sisend andmeid, mis on talletatud andmed poed. 

    InputDataset, saate määrata bloobimälu ümbris, mis sisaldab soovitud bloobimälu lähteandmetega. OutputDataset, saate määrata SQL tabel, mis talletatakse väljundi andmeid. Saate määrata ka muid atribuute, nt struktuuri, kättesaadavus ja poliitika.
3. Saate luua müügivõimaluste nimega **ADFTutorialPipeline** on ADFTutorialDataFactoryVS sisse. 

    Tulemas on **Kopeeri tegevust** , mis on Azure input koopiaid andmete Bloobivahemälu väljundi Azure SQL-i tabelisse. Kopeeri tegevuse sooritab Azure'i andmed Factory andmete liikumine. Tegevuse on tootja globaalselt saadaval teenust, mida saate kopeerida andmeid erinevate andmete poed turvaline, usaldusväärseid ja scalable viisil. Vt [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel Kopeeri tegevuse üksikasjad. 
4. Saate luua andmete factory, nimega **VSTutorialFactory**. Juurutada andmete factory ja kõik andmed Factory üksuste (lingitud teenused, tabelite ja tulemas).    

## <a name="prerequisites"></a>Eeltingimused

1. Lugege läbi [Õpetuse ülevaate](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikli ja täitke juhised **nõutav** . 
2. Peate olema **administraator on seotud Azure** saama avaldada andmete Factory üksuste Azure'i andmed Factory.  
3. Peab teil olema teie arvutisse installitud järgmine: 
    - Visual Studio 2013 või Visual Studio 2015
    - Laadige alla Azure SDK Visual Studio 2013 või Visual Studio 2015. [Azure'i allalaadimine lehele](https://azure.microsoft.com/downloads/) liikumiseks ja klõpsake **VS 2013** või **.NET** jaotises **VS 2015** .
    - Visual Studio uusima Azure'i andmed Factory lisandmooduli allalaadimine: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) või [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Selle lisandmooduli saate värskendada, tehes järgmist: klõpsake menüü **Tööriistad** -> **laiendid ja värskenduste** -> **Online'i** -> **Visual Studio galerii** -> **Microsoft Azure'i Factory Tools for Visual Studio** -> **värskendada**.

## <a name="create-visual-studio-project"></a>Visual Studio projekti loomine 
1. Käivitage **Visual Studio 2013**. Klõpsake menüüd **fail**, käsku **Uus**ja valige **Project**. Peaksite nägema dialoogiboksi **Uus projekt** .  
2. Dialoogiboksis **Uus projekt** valige **DataFactory** Mall ja klõpsake nuppu **Tühjenda andmete Factory projekti**. Kui te ei näe DataFactory Mall, sulgege Visual Studios, installige Azure'i SDK Visual Studio 2013 ja Visual Studio uuesti.  

    ![Dialoogiboks uus projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Sisestage projekt, **asukoht**ja nimi **lahenduse** **nimi** ja klõpsake nuppu **OK**.

    ![Solution Exploreris](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Looge lingitud teenused
Lingitud teenuste andmete linkimine või teenused on Azure andmete factory arvutada. Lugege teemat [toetatud andmete talletab](data-factory-data-movement-activities.md##supported-data-stores-and-formats) allikate ja neeldajate Kopeeri tegevuse ei toeta. Vt [arvutada lingitud teenuste](data-factory-compute-linked-services.md) jaoks ei toeta andmete Factory Arvuta teenuste loend. Selles õpetuses ei kasutada mis tahes Arvuta teenust. 

Selles etapis tuleb teil luua kaks lingitud teenuste: **AzureStorageLinkedService1** ja **AzureSqlLinkedService1**. AzureStorageLinkedService1 lingitud teenuse lingid konto Azure salvestusruumi ja AzureSqlLinkedService linke Azure SQL-andmebaasi andmete factory: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Azure Storage lingitud teenuse loomine

4. Paremklõpsake solution Exploreris **Lingitud teenused** , osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**.      
5. Dialoogiboksis **Lisa uus üksus** **Azure'i salvestusteenus lingitud** loendist ja klõpsake nuppu **Lisa**. 

    ![Uus lingitud teenus](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Asendage `<accountname>` ja `<accountkey>`* konto Azure salvestusruum ja selle klahvi nimi. 

    ![Azure'i salvestusruumi lingitud teenus](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Salvestage fail **AzureStorageLinkedService1.json** .

> Üksikasjad JSON atribuutide kohta leiate [Azure'i bloobimälu, et andmete teisaldamine](data-factory-azure-blob-connector.md#azure-storage-linked-service) .

### <a name="create-the-azure-sql-linked-service"></a>Looge lingitud Azure SQL-i teenus

5. Paremklõpsake **Solution Exploreris** **Lingitud teenuste** sõlme uuesti, osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**. 
6. Sel ajal, valige **Azure SQL-i lingitud teenust**ja klõpsake nuppu **Lisa**. 
7. **AzureSqlLinkedService1.json faili**asendada `<servername>`, `<databasename>`, `<username@servername>`, ja `<password>` nimedega oma Azure SQL-i server, andmebaasi, konto ja parool.    
8.  Salvestage fail **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Üksikasjad JSON atribuutide kohta leiate [Azure'i SQL-andmebaasi, et andmete teisaldamine](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-datasets"></a>Andmekomplektide loomine
Eelmises juhises loodud lingitud teenuste **AzureStorageLinkedService1** ja **AzureSqlLinkedService1** Azure Storage konto ja Azure SQL-andmebaasi andmete factory link: **ADFTutorialDataFactory**. Selles etapis tuleb määratleda kaks kogumid - **InputDataset** ja **OutputDataset** -, mis tähistavad sisend andmeid, mis on talletatud andmed salvestab, AzureStorageLinkedService1 ja AzureSqlLinkedService1 nimetatud. InputDataset, saate määrata bloobimälu ümbris, mis sisaldab soovitud bloobimälu lähteandmetega. OutputDataset, saate määrata SQL tabel, mis talletatakse väljundi andmeid.

### <a name="create-input-dataset"></a>Sisestuskeel andmekomplekti loomine
Selles etapis loote nimega **InputDataset** , mis viitab bloobimälu container Azure Storage, mis tähistab **AzureStorageLinkedService1** lingitud teenuse Andmekomplekt. Tabeli on ristküliku andmekomplekti ja ainuke andmekomplekti praegu toetatud. 

9. Paremklõpsake **Solution Exploreris** **tabelid** , osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**.
10. **Azure'i bloobimälu**Valige dialoogiboksis **Lisa uus üksus** ja klõpsake nuppu **Lisa**.   
10. JSON teksti asendamine järgmine tekst ja **AzureBlobLocation1.json** faili salvestada. 

        {
          "name": "InputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Võtke arvesse järgmist. 
    
    - andmekomplekti **Tüüp** on seatud **AzureBlob**.
    - **linkedServiceName** on seatud **AzureStorageLinkedService**. Samm 2 loodud lingitud teenus.
    - **folderPath** on seatud **adftutorial** ümbrises. Saate määrata ka mõne bloobimälu abil atribuudi **nimi** kausta nimi. Kuna siis määrate selle bloobimälu nime, peetakse mõne sisendandmete kõik plekid ümbrises andmeid.  
    - vormingu **Tüüp** väärtuseks **tekstivorming**
    - Koma (**columnDelimiter**) eraldatud tekstifail – **eesnimi** ja **perekonnanimi** – on kaks väljad 
    - **Kättesaadavus** väärtuseks **tunni** (**sagedus** väärtuseks **tund** ja **intervall** väärtuseks **1**). Seega andmete Factory otsitakse sisendandmete tunnis bloobimälu container (**adftutorial**) määratud juurkaustas. 
    
    kui te ei määra **faili nimi** soovitud **Sisestuskeel** andmekomplekti, käsitletakse sisendina kõik failid plekid Sisestuskeel kaustast (**folderPath**). Kui määrate faili nimi soovitud JSON, käsitletakse ainult määratud fail/bloobimälu asn sisestatud.
 
    Kui te ei määra on **väljund tabeli** **nimi** , **folderPath** failid teie enda loodud on nimega järgmises vormingus: andmed. &lt;Guid\&gt;. txt (näide: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **FolderPath** ja **failinimi** dünaamiliselt põhjal **SliceStart** aja määramiseks kasutage **partitionedBy** atribuuti. Järgmises näites folderPath kasutab aasta, kuu ja päeva SliceStart (sektorit, et töödelda alguskellaaeg) kaudu ja failinimi kasutab funktsiooni SliceStart tund. Näiteks kui tükk on koostatud 2016-09-20T08:00:00, wikidatagateway/wikisampledataout/2016/09/20 on seatud soovitud kaustanimi ja faili nimi on seatud 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> Üksikasjad JSON atribuutide kohta leiate [Azure'i bloobimälu, et andmete teisaldamine](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .

### <a name="create-output-dataset"></a>Väljundi andmekomplekti loomine
Selles etapis loote väljundi andmekomplekti, mis nimega **OutputDataset**. Tuvastatavate osutab tabelisse SQL Azure SQL andmebaasis, mis tähistab **AzureSqlLinkedService1**. 

11. Paremklõpsake **Solution Exploreris** **tabelite** uuesti, osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**.
12. Valige **Azure SQL-i**dialoogiboksis **Lisa uus üksus** ja klõpsake nuppu **Lisa**. 
13. JSON teksti asendamine järgmised JSON ja **AzureSqlTableLocation1.json** faili salvestada.

        {
          "name": "OutputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Võtke arvesse järgmist. 
    
    - andmekomplekti **Tüüp** on seatud **AzureSQLTable**.
    - **linkedServiceName** on seatud **AzureSqlLinkedService** (loodud lingitud teenuse etapp 2).
    - **TableName** on seatud **emp**.
    - Andmebaasi emp tabelis on kolm veergu- **ID**, **eesnimi**ja **perekonnanimi** –. ID on identiteedi veeru, seega peate määrama ainult **eesnimi** ja **perekonnanimi** siin.
    - **Kättesaadavus** väärtuseks **tunni** (**sagedus** **tund** seadmine ja **intervall** väärtuseks **1**).  Andmete Factory teenuse loob mõne väljundi andmete sektorit iga tunni SQL Azure'i andmebaasi tabelisse **emp** .

> [AZURE.NOTE]
> Üksikasjad JSON atribuutide kohta leiate [Azure'i SQL-andmebaasi, et andmete teisaldamine](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-pipeline"></a>Müügivõimaluste loomine 
Olete loonud sisend lingitud teenuste ja tabelite siiani. Nüüd looge müügivõimaluste **Kopeeri tegevuste** kopeerimiseks andmed on Azure Bloobivahemälu Azure SQL-andmebaasiga. 


1. Paremklõpsake **Solution Exploreris** **torujuhtmete** , osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**.  
15. Valige **Kopeeri andmete müügivõimaluste** dialoogiboksis **Lisa uus üksus** ja klõpsake nuppu **Lisa**. 
16. Asendage funktsiooni JSON järgmised JSON ja **CopyActivity1.json** faili salvestada.
            
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    Võtke arvesse järgmist.

    - Jaotises tegevused on ainult üks tegevust, mille **Tüüp** on seatud **Kopeeri**.
    - Sisestusmeetodi tegevuse jaoks on määratud **InputDataset** ja väljundi tegevuse jaoks on määratud **OutputDataset**.
    - Jaotises **typeProperties** **BlobSource** on määratud Reaallika tüüp ja **SqlSink** on määratud valamu tüüp.

    Asendage praeguse päeva- ja **lõppaeg** väärtus järgmisest päevast **käivitamine** atribuudi väärtust. Saate määrata osa kuupäeva ja kellaaja osa kuupäev aja vahele jätta. Näiteks "2016-02-03", mis võrdub "2016-02-03T00:00:00Z"
    
    Mõlemad käivitamine ja end kuupäevade ja kellaaegade peab olema [ISO](http://en.wikipedia.org/wiki/ISO_8601)-vormingus. Näide: 2016-10-14T16:32:41Z. **Lõpuaeg** pole kohustuslik, kuid kasutame selles õpetuses. 
    
    Kui määrate atribuudi **end** väärtus arvutatakse kui "**start + 48 tundi**". Tulemas lõputult käivitamiseks määrata **9999-09-09** **end** atribuudi väärtust.
    
    Eelmises näites on 24 andmete sektorit iga andmete sektorit on saadud tunnis.

## <a name="publishdeploy-data-factory-entities"></a>Andmete Factory üksuste avaldamine ja juurutamine
Selles etapis tuleb teil avaldada andmete Factory üksuste (lingitud teenused, andmekomplektide ja müügivõimaluste) varem loodud. Saate määrata ka uute andmete factory korraldada need üksused luua nimi.  

18. Paremklõpsake Solution Exploreris projekti, ja klõpsake nuppu **Avalda**. 
19. Kui kuvatakse dialoogiboks **oma Microsofti kontoga sisse logida** , sisestage mandaat, mis on Azure tellimuse konto ja klõpsake nuppu **Logi sisse**.
20. Peaksite nägema järgmine dialoogiboks:

    ![Dialoogiboks avaldamine](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Andmete factory lehe konfigureerimine, tehke järgmist. 
    1. **Looge uus andmete Factory** suvand.
    2. Sisestage **VSTutorialFactory** **nimi**.  
    
        > [AZURE.IMPORTANT]  
        > Azure'i andmed factory nimi peab olema globaalselt kordumatu. Kui saate tõrketeate kohta andmete factory nime avaldamisel, muuta andmete factory (nt yournameVSTutorialFactory) nime ja proovige uuesti avaldada. Nimede reeglid andmete Factory artefakte [Andmete Factory - nime andmise reeglid](data-factory-naming-rules.md) teemat.     
    3. Valige Azure tellimuse **tellimuse** välja.
     
        > [AZURE.IMPORTANT]Kui te ei näe mis tahes tellimus, veenduge, et te sisse loginud kontoga, millel on administraatori või co-admin tellimus.  
    4. Valige **ressursirühm** andmete Factory luua. 5. Valige soovitud andmed factory **piirkond** . Rippmenüü loendis kuvatakse ainult need regioonid, mille andmeid Factory teenus ei toeta.
6. Klõpsake nuppu **Järgmine** lehe **Avaldamine üksuste** kuvamiseks.
    
        ![Andmete factory lehe konfigureerimine](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. **Üksuste avaldamine** lehel tagada, et kõik andmed tehased üksused on valitud ja klõpsake nuppu **Järgmine** lehe **Kokkuvõte** kuvamiseks.
    
    ![Üksuste lehe avaldamine](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Kokkuvõte ja klõpsake nuppu **Järgmine** juurutamise protsessi käivitamiseks ja **Juurutamise oleku**vaatamine.

    ![Kokkuvõte lehe avaldamine](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. **Juurutamise oleku** leht, peaksite nägema oleku juurutamise käigus. Kui juurutamise on töö lõpetanud, klõpsake nuppu valmis. 
    ![Juurutamise oleku leht](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) meeles pidada järgmist: 

- Kui kuvatakse tõrketeade: "**see tellimus pole registreeritud kasutada nimeruumi Microsoft.DataFactory**", tehke ühte järgmistest ja proovige uuesti avaldada: 

    - Azure'i PowerShelli, käivitage järgmine käsk andmete Factory pakkuja registreerimiseks. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Käivitada järgmine käsk, et kinnitada, et andmete Factory, mis on registreeritud pakkuja. 
    
            Get-AzureRmResourceProvider
    - Azure'i tellimuse [Azure portaali](https://portal.azure.com) sisse login ja liikuge andmete Factory blade (või) andmete factory Azure portaali loomine. See toiming automaatselt registreerib pakkuja teie eest.
-   Andmete factory nimi võib registreerida DNS-i nimi tulevikus ja seega muutuvad avalikult nähtav.

> [AZURE.IMPORTANT] Andmete Factory eksemplari loomiseks peate olema administraator/co-admin Azure tellimuse

## <a name="summary"></a>Kokkuvõte
Selles õpetuses olete loonud mõne Azure'i andmed factory kopeerimiseks mõnda Azure'i andmed Bloobivahemälu Azure SQL-andmebaasi. Andmete factory, lingitud teenused, andmekomplektide ja müügivõimaluste loomiseks kasutatud Visual Studio. Siin on selles õpetuses tegite üksikasjalik juhiseid.  

1.  Loodud on Azure **andmete factory**.
2.  Loodud **lingitud teenused**.
    1. **Azure Storage** lingitud teenuse konto Azure Storage, mis hoiab sisendandmete link.    
    2. **Azure SQL-i** lingitud teenuse Azure SQL-i andmebaasi, mis hoiab andmeid väljundi link. 
3.  Loodud **andmekomplektide**, mis kirjeldavad sisendandmete ja väljundi andmete gaasijuhtmetena.
4.  **Müügivõimaluste** loodud **Kopeeri tegevuse** koos **BlobSource** allikas ja **SqlSink** valamu nimega. 


## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer abil saate vaadata andmete tehased

1. **Visual Studio**, klõpsake menüü **Vaade** ja käsku **Server Explorer**.
2. Serveri Exploreri aknas, laiendage **Azure** ja **Andmete Factory**laiendamine. Kui kuvatakse **Visual Studio sisse logida**, sisestage **konto** Azure tellimusega seostatud ja klõpsake nuppu **Jätka**. Sisestage **parool**ja klõpsake nuppu **Logi sisse**. Visual Studio proovib kõik Azure'i andmed tehased tellimuse kohta teabe saamiseks. Selle toimingu aknas **Andmete Factory ülesandeloendi** oleku kuvamine
    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Saate andmeid factory paremklõpsake ja valige eksportida andmed Factory uue projekti loomiseks Visual Studio projekti mõne olemasoleva factory andmete põhjal.
    ![Projekti VS factory andmete eksportimine](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Värskenda andmed Factory tööriistad Visual Studio
Azure'i andmed Factory tööriistad Visual Studio värskendamiseks tehke järgmist.

1. Klõpsake menüü **Tööriistad** ja valige **laiendid ja värskendused**. 
2. Valige vasakul paanil **värskendused** ja seejärel valige **Visual Studio galerii**.
4. **Visual Studio Azure'i andmed Factory tööriistad** ja klõpsake nuppu **Värskenda**. Kui te ei näe seda kirjet, on teil juba tööriistade uusim versioon. 

Leiate juhised, kuidas kasutada Azure portaali jälgida müügivõimaluste ja andmekomplektide selles õpetuses olete loonud [kuvari andmekomplektide ja kohaletoimetamisel](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) .

## <a name="see-also"></a>Vt ka
| Teema: | Kirjeldus |
| :---- | :---- |
| [Tegevuste andmete liikumine](data-factory-data-movement-activities.md) | Selles artiklis on toodud üksikasjalikku teavet saate kasutada õpetuse Kopeeri tegevuse kohta. |
| [Plaanimis- ja täitmise](data-factory-scheduling-and-execution.md) | Selles artiklis selgitatakse plaanimis- ja täitmise aspektide Azure'i andmed Factory rakenduse mudel. |
| [Torujuhtmete](data-factory-create-pipelines.md) | See artikkel aitab teil mõista torujuhtmed ja tegevusi Azure'i andmed Factory |
| [Andmekomplektide](data-factory-create-datasets.md) | See artikkel aitab teil mõista andmekogumite Azure'i andmed Factory.
| [Jälgimine ja haldamine torustike jälgimise App abil](data-factory-monitor-manage-app.md) | Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja silumine torujuhtmete jälgimine ja haldamine rakenduse abil. 
