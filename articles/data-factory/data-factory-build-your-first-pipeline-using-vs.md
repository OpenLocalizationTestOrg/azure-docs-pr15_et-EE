<properties
    pageTitle="Koostada esimese andmete factory (Visual Studio) | Microsoft Azure'i"
    description="Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste Visual Studio abil."
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
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Õpetus: Koosta oma Azure esimese andmete factory Microsoft Visual Studio abil
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-build-your-first-pipeline.md)
- [Azure'i portaal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShelli](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressursihaldur Mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-GA](data-factory-build-your-first-pipeline-using-rest-api.md)

Selles artiklis kasutada Microsoft Visual Studio oma esimese Azure'i andmed factory loomiseks.

## <a name="prerequisites"></a>Eeltingimused
1. Lugege läbi [Õpetuse ülevaate](data-factory-build-your-first-pipeline.md) artikli ja täitke juhised **nõutav** .
2. Peate olema **administraator on seotud Azure** saaksid avaldada andmete Factory Azure'i andmed Factory Visual Studio üksused.
3. Peab teil olema teie arvutisse installitud järgmine: 
    - Visual Studio 2013 või Visual Studio 2015
    - Laadige alla Azure SDK Visual Studio 2013 või Visual Studio 2015. [Azure'i allalaadimine lehele](https://azure.microsoft.com/downloads/) liikumiseks ja klõpsake **VS 2013** või **.NET** jaotises **VS 2015** .
    - Visual Studio uusima Azure'i andmed Factory lisandmooduli allalaadimine: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) või [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Selle lisandmooduli saate värskendada, tehes järgmist: klõpsake menüü **Tööriistad** -> **laiendid ja värskenduste** -> **Online'i** -> **Visual Studio galerii** -> **Microsoft Azure'i Factory Tools for Visual Studio** -> **värskendada**. 
 
Nüüd vaatame Visual Studio abil saate luua ka Azure'i andmed factory. 


## <a name="create-visual-studio-project"></a>Visual Studio projekti loomine 
1. Käivitage **Visual Studio 2013** või **Visual Studio 2015**. Klõpsake menüüd **fail**, käsku **Uus**ja valige **Project**. Peaksite nägema dialoogiboksi **Uus projekt** .  
2. Dialoogiboksis **Uus projekt** valige **DataFactory** Mall ja klõpsake nuppu **Tühjenda andmete Factory projekti**.   

    ![Dialoogiboks uus projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Sisestage projekt, **asukoht**ja nimi **lahenduse** **nimi** ja klõpsake nuppu **OK**.

    ![Solution Exploreris](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Looge lingitud teenused
Andmete factory võib olla üks või mitu torujuhtmete. Müügivõimaluste võib olla üks või mitu tegevusi. Näiteks Kopeeri tegevuse andmete kopeerimine mõnest muust allikast sihtkoha andmete pood ja Hdinsightiga taru tegevuste käivitamiseks taru skripti sisendandmete muuta. Lugege teemat [toetatud andmete talletab](data-factory-data-movement-activities.md##supported-data-stores-and-formats) allikate ja neeldajate Kopeeri tegevuse ei toeta. Vt [arvutada lingitud teenuste](data-factory-compute-linked-services.md) jaoks ei toeta andmete Factory Arvuta teenuste loend. 

Selles etapis tuleb teil link Azure Storage konto ja kuvatakse nõudmisel Windows Azure Hdinsightiga kobar oma andmete factory. Azure Storage konto hoiab selle valimi tulemas sisend- ja andmeid. Lingitud Hdinsightiga teenuse saab käivitada taru müügivõimaluste selles näites on märgitud. Tuvastada toodavad andmed poe/Arvuta teenuste kasutatakse teie stsenaariumi ja nende teenuste lingi andmed factory lingitud teenuste loomisega.  

Saate määrata nime ja sätted andmete factory hiljem kui avaldate oma andmete Factory lahendus.

#### <a name="create-azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus loomine
Selles etapis tuleb teil link Azure Storage konto oma andmete factory. Selles õpetuses mõeldud saate talletada sisend andmed ja HQL skriptifail sama Azure Storage konto. 

4. Paremklõpsake solution Exploreris **Lingitud teenused** , osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**.      
5. Dialoogiboksis **Lisa uus üksus** **Azure'i salvestusteenus lingitud** loendist ja klõpsake nuppu **Lisa**. 
3. Asendage **account_name** ja **accountkey** konto Azure salvestusruum ja selle klahvi nimi. Salvestusruumi Accessi tootenumbri hankimine leiate teemast [vaate, kopeerimine ja uuesti luua salvestusruumi kiirklahvide](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure'i salvestusruumi lingitud teenus](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Salvestage fail **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Windows Azure Hdinsightiga lingitud teenuse loomine
Selles etapis tuleb link kuvatakse nõudmisel Hdinsightiga kobar oma andmete factory. Hdinsightiga kobar käitusajal luuakse automaatselt ja kustutatud pärast seda teha töötlemiseks ja jõude jaoks määratud aja jooksul. Võite kasutada oma Hdinsightiga kobar asemel kuvatakse nõudmisel Hdinsightiga kobar. Üksikasjad leiate [Arvutada lingitud teenused](data-factory-compute-linked-services.md) . 

1. **Lahenduste Explorer**, paremklõpsake **Lingitud teenuste**, osutage käsule **Lisa**ja nuppu **Uus üksus**.
2. Valige **Hdinsightiga nõudmisel lingitud teenuste**ja klõpsake nuppu **Lisa**. 
3. Asendage **JSON** järgmist:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    Järgmises tabelis on ära toodud väljavõte kasutatakse atribuutide JSON kirjeldused:
    
    Atribuut | Kirjeldus
    -------- | -----------
    Versioon | Määrab, et versioon on Hdinsightiga loodud olema 3,2. 
    ClusterSize | Saate määrata Hdinsightiga kobar suurust. 
    TimeToLive | Mis määrab jõudeaja jaoks Hdinsightiga kobar, see enne kustutamist.
    linkedServiceName | Saate määrata salvestusruumi konto, mida kasutatakse Hdinsightiga loodud logide salvestamiseks

    Võtke arvesse järgmist. 
    
    - Andmete Factory loob **Windowsi-põhiste** Hdinsightiga kobar teile eelmise JSON. Te oleksite selle **Linuxi-põhiste** Hdinsightiga kobar loomine. Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - Võite kasutada **oma Hdinsightiga kobar** asemel kuvatakse nõudmisel Hdinsightiga kobar. Üksikasjad leiate [Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - Hdinsightiga kobar loob **vaikimisi container** bloobimälu, määratud JSON (**linkedServiceName**). Klaster kustutamisel ei kustutata Hdinsightiga selles ümbrises. Selline käitumine on kujundus. Nõudmisel Hdinsightiga lingitud teenusega Hdinsightiga kobar on loodud iga kord, kui mõnda sektorit töötlemise, kui ei ole olemasoleva reaalajas kobar (**timeToLive**). Klaster kustutatakse automaatselt, kui töötlemine on lõpule jõudnud.
    
        Nagu töödeldakse veel sektoritele, näete oma Azure'i bloobimälu palju ümbriste. Kui te ei pea neid tõrkeotsingu tööd, võite kustutage need salvestusruumi kuidagi vähendada. Järgige neid ümbriste nimed mustri: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Näiteks [Microsoft salvestusruumi Exploreri](http://storageexplorer.com/) abil saate kustutada ümbriste Azure'i bloobimälus.

    Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
4. Salvestage fail **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Andmekomplektide loomine
Selles etapis loote andmekomplektide sisend ja väljund andmete töötlemiseks taru. Vaadake neid andmekomplektide **AzureStorageLinkedService1** selles õpetuses olete loonud. Lingitud teenuse osutab Azure Storage konto ja andmekomplektide määrata talletamist, mis hoiab sisendit container, kausta, faili nimi ja andmete väljund.   

#### <a name="create-input-dataset"></a>Sisestuskeel andmekomplekti loomine

1. **Lahenduste Explorer**, paremklõpsake **tabelid**, osutage käsule **Lisa**ja nuppu **Uus üksus**. 
2. Valige loendist **Azure'i bloobimälu** , **InputDataSet.json**faili nime muutmine ja klõpsake nuppu **Lisa**.
3. Asendage **JSON** redaktoris järgmist: 

    JSON koodilõigu, loote nimega **AzureBlobInput** , mis tähistab sisendandmete tegevuse tulemas Andmekomplekt. Lisaks saate määrata, et sisendandmete asub bloobimälu ümbris nimega **adfgetstarted** ja kaust nimega **inputdata**
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Järgmises tabelis on ära toodud väljavõte kasutatakse atribuutide JSON kirjeldused:

  	| Atribuut | Kirjeldus |
  	| :------- | :---------- |
  	| tüüp | Atribuudi tüüp väärtuseks on seatud AzureBlob kuna andmete asub Azure'i bloobimälu. |  
  	| linkedServiceName | viitab AzureStorageLinkedService1, mis on varem loodud. |
  	| faili nimi | Atribuut pole kohustuslik. Kui argument seda atribuuti, kuvatakse folderPath kõik failid on peale. Sel juhul ainult input.log töödeldakse. |
  	| tüüp | Me kasutame tekstivorming on logifailide vormingus tekst. | 
  	| columnDelimiter | logifailide veerud on piiratud koma () |
  	| sagedus/intervall | määratud kuu ja intervalli sagedus on 1, mis tähendab, et Sisestuskeel sektorid on saadaval iga kuu. | 
  	| väliste | selle atribuudi väärtuseks on seatud väärtus true, kui sisendandmete on loodud andmete Factory teenus. | 
      
    
3. Salvestage fail **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Väljundi andmekomplekti loomine
Nüüd loote väljundi andmekomplekti tähistada väljundi Azure'i bloobimälu talletatud andmed. 

1. **Lahenduste Explorer**, paremklõpsake **tabelid**, osutage käsule **Lisa**ja nuppu **Uus üksus**. 
2. Valige loendist **Azure'i bloobimälu** , **OutputDataset.json**faili nime muutmine ja klõpsake nuppu **Lisa**. 
3. Asendage **JSON** redaktoris järgmist: 

    JSON koodilõigu, loote nimega **AzureBlobOutput**ja täpsustades andmeid, mis toodab taru skripti struktuuri Andmekomplekt. Lisaks saate määrata tulemused talletatavad bloobimälu ümbris nimega **adfgetstarted** ja kaust nimega **partitioneddata**. Jaotises **saadavuse** määrab, et väljundi andmekomplekti valmistatakse kuude kaupa.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Vaadake **loomine Sisestuskeel andmekomplekti** osa kirjeldused neid atribuute. Teil pole määratud välise atribuudi mõne väljundi andmekomplekti andmekomplekti on saadud andmete Factory teenuse.

4. Salvestage fail **OutputDataset.json** .


### <a name="create-pipeline"></a>Müügivõimaluste loomine
Selles juhises saate luua oma esimese müügivõimaluste **HDInsightHive** tegevuse. Sisestuskeel sektorit on saadaval iga kuu (sagedus: kuu, intervall: 1), toodetud väljundi sektorit iga kuu ja tegevuse ajasti atribuut on seatud ka kord kuus. Sätete väljundi andmekomplekti ja tegevuste ajasti peab vastama. Väljundi andmekomplekti on praegu, mis juhib ajakava, seega peate looma ka väljundi andmekomplekti isegi juhul, kui tegevuse pole mis tahes väljundi loomiseks. Kui tegevuse ei võta mis tahes sisendit, võite jätkata Sisestuskeel andmekomplekti loomine. Järgmised JSON kasutatakse atribuutide selgitatakse selle jaotise lõpus.

1. Paremklõpsake **Solution Exploreris** **torujuhtmete**, osutage käsule **Lisa**ja klõpsake **Uus üksus.** 
2. Valige loendist **Taru teisendus müügivõimaluste** ja klõpsake nuppu **Lisa**. 
3. Asendage **JSON** järgmised koodilõigu.

    > [AZURE.IMPORTANT] Asendage **storageaccountname** oma salvestusruumi konto nimi.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    JSON koodilõigu, loote kohaletoimetamisel, mis koosneb taru mõne Hdinsightiga kobar protsessi andmeid kasutava üks tegevus.
    
    JSON koodilõigu, loote kohaletoimetamisel, mis koosneb taru mõne Hdinsightiga kobar protsessi andmeid kasutava üks tegevus.
    
    Taru skriptifail **partitionweblogs.hql**, talletatakse Azure storage konto (määratud scriptLinkedService, mida nimetatakse **AzureStorageLinkedService1**) ja container **adfgetstarted** **skripti** kausta.

    **Määratleb** jaotise abil saate määrata edastatakse taru skripti taru konfiguratsiooni väärtustena käitusaja sätted (nt ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    Tulemas **algus** ja **lõpp** atribuutide määrab tulemas aktiivse perioodi.

    Saate määrata tegevuse JSON taru skript käivitatakse Arvuta, on **linkedServiceName** – **HDInsightOnDemandLinkedService**määratud.

    > [AZURE.NOTE] [Müügivõimaluste anatoomia](data-factory-create-pipelines.md#anatomy-of-a-pipeline) kohta vaadake teavet näites kasutatakse JSON atribuudid. 
3. Salvestage fail **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Sõltuv partitionweblogs.hql ja input.log lisamine 

1. Paremklõpsake **Solution Exploreris** aknas **sõltuvused** , osutage käsule **Lisa**ja nuppu **Olemasolev üksus**.  
2. Liikuge **C:\ADFGettingStarted** ja valige **partitionweblogs.hql**, **input.log** failid, ja klõpsake nuppu **Lisa**. Need kaks faili eeltingimused osana oli tekkinud [Õpetuse ülevaade](data-factory-build-your-first-pipeline.md).

Kui avaldate lahendus järgmise juhise juurde, **partitionweblogs.hql** faili üleslaadimisel skriptide kausta **adfgetstarted** bloobimälu ümbrises.   

### <a name="publishdeploy-data-factory-entities"></a>Andmete Factory üksuste avaldamine ja juurutamine

18. Paremklõpsake Solution Exploreris projekti, ja klõpsake nuppu **Avalda**. 
19. Kui kuvatakse dialoogiboks **oma Microsofti kontoga sisse logida** , sisestage mandaat, mis on Azure tellimuse konto ja klõpsake nuppu **Logi sisse**.
20. Peaksite nägema järgmine dialoogiboks:

    ![Dialoogiboks avaldamine](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Konfigureerimine factory lehel, tehke järgmist. 
    1. **Looge uus andmete Factory** suvand.
    2. Sisestage andmed factory kordumatu **nimi** . Näide: **FirstDataFactoryUsingVS09152016**. Nimi peab olema globaalselt kordumatu.  
    
    
        > [AZURE.IMPORTANT] Kui kuvatakse tõrketeade **andmete factory nimi "FirstDataFactoryUsingVS" on pole saadaval** , kui avaldamise, muutke nime (nt yournameFirstDataFactoryUsingVS). Nimede reeglid andmete Factory artefakte [Andmete Factory - nime andmise reeglid](data-factory-naming-rules.md) teemat.
3. Valige välja **tellimuse** jaoks õige tellimus.
     
     
        > [AZURE.IMPORTANT] Kui te ei näe mis tahes tellimus, veenduge, et te sisse loginud kontoga, millel on administraatori või co-admin tellimus.  
        
    4. Valige **ressursirühm** andmete Factory luua. 
    5. Valige soovitud andmed factory **piirkond** . 
    6. Klõpsake nuppu **Järgmine** lehe **Avaldamine üksuste** kuvamiseks. (Vajutage klahvi **MENÜÜS** liikumiseks välja nimi välja, kui nupp **Järgmine** on keelatud.) 
23. **Üksuste avaldamine** lehel tagada, et kõik andmed tehased üksused on valitud ja klõpsake nuppu **Järgmine** lehe **Kokkuvõte** kuvamiseks.     
24. Kokkuvõte ja klõpsake nuppu **Järgmine** juurutamise protsessi käivitamiseks ja **Juurutamise oleku**vaatamine.
25. **Juurutamise oleku** leht, peaksite nägema oleku juurutamise käigus. Kui juurutamise on töö lõpetanud, klõpsake nuppu valmis. 

 
Pange tähele, et olulisi punkte. 

- Kui kuvatakse tõrketeade: "**see tellimus pole registreeritud kasutada nimeruum Microsoft.DataFactory**", tehke ühte järgmistest ja proovige uuesti avaldada: 

    - Azure'i PowerShelli, käivitage järgmine käsk andmete Factory pakkuja registreerimiseks. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Käivitada järgmine käsk, et kinnitada, et andmete Factory, mis on registreeritud pakkuja. 
    
            Get-AzureRmResourceProvider
    - Logige Azure tellimuse [Azure portaali](https://portal.azure.com) sisse ja liikuge andmete Factory blade (või) andmete factory Azure portaali loomine. See toiming automaatselt registreerib pakkuja teie eest.
-   Andmete factory nimi võib registreerida DNS-i nimi tulevikus ja seega muutuvad avalikult nähtav.
-   Andmete Factory eksemplari loomiseks peate olema administraator või co-admin Azure tellimuse

 
## <a name="monitor-pipeline"></a>Müügivõimaluste jälgimine

### <a name="monitor-pipeline-using-diagram-view"></a>Kuvari müügivõimaluste diagrammivaate abil
6. [Azure'i portaali](https://portal.azure.com/)sisse logida, tehke järgmist:
    1. **Rohkem teenuseid** ja seejärel klõpsake **andmete tehased**.
        ![Andmete tehased sirvimine](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Valige oma andmete factory nimi (nt: **FirstDataFactoryUsingVS09152016**) andmete tehased loendist. 
        ![Valige oma andmete factory](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Oma andmete factory avalehel nuppu **Diagramm**.
  
    ![Diagrammi paan](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. Diagrammivaates, näete torujuhtmed ja andmekomplektide kasutatud selles õpetuses ülevaade.
    
    ![Diagrammivaate](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Tulemas vaadata kõiki tegevusi, paremklõpsake müügivõimaluste skemaatilise diagrammi ja klõpsake käsku Ava kohaletoimetamisel. 

    ![Avatud müügivõimaluste menüü](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Veenduge, et näete HDInsightHive tegevuse teel. 
  
    ![Avatud müügivõimaluste kuvamine](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Liikuda eelmisele vaatesse naasmiseks klõpsake **andmete factory** lingirea menüü ülaosas. 
10. Topeltklõpsake **Diagrammivaate**andmekomplekti **AzureBlobInput**. Veenduge, et sektorit on **valmis** olekus. Võib kuluda paar minutit sektorit kuvamiseks valmis olekus. Kui see ei juhtu, pärast mida oodata millalgi, näha, kas sisend faili (input.log) õige container (adfgetstarted) ja kausta (inputdata).

    ![Kas olete valmis olekus Sisestuskeel sektorit](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Klõpsake nuppu **X** **AzureBlobInput** blade sulgemiseks. 
12. Topeltklõpsake **Diagrammivaate**andmekomplekti **AzureBlobOutput**. Te näete, et sektorit, mida praegu töödeldakse.

    ![Andmekomplekti](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Kui töötlemine on lõpule jõudnud, näete sektorit **valmis** olekus.

    > [AZURE.IMPORTANT] Kuvatakse nõudmisel Hdinsightiga kobar loomine kulub tavaliselt millalgi (umbes 20 minutit). Seetõttu oodata müügivõimaluste teha töötlemiseks sektorit **30 minutit** .  

    ![Andmekomplekti](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Kui **olete valmis** olekus on sektorit, vaadake **partitioneddata** kausta **adfgetstarted** ümbrises bloobimälus väljundi andmete jaoks.  
 
    ![väljundi andmed](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klõpsake üksikasjade seda **andmete sektorit** blade kuvamiseks sektorit.

    ![Andmete sektorit üksikasjad](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Toimingu käivitada **tegevuse jookseb loendi** toimingu käivitamine (taru tegevus meie stsenaariumi) **tegevuste käivitamine üksikasjad** aknas üksikasjade kuvamiseks klõpsake nuppu.   
    ![Tegevuste käivitamine üksikasjad](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Logifailide, saate vaadata taru päring, mis on täidetud ja olekuteavet. Need logid on seotud probleemide tõrkeotsingu jaoks.  
 

Leiate juhised, kuidas kasutada Azure portaali jälgida müügivõimaluste ja andmekomplektide selles õpetuses olete loonud [kuvari andmekomplektide ja kohaletoimetamisel](data-factory-monitor-manage-pipelines.md) .

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Müügivõimaluste jälgimine ja haldamine rakenduse abil jälgimine
Saate kasutada kuvari & rakenduse jälgida oma torustike haldamine. Täpsemat teavet selle rakenduse kasutamise kohta leiate teemast [jälgimine ja haldamine Azure'i andmed Factory torujuhtmete jälgimine ja haldamine rakenduse abil](data-factory-monitor-manage-app.md).

1. Klõpsake nuppu jälgimine ja haldamine paani.

    ![Jälgimine ja haldamine paan](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. Peaks näha jälgimine ja haldamine rakenduses. Muuta **alguskellaaeg** ja **lõppkellaaeg** vastavaks start (04-01-2016 12:00 AM) ja lõpuaegade (04-02-2016 12:00 AM) müügivõimaluste ja klõpsake nuppu **Rakenda**.

    ![Jälgimine ja haldamine rakendus](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Valige soovitud tegevus akna tegevuse Windows loendist seda üksikasjade kuvamiseks. 
    ![Tegevuste aknas üksikasjad](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] Sisestuskeel faili kustutamine kui sektorit on töötlemine õnnestus. Seetõttu sektorit uuesti või õpetuse uuesti teha soovite, laadige inputdata kausta adfgetstarted-ümbrisest Sisestuskeel faili (input.log).
 

## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer abil saate vaadata andmete tehased

1. **Visual Studio**, klõpsake menüü **Vaade** ja käsku **Server Explorer**.
2. Serveri Exploreri aknas, laiendage **Azure** ja **Andmete Factory**laiendamine. Kui kuvatakse **Visual Studio sisse logida**, sisestage **konto** Azure tellimusega seostatud ja klõpsake nuppu **Jätka**. Sisestage **parool**ja klõpsake nuppu **Logi sisse**. Visual Studio proovib kõik Azure'i andmed tehased tellimuse kohta teabe saamiseks. Selle toimingu aknas **Andmete Factory ülesandeloendi** oleku kuvamine

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Paremklõpsake andmete factory ja valige **Eksportida andmed Factory uue projekti** loomiseks Visual Studio projekti mõne olemasoleva factory andmete põhjal.

    ![Eksportida andmed factory](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Värskenda andmed Factory tööriistad Visual Studio

Azure'i andmed Factory tööriistad Visual Studio värskendamiseks tehke järgmist.

1. Klõpsake menüü **Tööriistad** ja valige **laiendid ja värskendused**.
2. Valige vasakul paanil **värskendused** ja seejärel valige **Visual Studio galerii**.
3. **Visual Studio Azure'i andmed Factory tööriistad** ja klõpsake nuppu **Värskenda**. Kui te ei näe seda kirjet, on teil juba tööriistade uusima versiooni. 

## <a name="use-configuration-files"></a>Konfiguratsiooni failide kasutamine
Failid Visual Studio abil saate lingitud teenuste/tabelid/torustikele teisiti iga keskkonna atribuutide konfigureerimine. 

Võtke arvesse järgmist JSON mõistet teenuse Azure Storage lingitud. Määramiseks **connectionString** account_name ja accountkey põhjal keskkonnas (arendaja/tootmiskatsete), millele juurutate andmete Factory üksuste erinevaid väärtusi. Võite saavutada, kasutades eraldi konfiguratsioonifail iga keskkond seda käitumist. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Otsingukonfiguratsiooni faili lisamine
Iga keskkonna konfiguratsioon faili lisamiseks toimige järgmiselt.   

1. Paremklõpsake oma Visual Studio lahendus andmete Factory projekti, osutage käsule **Lisa**ja klõpsake nuppu **Uus üksus**.
2. Loendist **Config** installitud mallide vasakul, valige **Konfiguratsioonifail**, sisestage **nimi** konfiguratsioonile ja klõpsake nuppu **Lisa**.

    ![Otsingukonfiguratsiooni faili lisamine](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Lisage konfiguratsiooni parameetrid ja nende väärtuste järgmises vormingus.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    Selles näites konfigureerib atribuut connectionString lingitud Azure SQL-i teenuse ja teenuse Azure Storage lingitud. Pange tähele, et süntaks määramiseks nimi on [JsonPath](http://goessner.net/articles/JsonPath/).   

    Kui JSON on atribuut, millel on massiivi väärtustest, nagu on näidatud järgmine kood:  

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
    
    Konfigureerige atribuudid, nagu on näidatud järgmises konfiguratsiooni failis (Kasuta 0-põhine indekseerimine). 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Atribuutide nimesid tühikutega
Kui atribuudi nimi sisaldab tühikuid, kasutage nurksulgudega, nagu on näidatud järgmises näites (andmebaasiserveri nimi). 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Kasutades konfiguratsiooni lahenduse juurutamine
Kui avaldate vs Azure'i andmed Factory üksused, saate määrata konfiguratsiooni, mida soovite selle avaldamise toimingu jaoks kasutada. 

Avaldada üksuste Azure'i andmed Factory projekti konfiguratsiooni faili abil:   

1. Paremklõpsake andmete Factory projekti ja klõpsake nuppu **Avalda** dialoogiboks **Avaldamine üksuste** kuvamiseks. 
2. Valige soovitud olemasolevad andmed factory või väärtuste loomine andmete factory **konfigureerimine andmete factory** lehe määramine ja klõpsake nuppu **edasi**.   
3. Lehe **Avaldamine üksuste** : kuvatakse ripploendis koos saadaval konfiguratsioone **Valige juurutamise Config** välja.

    ![Valige config fail](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Valige **konfigureerimise faili** , mida soovite kasutada, ja klõpsake nuppu **edasi**. 
5. Veenduge, et näete **Kokkuvõte** lehel JSON-faili nimi ja klõpsake nuppu **edasi**. 
6. Kui juurutamine toiming on lõpule jõudnud, klõpsake nuppu **valmis** . 

Juurutamisel, kasutatakse seadmine atribuutide väärtused JSON failid andmete Factory üksuste enne üksuste on juurutatud Azure'i andmed Factory teenuse konfiguratsiooni faili olevad väärtused.   

## <a name="summary"></a>Kokkuvõte 
Selles õpetuses loodud on Azure andmete factory andmete töötlemise taru skripti käivitades Hdinsightiga Hadoopi klaster. Saate kasutada andmete Factory redaktori Azure portaali teha järgmist:  

1.  Loodud on Azure **andmete factory**.
2.  Loodud kaks **lingitud teenused**.
    1.  **Azure Storage** lingitud teenus hoiab andmeid factory sisend faile oma Azure'i bloobimälu link.
    2.  **Windows Azure Hdinsightiga** nõudmisel lingitud teenuse andmete factory kuvatakse nõudmisel Hdinsightiga Hadoopi kobar linkida. Azure'i andmed loob mõne Hdinsightiga Hadoopi kobar lihtsalt õigel ajal töödelda sisendandmete ja väljundi andmete. 
3.  Loodud kaks **andmekomplektide**, mis kirjeldavad sisend- ja andmete Hdinsightiga taru tegevuste teel. 
4.  **Müügivõimaluste** loodud **Hdinsightiga taru** tegevuse.  


## <a name="next-steps"></a>Järgmised sammud
Selles artiklis loomist müügivõimaluste teisendus tegevus (Hdinsightiga tegevuste), mis töötab taru script on nõudmisel Hdinsightiga kobar. Kopeeri tegevuse mõne Azure'i bloobimälu andmete kopeerimine Azure SQL-i kasutamise kohta leiate teemast [Õppeteema: andmete kopeerimine, on Azure SQL Azure'i Bloobivahemälu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
  
## <a name="see-also"></a>Vt ka
| Teema: | Kirjeldus |
| :---- | :---- |
| [Tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) | Selles artiklis on toodud andmete teisendus tegevused (nt Hdinsightiga taru teisendus kasutasite selles õpetuses) loendi Azure'i andmed Factory ei toeta. | 
| [Plaanimis- ja täitmise](data-factory-scheduling-and-execution.md) | Selles artiklis selgitatakse plaanimis- ja täitmise aspektide Azure'i andmed Factory rakenduse mudel. |
| [Torujuhtmete](data-factory-create-pipelines.md) | See artikkel aitab teil mõista torujuhtmed ja Azure'i andmed Factory ja kuidas neid kasutada ehitada lõpuni Andmepõhiste töövoogude oma stsenaarium või ettevõtte tegevusi. |
| [Andmekomplektide](data-factory-create-datasets.md) | See artikkel aitab teil mõista andmekogumite Azure'i andmed Factory.
| [Jälgimine ja haldamine torustike jälgimise App abil](data-factory-monitor-manage-app.md) | Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja silumine torujuhtmete jälgimine ja haldamine rakenduse abil. 
