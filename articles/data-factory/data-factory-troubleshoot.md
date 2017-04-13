<properties 
    pageTitle="Azure'i andmed Factory probleemide tõrkeotsing" 
    description="Saate teada, kuidas Azure'i andmed Factory seotud probleemide tõrkeotsing." 
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
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Andmete Factory probleemide tõrkeotsing
Sellest artiklist leiate probleemide tõrkeotsingu näpunäiteid Azure'i andmed Factory kasutamisel. Selles artiklis ei loetleta võimalikke probleeme, kui kasutate teenust, kuid see hõlmab teatud ja üldine tõrkeotsingu näpunäiteid.   

## <a name="troubleshooting-tips"></a>Tõrkeotsingu näpunäited

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Tõrge: Tellimus on registreeritud kasutada nimeruum "Microsoft.DataFactory"
Kui kuvatakse järgmine tõrketeade, Azure'i andmed Factory ressursi pakkuja teie arvutis pole registreeritud. Tehke järgmist. 

1. Käivitage PowerShelli Azure. 
2. Logige sisse oma Azure'i konto järgmise käsu abil.
        Logi sisse – AzureRmAccount 
3. Käivitage järgmine käsk Azure'i andmed Factory pakkuja registreerimiseks.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Probleem: Volitamata viga andmete Factory cmdlet-käsu käivitamisel
Kasutate Azure PowerShelliga ilmselt ei paremale Azure'i konto või tellimus. Parem Azure'i konto ja Azure PowerShelli kasutamine tellimus valimiseks kasutage järgmised cmdlet-käsud. 

1. Logi sisse – AzureRmAccount – kasutage õige kasutaja ID ja parool
2. Get-AzureRmSubscription – kõik selle konto tellimuste vaatamine. 
3. Valige-AzureRmSubscription &lt;tellimuse nime&gt; – valige see õige tellimus. Kasutage sama ühe abil saate luua andmete factory Azure'i portaalis.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Probleem: Ei õnnestu käivitada andmete Management Gateway kiire häälestamise Azure'i portaalis
Andmehalduslüüsi Express häälestamise nõuab Internet Exploreri või Microsoft ClickOnce'i ühilduva veebibrauseri kaudu. Kui kiire häälestamine ei käivitu, tehke ühte järgmistest. 

- Kasutage Internet Explorer või Microsoft ClickOnce'i ühilduvad veebibrauseris.

    Kui kasutate Chrome'i, lugege [Chrome'i](https://chrome.google.com/webstore/), otsida märksõna "ClickOnce'i" järgi, valige üks ClickOnce'i laiendid ja installige see. 
    
    Tehke sama Firefox (installi lisandmoodul). Tööriistariba (kolme horisontaalsed jooned ülemises paremas nurgas), klõpsake nuppu Ava menüü nuppu klõpsake lisandmoodulid, otsida märksõna "ClickOnce'i" järgi, valige üks ClickOnce'i laiendid ja installige see. 

- Kasutage **Käsitsi häälestus** link, kuvatakse sama enne portaalis. Seda moodust abil installi faili alla laadida ja käivitada käsitsi. Pärast installi õnnestumise, kuvatakse dialoogiboks andmete haldamise lüüsi konfigureerimine. Portaali kuval **klahvi** kopeerida ja kasutada seda configuration manager teenuse lüüsi käsitsi registreerimiseks.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Probleem: Nurjuda asutusesisese SQL serveriga ühenduse loomiseks 
Käivitage **Andmete Andmehalduslüüsi Konfigureerimishalduri** lüüsiseadmega ja SQL serveriga ühenduse testimiseks arvutist lüüsi vahekaardil **tõrkeotsing** . Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Probleem: Sisendi sektorid on ootel olekus kunagi

Sektorite võiks **oodata** olekus mitmel põhjusel. Üks levinud põhjuseid on **välised** atribuut on seatud **tõene**. Mis tahes andmekomplekti valmistatakse Azure'i andmed Factory väljapoole peaks tähistatud **välise** atribuut. Atribuut näitab, et andmed on välise ja ei varundatud mis tahes torujuhtmetes andmete factory sees. Andmete sektorid märgitakse **lõpetatuks** kui andmed on vastava poes saadaval. 

Vt järgmist näidet kasutamine **väliste** atribuudi. Soovi korral saate määrata **externalData*** kui seate välise tõene.

Vt [andmekomplektide](data-factory-create-datasets.md) artikkel selle atribuudi kohta lisateabe saamiseks.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Tõrke lahendamiseks JSON määratlus sisendtabel **välise** atribuudi ja valikuline **externalData** jaotises lisamine ja looge tabel. 

### <a name="problem-hybrid-copy-operation-fails"></a>Probleem: Hübriid koopia toiming nurjub
Vaadata [lüüsi probleemide tõrkeotsingu](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) juhised probleemide tõrkeotsingu kohapealse andmesalve abil Andmehalduslüüsi ja sealt kopeerimine. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Probleem: Nõudmisel Hdinsightiga ettevalmistamise nurjub?
Lingitud teenuse tüüpi HDInsightOnDemand kasutamisel peate määrama linkedServiceName, mis viitab mõni Azure'i bloobimälu. Factory andmeteenuse kasutab säilitamise logid ja klaster nõudmisel Hdinsightiga täiendavad failid salvestada.  Mõnikord on nõudmisel Hdinsightiga klaster ettevalmistamise nurjub järgmine tõrketeade:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

See tõrge näitab tavaliselt salvestusruumi konto on linkedServiceName määratud asukohta pole kus toimub Hdinsightiga ettevalmistamise andmete keskmist paigal. Näide: kui teie andmed factory on Lääne USA ja Azure salvestusruumi on Ida-USA, nõudmisel ettevalmistamise nurjub Lääne US.

Lisaks on teine JSON atribuudi additionalLinkedServiceNames kus täiendavat salvestusruumi kontod täpsustatud nõudmisel Hdinsightiga. Need lingitud salvestusruumi kontod peaks olema Hdinsightiga kobar samasse asukohta või sama viga ei ole.

### <a name="problem-custom-net-activity-fails"></a>Probleem: Kohandatud .NET tegevuse nurjub
Üksikasjalikud juhised leiate [silumine müügivõimaluste kohandatud toimingute](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Azure'i portaali abil saate otsida 

### <a name="using-portal-blades"></a>Portaali labad abil
[Müügivõimaluste jälgimine](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) leiate üksikasjalikud juhised. 

### <a name="using-monitor-and-manage-app"></a>Kuvari abil ja rakenduse haldamine
Vt [jälgimine ja haldamine andmete factory torujuhtmete jälgimine ja haldamine rakenduse abil](data-factory-monitor-manage-app.md) üksikasju. 

## <a name="use-azure-powershell-to-troubleshoot"></a>PowerShelli kasutamine Azure tõrkeotsing

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Azure'i PowerShelli abil saate otsida tõrge  
Üksikasjad leiate [kuvari andmete Factory torujuhtmete Azure PowerShelli kaudu](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 