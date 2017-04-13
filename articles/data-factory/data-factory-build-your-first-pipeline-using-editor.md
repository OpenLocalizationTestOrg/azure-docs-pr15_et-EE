<properties
    pageTitle="Koostada esimese andmete factory (Azure'i portaal) | Microsoft Azure'i"
    description="Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste Azure portaali andmete Factory redaktori kasutamine."
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
    ms.date="09/14/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Õpetus: Koostada oma esimese Azure'i andmed factory Azure'i portaalis
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-build-your-first-pipeline.md)
- [Azure'i portaal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShelli](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressursihaldur Mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-GA](data-factory-build-your-first-pipeline-using-rest-api.md)

Selles artiklis saate teada, kuidas luua oma esimese Azure'i andmed factory [Azure portaali](https://portal.azure.com/) abil. 

## <a name="prerequisites"></a>Eeltingimused        
1. Lugege läbi [Õpetuse ülevaate](data-factory-build-your-first-pipeline.md) artikli ja täitke juhised **nõutav** .
2. See artikkel ei paku kontseptuaalne ülevaade Azure'i andmed Factory teenuse. Soovitame läbida [Sissejuhatus Azure'i andmed Factory](data-factory-introduction.md) artiklis üksikasjaliku ülevaate saamiseks teenuse.  

## <a name="create-data-factory"></a>Andmete factory loomine
Andmete factory võib olla üks või mitu torujuhtmete. Müügivõimaluste võib olla üks või mitu tegevusi. Näiteks Kopeeri tegevuse andmete kopeerimiseks mõnest muust allikast sihtkoha andmed salvestada ja Hdinsightiga taru tegevuste käivitamiseks taru skripti muuta sisendandmete toote väljund andmed. Alustame andmete factory loomise selles etapis tuleb. 

1.  [Azure'i portaali](https://portal.azure.com/)sisse logida.
2.  Klõpsake vasakul menüüs **Uus** , klõpsake **andmete + Analytics**ja klõpsake **Andmete Factory**.
        
    ![Blade loomine](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  **Uute andmete factory** tera, sisestage **GetStartedDF** nimi.

    ![Uute andmete factory blade](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] 
    > Azure'i andmed factory nimi peab olema **globaalselt kordumatu**. Kui kuvatakse tõrketeade: **andmete factory nimi "GetStartedDF" on pole saadaval**. Muutke nime andmete factory (nt yournameGetStartedDF) ja proovige uuesti luua. Nimede reeglid andmete Factory artefakte [Andmete Factory - nime andmise reeglid](data-factory-naming-rules.md) teemat.
    > 
    > Andmete factory nimi võib registreerida **DNS-i** nimi tulevikus ja seega muutuvad avalikult nähtav.

3.  Valige **Azure'i tellimus** , kuhu soovite andmed factory luua. 
4.  Valige olemasolev **ressursirühm** või looge ressursirühma. Juhend on luua nimega ressursirühma: **ADFGetStartedRG**. 
5.  Klõpsake nuppu **Loo** **Uus andmete factory** enne.

    > [AZURE.IMPORTANT] Andmete Factory eksemplari loomiseks peate olema [Andmete Factory kaasautori](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) roll tasemel tellimuse/ressursi rühma liige. 
6.  Näete andmete factory, luuakse **Startboard** Azure portaali järgmiselt:   

    ![Andmete factory olek loomine](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Palju õnne! Olete loonud oma esimese andmete factory. Pärast andmete factory on loodud, kuvatakse andmete factory leht, mis kuvatakse andmete factory sisu.   

    ![Andmete Factory blade](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Müügivõimaluste loomine andmete factory, peate esmalt looma mõned andmed Factory üksused. Esmalt looge lingitud teenused link andmete poed/avaldis arvutab teie andmesalve, määratleda sisendi ja väljundi andmekomplektide sisend andmete lingitud andmete esitamiseks ja seejärel looge tulemas tegevust, mis kasutab neid andmekomplektide. 

## <a name="create-linked-services"></a>Looge lingitud teenused
Selles etapis tuleb teil link Azure Storage konto ja kuvatakse nõudmisel Windows Azure Hdinsightiga kobar oma andmete factory. Azure Storage konto hoiab selle valimi tulemas sisend- ja andmeid. Lingitud Hdinsightiga teenuse saab käivitada taru müügivõimaluste selles näites on märgitud. Määratlege, millist [andmete talletamiseks](data-factory-data-movement-activities.md)/[arvutada teenuste](data-factory-compute-linked-services.md) kasutatakse teie stsenaariumi ja nende teenuste lingi andmed factory lingitud teenuste loomisega.  

### <a name="create-azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus loomine
Selles etapis tuleb teil link Azure Storage konto oma andmete factory. Selles õpetuses saate talletada sisend andmed ja HQL skriptifail sama Azure Storage konto. 

1.  Klõpsake **autor ja juurutamine** **Andmete FACTORY** enne **GetStartedDF**jaoks. Peaksite nägema andmete Factory redaktor. 
     
    ![Koostamine ja juurutamine paan](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  **Uute andmete talletamiseks** ja valige **Azure salvestusruumi**.

    ![Uute andmete talletamiseks - Azure Storage - menüü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.  Peaksite nägema JSON script Editor Azure Storage lingitud teenuse loomiseks. 
    
    ![Azure'i lingitud salvestusteenus](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
     
4. Asendage Azure storage konto ja **konto võti** Accessi võti Azure storage konto nimi **konto nimi** . Salvestusruumi Accessi tootenumbri hankimine leiate teemast [vaate, kopeerimine ja uuesti luua salvestusruumi kiirklahvide](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Klõpsake **Deploy** käsuriba lingitud teenuse kasutuselevõtuks.

    ![Nupp juurutamine](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Pärast lingitud teenus on edukalt juurutatud, **mustand-1** aken kaob ja puuvaates vasakul **AzureStorageLinkedService** kuvatakse. 
    ![Lingitud salvestusteenus menüü](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### <a name="create-azure-hdinsight-linked-service"></a>Windows Azure Hdinsightiga lingitud teenuse loomine
Selles etapis tuleb link kuvatakse nõudmisel Hdinsightiga kobar oma andmete factory. Hdinsightiga kobar käitusajal luuakse automaatselt ja kustutatud pärast seda teha töötlemiseks ja jõude jaoks määratud aja jooksul. 

1. Klõpsake **Andmete Factory redaktori** **... Lisateavet**, klõpsake nuppu **Uus arvutada**ja valige **nõudmisel Hdinsightiga kobar**.

    ![Uue Arvuta](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopeerige ja kleepige järgmine koodilõigu **mustand-1** aknasse. JSON koodilõigu kirjeldatakse selle Hdinsightiga kobar nõudmisel loomiseks kasutatud atribuute. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    Järgmises tabelis on ära toodud väljavõte kasutatakse atribuutide JSON kirjeldused:
    
  	| Atribuut | Kirjeldus |
  	| :------- | :---------- |
  	| Versioon | Määrab, et versioon on Hdinsightiga loodud olema 3,2. | 
  	| ClusterSize | Saate määrata Hdinsightiga kobar suurust. | 
  	| TimeToLive | Mis määrab jõudeaja jaoks Hdinsightiga kobar, see enne kustutamist. |
  	| linkedServiceName | Saate määrata salvestusruumi konto, mida kasutatakse Hdinsightiga loodud logide salvestamiseks. |

    Võtke arvesse järgmist. 
    
    - Andmete Factory loob **Windowsi-põhiste** Hdinsightiga kobar teile selle JSON. Te oleksite selle **Linuxi-põhiste** Hdinsightiga kobar loomine. Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - Võite kasutada **oma Hdinsightiga kobar** asemel kuvatakse nõudmisel Hdinsightiga kobar. Üksikasjad leiate [Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - Hdinsightiga kobar loob **vaikimisi container** bloobimälu, määratud JSON (**linkedServiceName**). Klaster kustutamisel ei kustutata Hdinsightiga selles ümbrises. Selline käitumine on kujundus. Nõudmisel Hdinsightiga lingitud teenusega Hdinsightiga kobar on loodud iga kord, kui mõnda sektorit töötlemise, kui ei ole olemasoleva reaalajas kobar (**timeToLive**). Klaster kustutatakse automaatselt, kui töötlemine on lõpule jõudnud.
    
        Nagu töödeldakse veel sektoritele, näete oma Azure'i bloobimälu palju ümbriste. Kui te ei pea neid tõrkeotsingu tööd, võite kustutage need salvestusruumi kuidagi vähendada. Järgige neid ümbriste nimed mustri: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Näiteks [Microsoft salvestusruumi Exploreri](http://storageexplorer.com/) abil saate kustutada ümbriste Azure'i bloobimälus.

    Üksikasjad leiate [Nõudmisel Hdinsightiga lingitud teenus](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .
3. Klõpsake **Deploy** käsuriba lingitud teenuse kasutuselevõtuks. 

    ![Nõudmisel Hdinsightiga lingitud teenuse juurutamine](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Veenduge, et näete nii **AzureStorageLinkedService** ja **HDInsightOnDemandLinkedService** puu kuvamiseks klõpsake vasakus servas.

    ![Puuvaade lingitud teenustega](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Andmekomplektide loomine
Selles etapis loote andmekomplektide sisend ja väljund andmete töötlemiseks taru. Vaadake neid andmekomplektide **AzureStorageLinkedService** selles õpetuses olete loonud. Lingitud teenuse osutab Azure Storage konto ja andmekomplektide määrata talletamist, mis hoiab sisendit container, kausta, faili nimi ja andmete väljund.   

### <a name="create-input-dataset"></a>Sisestuskeel andmekomplekti loomine

1. Klõpsake **Andmete Factory redaktori** **... Lisateavet** käsuriba, klõpsake nuppu **Uus andmekomplekti**ja valige **Azure'i bloobimälu**.

    ![Uue andmekomplekti](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopeerige ja kleepige järgmine koodilõigu mustand-1 aknasse. JSON koodilõigu, loote nimega **AzureBlobInput** , mis tähistab sisendandmete tegevuse tulemas Andmekomplekt. Lisaks saate määrata, et sisendandmete asub bloobimälu ümbris nimega **adfgetstarted** ja kaust nimega **inputdata**.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
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
  	| linkedServiceName | viitab AzureStorageLinkedService, mis on varem loodud. |
  	| faili nimi | Atribuut pole kohustuslik. Kui argument seda atribuuti, kuvatakse folderPath kõik failid on peale. Sel juhul ainult input.log töödeldakse. |
  	| tüüp | Me kasutame tekstivorming on logifailide vormingus tekst. | 
  	| columnDelimiter | logifailide veerud on piiratud koma () |
  	| sagedus/intervall | määratud kuu ja intervalli sagedus on 1, mis tähendab, et Sisestuskeel sektorid on saadaval iga kuu. | 
  	| väliste | selle atribuudi väärtuseks on seatud väärtus true, kui sisendandmete on loodud andmete Factory teenus. | 
        
3. Äsja loodud andmekomplekti juurutamiseks käsuribal nuppu **Deploy** . Peaksite nägema andmekomplekti puuvaates vasakul. 


### <a name="create-output-dataset"></a>Väljundi andmekomplekti loomine
Nüüd loote väljundi andmekomplekti tähistada väljundi Azure'i bloobimälu talletatud andmed. 

1. Klõpsake **Andmete Factory redaktori** **... Lisateavet** käsuriba, klõpsake nuppu **Uus andmekomplekti**ja valige **Azure'i bloobimälu**.  
2. Kopeerige ja kleepige järgmine koodilõigu mustand-1 aknasse. JSON koodilõigu, loote nimega **AzureBlobOutput**ja täpsustades andmeid, mis toodab taru skripti struktuuri Andmekomplekt. Lisaks saate määrata tulemused talletatavad bloobimälu ümbris nimega **adfgetstarted** ja kaust nimega **partitioneddata**. Jaotises **saadavuse** määrab, et väljundi andmekomplekti valmistatakse kuude kaupa.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
3. Äsja loodud andmekomplekti juurutamiseks käsuribal nuppu **Deploy** .
4. Veenduge, et andmekomplekti on loodud.

    ![Puuvaade lingitud teenustega](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Müügivõimaluste loomine
Selles juhises saate luua oma esimese müügivõimaluste **HDInsightHive** tegevuse. Sisestuskeel sektorit on saadaval kuus (sagedus: kuu, intervall: 1), väljundi sektorit toodetud kuus ja atribuudi ajasti tegevuse jaoks on ka seatud kuus. Sätete väljundi andmekomplekti ja tegevuste ajasti peab vastama. Väljundi andmekomplekti on praegu, mis juhib ajakava, seega peate looma ka väljundi andmekomplekti isegi juhul, kui tegevuse pole mis tahes väljundi loomiseks. Kui tegevuse ei võta mis tahes sisendit, võite jätkata Sisestuskeel andmekomplekti loomine. Järgmised JSON kasutatakse atribuutide selgitatakse selle jaotise lõpus. 

1. Klõpsake **Andmete Factory redaktori** **kolmikpunkti (…) Veel käske** ja seejärel klõpsake käsku **Uus kohaletoimetamisel**.
    
    ![Müügivõimaluste nupp uus](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopeerige ja kleepige järgmine koodilõigu mustand-1 aknasse.

    > [AZURE.IMPORTANT] Asendage **storageaccountname** soovitud JSON salvestusruumi konto nimi.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService",
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
    
    Taru skriptifail **partitionweblogs.hql**, talletatakse Azure storage konto (määratud scriptLinkedService, mida nimetatakse **AzureStorageLinkedService**) ja container **adfgetstarted** **skripti** kausta.

    **Määratleb** jaotise abil saate määrata edastatakse taru skripti taru konfiguratsiooni väärtustena käitusaja sätted (nt ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    Tulemas **algus** ja **lõpp** atribuutide määrab tulemas aktiivse perioodi.

    Saate määrata tegevuse JSON taru skript käivitatakse Arvuta, on **linkedServiceName** – **HDInsightOnDemandLinkedService**määratud.

    > [AZURE.NOTE] [Müügivõimaluste anatoomia](data-factory-create-pipelines.md#anatomy-of-a-pipeline) kohta vaadake teavet näites kasutatakse JSON atribuudid. 

3. Kontrollige järgmist. 
    1. **Input.log** fail on olemas **inputdata** kausta **adfgetstarted** ümbris paanide Azure'i bloobimälu
    2. **partitionweblogs.HQL** fail on olemas **adfgetstarted** ümbris paanide Azure'i bloobimälu **skript** kausta. Täieliku nõutav etappide [Õpetuse ülevaade](data-factory-build-your-first-pipeline.md) kui te ei näe neid faile. 
    3. Veenduge, et **storageaccountname** asendada tulemas JSON salvestusruumi konto nimi. 
2. Klõpsake käsku **Deploy** käsuribal juurutamiseks tulemas. Kuna varem on seadistatud **algus** ja **lõpp** aeg ja **isPaused** väärtuseks false, müügivõimaluste (tegevus teel) käivitatakse kohe pärast juurutamist. 
4. Veenduge, et näete müügivõimaluste puuvaates.

    ![Müügivõimaluste ja puuvaates](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Palju õnne, olete loonud oma esimese müügivõimaluste!

## <a name="monitor-pipeline"></a>Müügivõimaluste jälgimine

### <a name="monitor-pipeline-using-diagram-view"></a>Kuvari müügivõimaluste diagrammivaate abil

6. Klõpsake nuppu **X** andmete Factory Editor labad sulgemiseks ja andmete Factory tera tagasiliikumiseks ja nuppu **skeem**.
  
    ![Diagrammi paan](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. Diagrammivaates, näete torujuhtmed ja andmekomplektide kasutatud selles õpetuses ülevaade.
    
    ![Diagrammivaate](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. Tulemas vaadata kõiki tegevusi, paremklõpsake müügivõimaluste skemaatilise diagrammi ja klõpsake käsku Ava kohaletoimetamisel. 

    ![Avatud müügivõimaluste menüü](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Veenduge, et näete HDInsightHive tegevuse teel. 
  
    ![Avatud müügivõimaluste kuvamine](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Liikuda eelmisele vaatesse naasmiseks klõpsake **andmete factory** lingirea menüü ülaosas. 
10. Topeltklõpsake **Diagrammivaate**andmekomplekti **AzureBlobInput**. Veenduge, et sektorit on **valmis** olekus. Võib kuluda paar minutit sektorit kuvamiseks valmis olekus. Kui see ei juhtu, pärast mida oodata millalgi, näha, kas sisend faili (input.log) õige container (adfgetstarted) ja kausta (inputdata).

    ![Kas olete valmis olekus Sisestuskeel sektorit](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Klõpsake nuppu **X** **AzureBlobInput** blade sulgemiseks. 
12. Topeltklõpsake **Diagrammivaate**andmekomplekti **AzureBlobOutput**. Te näete, et sektorit, mida praegu töödeldakse.

    ![Andmekomplekti](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Kui töötlemine on lõpule jõudnud, näete sektorit **valmis** olekus.
    
>[AZURE.IMPORTANT] Kuvatakse nõudmisel Hdinsightiga kobar loomine kulub tavaliselt millalgi (umbes 20 minutit). Seetõttu oodata müügivõimaluste teha töötlemiseks sektorit **30 minutit** .    

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Kui **olete valmis** olekus on sektorit, vaadake **partitioneddata** kausta **adfgetstarted** ümbrises bloobimälus väljundi andmete jaoks.  
 
    ![väljundi andmed](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Klõpsake üksikasjade seda **andmete sektorit** blade kuvamiseks sektorit.

    ![Andmete sektorit üksikasjad](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
12. Toimingu käivitada **tegevuse käivitatakse loendi** toimingu käivitamine (taru tegevus meie stsenaariumi) **tegevuste käivitamine üksikasjad** aknas üksikasjade kuvamiseks klõpsake nuppu.   

    ![Tegevuste käivitamine üksikasjad](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)  
    
    Logifailide, saate vaadata taru päring, mis on täidetud ja olekuteavet. Need logid on seotud probleemide tõrkeotsingu jaoks.
Vt [jälgimine ja haldamine torujuhtmete abil Azure portaali labad](data-factory-monitor-manage-pipelines.md) üksikasjalikumat teavet artiklist. 

> [AZURE.IMPORTANT] Sisestuskeel faili kustutamine kui sektorit on töötlemine õnnestus. Seetõttu sektorit uuesti või õpetuse uuesti teha soovite, laadige inputdata kausta adfgetstarted-ümbrisest Sisestuskeel faili (input.log).

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Müügivõimaluste jälgimine ja haldamine rakenduse abil jälgimine
Saate kasutada kuvari & rakenduse jälgida oma torustike haldamine. Täpsemat teavet selle rakenduse kasutamise kohta leiate teemast [jälgimine ja haldamine Azure'i andmed Factory torujuhtmete jälgimine ja haldamine rakenduse abil](data-factory-monitor-manage-app.md).

1. Klõpsake avalehel oma andmete Factory **jälgimine ja haldamine** paani.

    ![Jälgimine ja haldamine paan](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png) 
2. Peaksite nägema **rakenduste jälgimine ja haldamine**. Muuta **alguskellaaeg** ja **lõppkellaaeg** vastavaks start (04-01-2016 12:00 AM) ja lõpuaegade (04-02-2016 12:00 AM) müügivõimaluste ja klõpsake nuppu **Rakenda**.

    ![Jälgimine ja haldamine rakendus](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png) 
3. Valige soovitud tegevus akna **Tegevuse Windows** loendist seda üksikasjade kuvamiseks. 
    ![Tegevuste aknas üksikasjad](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


## <a name="summary"></a>Kokkuvõte 
Selles õpetuses loodud on Azure andmete factory andmete töötlemise taru skripti käivitades Hdinsightiga Hadoopi klaster. Saate kasutada andmete Factory redaktori Azure portaali teha järgmist:  

1.  Loodud on Azure **andmete factory**.
2.  Loodud kaks **lingitud teenused**.
    1.  **Azure Storage** lingitud teenus hoiab andmeid factory sisend faile oma Azure'i bloobimälu link.
    2.  **Windows Azure Hdinsightiga** nõudmisel lingitud teenus on nõudmisel Hdinsightiga Hadoopi kobar linkida andmeid factory. Azure'i andmed loob mõne Hdinsightiga Hadoopi kobar lihtsalt õigel ajal töödelda sisendandmete ja väljundi andmete. 
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

  

