<properties     
    pageTitle="Azure'i andmed Factory - näidised" 
    description="Annab üksikasjade näidised, mis on Azure andmete Factory teenus." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure'i andmed Factory - näidised

## <a name="samples-on-github"></a>Näidised github
[GitHub Azure-DataFactory hoidla](https://github.com/azure/azure-datafactory) sisaldab mitu proovi, mille abil saate kiiresti ramp üles Azure'i andmed Factory teenusega (või) muutmine skripte ja kasutada seda oma rakenduses. Samples\JSON kaust sisaldab JSON Koodilõigud levinud stsenaariumi järgi.

| Näidis | Kirjeldus |
| :----- | :---------- | 
| [ADF kiirtutvustus](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | Selle valimi pakub on-lõpuni kiirtutvustus Azure'i andmed Factory abil andmete logifailid pöörduda ülevaateid logifailid töötlemiseks. <br/><br/>Ülevaate, andmete Factory müügivõimaluste kogub valimi logid protsesside rikastab logide andmed viide andmetega ja muudab andmed tulemuslikkuse turunduskampaania, mis võeti hiljuti kasutusele kohta. |
| [JSON näidised](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | See näide annab JSON näited levinud stsenaariumi. | 
| [Http andmete Downloader näidis](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | Selle valimi vitriine andmete allalaadimist lõpp HTTP kaudu Azure'i bloobimälu kohandatud .NET tegevuse abil. |
| [Rist atribuudile Dot Net tegevuse näidis](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | See näide võimaldab Autor kohandatud .NET tegevuse, mis on piiratud vahemikuga komplekti versioonid kasutavad ADF käivitit (nt WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x jne). |
| [Käivitage R skript](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  See näide sisaldab autonoomsest RScript.exe saab kasutada andmete Factory kohandatud tegevuse. See näide töötab ainult oma (mitte tellitav) Hdinsightiga kobar, mis on juba installitud R seda. |
| [Autonoomsest säde tööd Hdinsightiga Hadoopi kobar](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | See näide näitab, kuidas autonoomsest säde programmi MapReduce tegevuse abil. Säde programm kopeerib ühe Azure'i bloobimälu container andmed lihtsalt teise. |
| [Twitteri analüüsi Azure seadme õppimine paketi hinded tegevuse abil](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | See näide näitab, kuidas kasutada AzureMLBatchScoringActivity autonoomsest Azure seadme õ mudelit, mis teeb Twitteri meeleolu analüüsi, hinded, ennustamine jne. |
| [Kohandatud toimingute abil Twitteri analüüs](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  See näide näitab, kuidas kasutada kohandatud .NET tegevuse autonoomsest Azure seadme õ mudelit, mis teeb Twitteri meeleolu analüüsi, hinded, ennustamine jne. |
| [Parameetritega torustikele Azure seadme õpetused](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Valimi pakub lõpuni C# koodi N torustikele hinded ja ümberõpe iga teises regioonis parameetriga, kus piirkondade loendiga on pärit parameters.txt faili, mis on kaasas selle valimi juurutamiseks. | 
| [Viide andmete värskendamine Azure'i voo Analytics projektide jaoks](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  See näide näitab, kuidas kasutada Azure'i andmed Factory ja Azure voo Analytics koos viide andmetega päringute käivitamine ja setup viide ajakava andmete värskendamine. |
| [Hübriidjuurutuse müügivõimaluste kohapealse Hortonworks Hadoopi abil](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | Valimi kasutab mõnda kohapealse Hadoopi kobar Arvuta target andmete Factory töö töötab täpselt samamoodi nagu muude Arvuta sihtkohtade tuleks lisada, nagu on Hdinsightiga vastavalt Hadoopi kobar pilveteenuses. |
| [JSON tulemus tööriist](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | See tööriist võimaldab teil teisendada JSONs versiooni enne 2015-07-01-eelvaade, et uusim või 2015 07-01-eelvaates (vaikesäte). |  
| [U-SQL näidisfaili Sisestuskeel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  See on valimi fail, mis kasutavad U-SQL-i tegevus. | 

## <a name="azure-resource-manager-templates"></a>Azure'i ressursihaldur Mallid
Andmete Factory github leiate Azure'i ressursihaldur järgmised mallid. 

| Mall | Kirjeldus |
| -------- | ----------- | 
| [Kopeerige Azure'i bloobimälu Azure SQL-andmebaasiga](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Selle malli juurutamine loob mõne Azure'i andmed factory kohaletoimetamisel, mis kopeerib määratud Azure'i bloobimälu andmete Azure SQL-andmebaasiga |    
| [Kopeerige Salesforce'i Azure'i bloobimälu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Selle malli juurutamine loob mõne Azure'i andmed factory kohaletoimetamisel, mis kopeerib määratud Salesforce'i konto andmete Azure'i bloobimälu. |    
| [Muuta andmete taru skripti käivitades mõne Windows Azure Hdinsightiga kobar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Selle malli juurutamine loob mõne Azure'i andmed factory kohaletoimetamisel, mis muudab andmete valimi taru skripti käivitades mõne Azure Hdinsightiga Hadoopi kobar. | 


## <a name="samples-in-azure-portal"></a>Näidised Azure'i portaalis
Saate paani **valimi torujuhtmete** oma andmete factory avalehel juurutada oma andmete factory valimi torujuhtmed ja nende seotud üksuste (andmekomplektide ja lingitud teenused). 

1. Andmete factory luua või avada mõne olemasoleva andmete factory. Teemast [Azure andmete Factory alustamine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) juhised andmete factory loomiseks.
2. Klõpsake andmete factory **Andmete FACTORY** labale **valimi torujuhtmete** paani.

    ![Valimi torujuhtmete paan](./media/data-factory-samples/SamplePipelinesTile.png)

2. Klõpsake **valimi torujuhtmete** labale **valimi** soovite juurutada. 
    
    ![Proovi torujuhtmete blade](./media/data-factory-samples/SampleTile.png)

3. Määrake valimi sätete konfigureerimine. Näiteks oma Azure storage konto nimi ja konto võti Azure SQL serveri nimi, andmebaasi, kasutaja-ID ja parooliga jne. 

    ![Valimi blade](./media/data-factory-samples/SampleBlade.png)

4. Pärast seda, kui olete teinud täpsustades konfiguratsioonisätted, klõpsake nuppu **Loo** luua ja juurutada valimi torujuhtmed ja lingitud teenuste/tabelite torujuhtmete kasutavad.
5. Näete juurutamise oleku klõpsasite varem **valimi torujuhtmete** enne valimi paani.

    ![Juurutamise olek](./media/data-factory-samples/DeploymentStatus.png)

6. Kui paani valimi **juurutamise õnnestus** teade, sulgege **valimi torujuhtmete** tera.  
5. **Andmete FACTORY** tera, näete lingitud teenused, andmekogumi ja torujuhtmete lisatakse teie andmete factory.  

    ![Andmete Factory blade](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Visual Studio näidised

### <a name="prerequisites"></a>Eeltingimused

Peab teil olema teie arvutisse installitud järgmine: 

- Visual Studio 2013 või Visual Studio 2015
- Laadige alla Azure SDK Visual Studio 2013 või Visual Studio 2015. [Azure'i allalaadimine lehele](https://azure.microsoft.com/downloads/) liikumiseks ja klõpsake **VS 2013** või **.NET** jaotises **VS 2015** .
- Visual Studio uusima Azure'i andmed Factory lisandmooduli allalaadimine: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) või [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Kui kasutate Visual Studio 2013, saate värskendada ka selle lisandmooduli, tehes järgmist: klõpsake menüü **Tööriistad** -> **laiendid ja värskenduste** -> **Online'i** -> **Visual Studio galerii** -> **Microsoft Azure'i Factory Tools for Visual Studio** -> **värskendada**.

### <a name="use-data-factory-templates"></a>Andmete Factory mallide kasutamine

1. Klõpsake menüü **fail** käsku **Uus**ja valige **Project**. 
2. Dialoogiboksis **Uus projekt** tehke järgmist. 
    1. Valige jaotises **Mallid** **DataFactory** . 
    2. Valige parempoolsel paanil **Andmete Factory Mallid** . 
    3. Sisestage projekti **nime** . 
    4. Valige projekti **asukoht** . 
    5. Klõpsake nuppu **OK**. 

    ![Dialoogiboks uus projekt](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. Dialoogiboksis **Andmete Factory Mallid** näidismall valige jaotises **Kasuta puhul Mallid** ja klõpsake nuppu **edasi**. Järgmised toimingud sõelub **Kliendi profiili** malli abil. Juhised kehtivad sarnaselt muude proovi. 

    ![Dialoogiboksis andmete Factory Mallid](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. **Andmete Factory konfiguratsiooni** dialoogi **järgmisel** lehel nuppu **Andmete Factory põhitõed** .
8. Lehel **konfigureerimine andmete factory** tehke järgmist. 
    1. Valige **Loo uus andmete Factory**. Saate **kasutada olemasolevaid andmeid factory**.
    2. Sisestage andmed factory **nimi** .
    3. Valige **Azure'i tellimus** , kuhu soovite andmed factory luua. 
    4. **Ressursirühm** Factory andmete valimine
    5. Valige **Lääne USA**, **Ida-USA**või **Põhja-Euroopa** **piirkonna**jaoks.
    6. Klõpsake nuppu **edasi**. 
9. Lehel **konfigureerimine andmete talletab** määrata mõne olemasoleva **SQL Azure'i andmebaas** ja **Azure storage konto** (või) luua andmebaasi/salvestusruumi ja nuppu edasi. 
10. Klõpsake lehel **konfigureerimine arvutada** valige vaikesätted ja klõpsake nuppu **edasi**. 
11. Vaadake üle kõik sätted **kokkuvõtteleht,** ja klõpsake nuppu **edasi**. 
12. **Juurutamise oleku** leht, oodake, kuni juurutamise on lõpule jõudnud, ja klõpsake nuppu **valmis**.
13. Paremklõpsake Solution Exploreris projekti, ja klõpsake nuppu **Avalda**. 
19. Kui kuvatakse dialoogiboks **oma Microsofti kontoga sisse logida** , sisestage mandaat, mis on Azure tellimuse konto ja klõpsake nuppu **Logi sisse**.
20. Peaksite nägema järgmine dialoogiboks:

    ![Dialoogiboks avaldamine](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. **Konfigureerimine andmete factory** lehel tehke järgmist. 
    1. Kinnitage seda suvandit **Kasuta olemasolevaid andmeid factory** .
    2. Valige **andmete factory** olid valige kui malli abil. 
    6. Klõpsake nuppu **Järgmine** lehe **Avaldamine üksuste** kuvamiseks. (Vajutage klahvi **MENÜÜS** liikumiseks välja nimi välja, kui nupp **Järgmine** on keelatud.) 
23. **Üksuste avaldamine** lehel tagada, et kõik andmed tehased üksused on valitud ja klõpsake nuppu **Järgmine** lehe **Kokkuvõte** kuvamiseks.     
24. Kokkuvõte ja klõpsake nuppu **Järgmine** juurutamise protsessi käivitamiseks ja **Juurutamise oleku**vaatamine.
25. **Juurutamise oleku** leht, peaksite nägema oleku juurutamise käigus. Kui juurutamise on töö lõpetanud, klõpsake nuppu valmis. 

Visual Studio abil andmete Factory üksuste koostamine ja avaldamine Azure'i üksikasjad leiate [koostada esimese andmete factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) .          