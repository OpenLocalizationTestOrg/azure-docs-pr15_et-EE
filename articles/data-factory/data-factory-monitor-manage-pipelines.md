<properties 
    pageTitle="Jälgimine ja haldamine Azure'i andmed Factory torujuhtmete" 
    description="Saate teada, kuidas Azure'i portaal ja Azure PowerShelli abil jälgimine ja haldamine Azure'i andmed tehased ja torujuhtmed loomist." 
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
    ms.date="09/06/2016" 
    ms.author="spelluru"/>


# <a name="monitor-and-manage-azure-data-factory-pipelines"></a>Jälgimine ja haldamine Azure'i andmed Factory torujuhtmete
> [AZURE.SELECTOR]
- [Azure'i portaalis Azure PowerShelli abil](data-factory-monitor-manage-pipelines.md)
- [Kasutades jälgimise ja halduse rakenduse](data-factory-monitor-manage-app.md)

Andmete Factory teenus pakub usaldusväärsest ja täieliku ülevaate säilitamiseks, töötlemiseks ja andmete liikumise teenuseid. Teenus pakub teile armatuurlaua jälgimine aitab, mille abil saate teha järgmisi toiminguid: 

- Hinnake kiiresti lõpuni andmete müügivõimaluste seisund.
- Probleemide tuvastamine ja võtmine liikmete vajaduse korral. 
- Jälita andmete pärandi. 
- Jälgida kõigis allikate andmete vahel seoseid.
- Kuva täieliku ajaloo arvestus töö täitmise, süsteemi seisundi ja sõltuvused.

Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja oma torujuhtmete silumine. See sisaldab ka teavet teatiste loomine ja teatise saamine kindla tõrgete kohta.

## <a name="understand-pipelines-and-activity-states"></a>Tegevuste olekud ja torujuhtmete mõistmine
Azure portaali kasutamisel saate teha järgmist.

- Saate vaadata oma andmeid factory diagrammina
- Müügivõimaluste tegevuste kuvamine
- Sisend- ja andmekomplektide kuvamine
- ja palju muud. 

Sellest jaotisest leiate ka, kuidas mõnda sektorit üleminekud ühe riigi teise.   

### <a name="navigate-to-your-data-factory"></a>Liikuge oma andmete factory
1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
2.  Klõpsake vasakpoolses menüüs **andmed tehased** . Kui te ei näe, klõpsake **rohkem teenuseid >** ja klõpsake käsku **andmete tehased** **ÄRIANALÜÜSI + ANALYTICS** kategoorias. 

    ![Sirvige kõik -> andmete tehased](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)

    Peaksite nägema andmete tehased **andmete tehased** tera. 
4. Andmete tehased tera, valige andmete factory, mis teid huvitab.

    ![andmete valimine factory](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)  
5.  ja näete andmete factory Avaleht (**andmete factory** blade).

    ![Andmete factory blade](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Diagrammivaate oma andmete factory.
Diagrammi vaate andmete factory pakub ühe paani klaas jälgida ja andmete hankimise ja varade haldamine.

Oma andmete factory diagrammi vaate kuvamiseks klõpsake **diagrammi** andmete factory avalehel.

![Diagrammivaate](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Suurendamine, vähendamine, suumi mahutamiseks, suumi 100%, lukustada skemaatilise diagrammi paigutuse ja paigutage automaatselt torujuhtmed ja tabelid. Saate vaadata ka selle pärandi teave (enne ja üksuste valitud üksuste kuvamine).
 

### <a name="activities-inside-a-pipeline"></a>Müügivõimaluste tegevusi 
1. Paremklõpsake soovitud müügivõimaluste ja klõpsake nuppu **Ava müügivõimaluste** näha kõiki tegevusi tulemas koos sisend- ja andmekomplektide tegevuste jaoks. See funktsioon on kasulik, kui teie müügivõimaluste sisaldab rohkem kui ühe tegevuse ja te soovite mõistmine ühe müügivõimaluste funktsionaalseid liini.

    ![Avatud müügivõimaluste menüü](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)  
2. Järgmises näites kuvatakse kaks tegevuste teel oma sisendeid ja väljundeid. Tegevuse **JoinData** Hdinsightiga taru tegevuse tüüp ja **EgressDataAzure** Kopeeri tegevuse tüüp on selle valimi kohaletoimetamisel. 
    
    ![Müügivõimaluste tegevusi](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png) 
3. Saate liikuda andmete Factory avalehele naasmiseks andmete factory lingirea ülemises vasakus nurgas linki klõpsates.

    ![Liikuge tagasi andmete factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-state-of-each-activity-inside-a-pipeline"></a>Kuva olek sees müügivõimaluste iga tegevuse
Saate vaadata tegevuse praeguse oleku, mis tahes andmekomplektide, toodetud tegevuse oleku vaatamine. 

Näide: järgmises näites **BlobPartitionHiveActivity** käivitus edukalt ja toodeti andmekomplekt nimega **PartitionedProductsUsageTable**, mis on **valmis** olekus.

![Müügivõimaluste olek](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Topeltklõpsake **PartitionedProductsUsageTable** diagrammivaates tutvustatakse kõik sektorid toodetud muu tegevuse käivitatakse müügivõimaluste sees. Saate vaadata **BlobPartitionHiveActivity** käivitus edukalt iga kuu jaoks viimase kaheksa kuu ja toodeti sektorid **valmis** olekus.

Andmekomplekti sektorite andmete factory võib olla üks järgmistest olekutest.

<table>
<tr>
    <th align="left">Olek</th><th align="left">Substate</th><th align="left">Kirjeldus</th>
</tr>
<tr>
    <td rowspan="8">Ootel</td><td>ScheduleTime</td><td>On aeg pole käivitamiseks sektorit.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Varustava sõltuvused ei ole valmis.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Arvuta ressursid pole saadaval.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Kõik tegevuse eksemplarid on hõivatud muude sektorite töötab.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Tegevuste on peatatud ja ei saa käivitada sektorid seni, kuni see jätkatakse.</td>
</tr>
<tr>
<td>Proovige</td><td>Tegevuste täitmise uuesti proovida.</td>
</tr>
<tr>
<td>Valideerimine</td><td>Valideerimine pole veel alanud.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Ootamine valideerimise abil uuesti proovida.</td>
</tr>
<tr>
<tr
<td rowspan="2">InProgress</td><td>Kontrollimine</td><td>Valideerimine on pooleli.</td>
</tr>
<td></td>
<td>Töötlemise sektorit.</td>
</tr>
<tr>
<td rowspan="4">Nurjus.</td><td>Ajalõppe</td><td>Täitmise võttis oodatust, mis on lubatud tegevuse.</td>
</tr>
<tr>
<td>Tühistatud</td><td>Kasutaja toiming tühistada.</td>
</tr>
<tr>
<td>Valideerimine</td><td>Valideerimine nurjus.</td>
</tr>
<tr>
<td></td><td>Ei saanud luua ja/või kinnitage sektorit.</td>
</tr>
<td>Kas olete valmis</td><td></td><td>Sektorit on kasutusvalmis.</td>
</tr>
<tr>
<td>Vahele</td><td></td><td>Ei töödelda sektorit.</td>
</tr>
<tr>
<td>Ükski</td><td></td><td>Sektorit, mida kasutada mõne muu seisundiga on olemas, kuid on lähtestatud.</td>
</tr>
</table>



Saate tükk üksikasjade vaatamiseks klõpsata sektorit kirje **Viimati värskendatud sektorid** tera.

![Sektorit üksikasjad](./media/data-factory-monitor-manage-pipelines/slice-details.png)
 
Kui sektorit võtmist mitu korda, kuvatakse loendis **tegevuse töötab** mitu rida. Saate vaadata tegevuse käivitamiseks nuppu Käivita kirje loendis **tegevuse töötab** üksikasjad. Loendis kuvatakse kõik logifailid kuvatakse tõrketeade, kui mõni koos. See funktsioon on kasulik vaadata ja silumine logid ilma oma andmete factory lahkuda.

![Tegevuste käivitamine üksikasjad](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Kui **olete valmis** olekus pole sektorit, näete varustava sektoritele, mis ei ole valmis ja blokeerivad kaudu käivitamisel **varustava sektoritele, mis on valmis pole** loendis praegune sektorit. See funktsioon on kasulik, kui teie sektorit on **Ootel** olekus ja te soovite mõista varustava sõltuvused, kus ootavad sektorit.

![Enne sektorid ei ole valmis](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Andmekomplekti olekus skeem
Kui juurutate andmete factory ja torujuhtmetele kehtiv aktiivse perioodi, viilud andmekomplekti ülemineku ühest riigist teise. Praegu järgib sektorit oleku olek järgmisel joonisel:

![Maakond skeem](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Andmekomplekti olekus ülemineku meilivoo andmete factory: ootel -> sisse-edenemist/pooleli (Validating) -> valmis/nurjunud

**Ootel** riigi jaoks eelse tingimused, enne sektorid käivitada. Seejärel tegevuse käivitab täitmine ja sektorit läheb **Pooleli** olekus. Tegevuste täitmise võib õnnestub või ei. **Lõpetatuks**märgitud sektorit ' või **nurjunud** täitmise tulemi põhjal. 

Saate lähtestada sektorit uuesti kasutusele **valmis** või **nurjunud** **Ootel** olekusse. Saate ka märkida **Jäta**, mis takistab tegevuse käivitamisel sektorit riigi ja Töötle sektorit.


## <a name="manage-pipelines"></a>Torustike haldamine
Saate hallata oma torujuhtmete Azure PowerShelli kaudu. Näiteks saate viige ja jätkamine torujuhtmete Azure PowerShelli cmdlet-käskude abil. 

### <a name="pause-and-resume-pipelines"></a>Viige ja torujuhtmete jätkamine
Te saate paus/peatada torujuhtmete **Peata-AzureRmDataFactoryPipeline** PowerShelli cmdlet-käsu abil. Selle cmdlet-käsu on kasulik, kui te ei soovi oma torujuhtmete käivitada enne, kui mõni probleem on lahendatud.

Näide: järgmine ekraanipilt on tuvastatud probleemi koos **PartitionProductsUsagePipeline** **productrecgamalbox1dev** andmete factory sisse ja soovime tulemas peatada.

![Müügivõimaluste peatatakse](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

Müügivõimaluste peatamiseks käivitage PowerShelli järgmine käsk:

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Näiteks:

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

Kui probleem on lahendatud koos **PartitionProductsUsagePipeline**, saate jätkata peatatud müügivõimaluste järgmist PowerShelli käsu:

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Näiteks:

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## <a name="debug-pipelines"></a>Torujuhtmete silumine
Azure'i andmed Factory pakub rikkaliku Azure portaali ja Azure PowerShelli silumine ja tõrkeotsingu torujuhtmete kaudu.

### <a name="find-errors-in-a-pipeline"></a>Müügivõimaluste tõrgete otsimine
Tegevuste käivitamine nurjumisel müügivõimaluste andmekomplekti toodetud tulemas on tõrkeolekus tõttu. Saate silumine ja tõrkeotsing Azure'i andmed Factory, kasutades järgmisi elemente.

#### <a name="use-azure-portal-to-debug-an-error"></a>Azure'i portaali abil silumine tõrge.

3.  Klõpsake **tabeli** labale **olek** on **nurjunud**määratud probleemi sektorit.

    ![Tabeli abaluuga probleemi sektorit](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4.  Klõpsake **Andmete sektorit** tera, käivitamine, mida ei saanud tegevuse.
    
    ![Dataslice viga](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5.  **Tegevuste käivitamine üksikasjad** tera, saate alla laadida Hdinsightiga töötlemine seostatud failide. Klõpsake olek/stderr tõrke logifaili, mis sisaldab tõrke üksikasjade alla laadida.

    ![Tegevuste käivitamine üksikasjad blade viga](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)  

#### <a name="use-the-powershell-to-debug-an-error"></a>Tõrke silumine on PowerShelli abil
1.  Käivitage **PowerShelli Azure**.
3.  **Get-AzureRmDataFactorySlice** käsu sektorid ja nende olekuid kuvamiseks. Peaksite nägema tükk olekuga: **nurjunud**.       

            Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Näiteks:
        
            Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Asendage **StartDateTime** Set-AzureRmDataFactoryPipelineActivePeriod jaoks määratud StartDateTime väärtuse.
4. Nüüd, käivitage **Get-AzureRmDataFactoryRun** cmdlet-käsk saada üksikasjade tegevuste käivitamine sektorit.

        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Näiteks:

        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    StartDateTime väärtus on märgitud eelmises juhises viga/probleem sektorit algusaeg. Kuupäeva / kellaaja peaks olema jutumärkides.
5.  Peaksite nägema väljund üksikasjadega (sarnaneb järgmisega) tõrke kohta:

            Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
            ResourceGroupName       : ADF
            DataFactoryName         : LogProcessingFactory3
            TableName               : EnrichedGameEventsTable
            ProcessingStartTime     : 10/10/2014 3:04:52 AM
            ProcessingEndTime       : 10/10/2014 3:06:49 AM
            PercentComplete         : 0
            DataSliceStart          : 5/5/2014 12:00:00 AM
            DataSliceEnd            : 5/6/2014 12:00:00 AM
            Status                  : FailedExecution
            Timestamp               : 10/10/2014 3:04:52 AM
            RetryAttempt            : 0
            Properties              : {}
            ErrorMessage            : Pig script failed with exit code '5'. See wasb://     adfjobs@spestore.blob.core.windows.net/PigQuery
                                            Jobs/841b77c9-d56c-48d1-99a3-
                        8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                        more details.
            ActivityName            : PigEnrichLogs
            PipelineName            : EnrichGameLogsPipeline
            Type                    :
    
    
6.  Cmdlet-käsk **Salvesta-AzureRmDataFactoryLog** ettekandmiseks Id väärtus, mida näha väljund ja cmdlet **-DownloadLogsoption** kasutamise logifailide alla laadida.

            Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"


## <a name="rerun-failures-in-a-pipeline"></a>Käivitage uuesti müügivõimaluste tõrked

### <a name="using-azure-portal"></a>Azure'i portaalis

Kui olete tõrkeotsing ja silumine müügivõimaluste tõrked, käivitage tõrkeid tõrge sektorit liikumine ja käsuribal nuppu **Käivita** .

![Nurjunud sektorit uuesti](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Juhuks, kui sektorit ei ole valideerimine poliitika tõrke tõttu (jaoks ex: andmed pole saadaval), saate selle tõrke lahendamiseks ja kinnitage uuesti käsuribal nuppu **Valideeri** .
![Tõrgete parandamine ja kinnitage](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="using-azure-powershell"></a>Azure'i PowerShelli abil

Käivitage cmdlet Set-AzureRmDataFactorySliceStatus abil ebaõnnestumist. Teemast [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) teemas süntaks ja muud üksikasjad cmdlet-käsu kohta. 

**Näide:** Järgmises näites määrab kõik sektorid tabeli olekut 'DAWikiAggregatedData' ootamine Azure'i andmed factory "WikiADF" sisse.

Funktsiooni UpdateType on seatud UpstreamInPipeline, mis tähendab, et iga tabeli jaoks sektorit ja sõltuvad (varustava) tabelite olekud on määratud "Ootamine." Muude võimalike väärtus see parameeter on "Individuaalse."

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## <a name="create-alerts"></a>Teatiste loomine
Azure'i logid kasutaja sündmusi, kui on Azure ressurss (nt andmete factory) on loodud, värskendada või kustutada. Saate luua nende sündmuste teatised. Andmete Factory võimaldab teil jäädvustada erinevate mõõdikute ja mõõdikute kohta teatiste loomine. Soovitame kasutada ajalooliste eesmärgil reaalajas jälgimiseks ja mõõdikute sündmused. 

### <a name="alerts-on-events"></a>Sündmuste teatised
Azure'i sündmuste anda kasulik teadmisi, mis toimub oma Azure ressursse. Azure'i logid kasutaja sündmusi, kui mõni Azure ressursi (nt andmete factory) on loodud, värskendada või kustutada. Azure'i andmed Factory kasutamisel luuakse sündmuste kui:

- Azure'i andmed Factory on värskendatud, loodud ja kustutatud.
- Andmete töötlemise (kutsutud käivitatakse) on alustamine/lõpule viidud.
- Kuvatakse nõudmisel Hdinsightiga kobar on loodud ja eemaldada.

Saate neid kasutaja sündmuste kohta teatiste loomine ja konfigureerimine need administraator ja kaasadministraatorite tellimuse meiliteatiste saatmiseks. Lisaks saate määrata täiendavate meiliaadresside kasutajad, kellel on vaja e-posti teatisi, kui tingimused on täidetud. See funktsioon on kasulik, kui soovite saada teada, tõrkeid ja ei soovi pidevalt jälgida oma andmete factory.

> [AZURE.NOTE] Praegu ei kuvata portaali teatiste sündmuste kohta. [Jälgimine ja haldamine rakenduse](data-factory-monitor-manage-app.md) abil saate kuvada kõik teatised.

#### <a name="specifying-an-alert-definition"></a>Saate määrata teatiste määratlus on:
Selle teatiste määratluse määramiseks loote JSON fail, mis kirjeldab toiminguid, mida soovite kohta teatisi. Järgmises näites teatise saadab meiliteatise RunFinished toimingu jaoks. Olge täpne, meiliteatise saadetakse kui soovitud andmed factory Käivita on lõpule viidud ja Käivita ei ole (oleku = FailedExecution).

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

JSON määratlus, **subStatus** saab eemaldada, kui te ei soovi teatud tõrke kohta teatisi.

Selles näites häälestab teatise jaoks kõik andmed tehased teie tellimus. Kui soovite teatise olema kindla andmete factory seadistatud, saate määrata **andmeallika**andmete factory **resourceUri** :

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

Järgmises tabelis toodud toimingud ja olekuid (ja sub olekud) loend.

Toimingu nimi | Olek | Sub olek
-------------- | ------ | ----------
RunStarted | Alustamine | Käivitamine
RunFinished | Katse nurjus / õnnestus | FailedResourceAllocation<br/><br/>Õnnestus<br/><br/>FailedExecution<br/><br/>Ajalõppe<br/><br/>< tühistatud<br/><br/>FailedValidation<br/><br/>Hüljatud
OnDemandClusterCreateStarted | Alustamine
OnDemandClusterCreateSuccessful | Õnnestus
OnDemandClusterDeleted | Õnnestus

[Reegli loomine](https://msdn.microsoft.com/library/azure/dn510366.aspx) kohta vaadake teavet kasutatud näites JSON-elemente. 

#### <a name="deploying-the-alert"></a>Teatise juurutamine 
Teatise juurutamiseks kasutada Azure PowerShelli cmdlet-käsk: **Uus-AzureRmResourceGroupDeployment**, nagu on näidatud järgmises näites:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

Kui ressursi rühma juurutamise on lõpule jõudnud, näete järgmist teavet:

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

> [AZURE.NOTE] [Reegli loomine teatise](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API abil saate luua reegli. JSON last on sarnane JSON näide.  

#### <a name="retrieving-the-list-of-azure-resource-group-deployments"></a>Azure'i ressursi rühma juurutuste loendi toomine
Juurutatud Azure'i ressursirühm juurutuste loendi toomiseks kasutada i cmdlet: **Get-AzureRmResourceGroupDeployment**, nagu on näidatud järgmises näites:

    Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### <a name="troubleshooting-user-events"></a>Kasutaja sündmuste tõrkeotsing


1. Kuvatakse kõik sündmused, mis on loodud pärast paani **mõõdikute ja toimingud** .

    ![Paani mõõdikute ja toimingud](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)

2. Klõpsake **sündmuste** paani sündmuste kuvamiseks. 

    ![Sündmuste paan](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. **Sündmuste** tera, saate näha üksikasjalikku teavet sündmuste, filtreerimine sündmused jne. 

    ![Sündmuste blade](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Klõpsake **toimingu** toimingute loend, mis põhjustab tõrke.
    
    ![Soovitud toiming](./media/data-factory-monitor-manage-pipelines/select-operation.png) 
5. Klõpsake sündmust **tõrge** tõrke üksikasjade kuvamiseks.

    ![Sündmuse tõrge](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)
  

Teemast [Azure ülevaate cmdlettide](https://msdn.microsoft.com/library/mt282452.aspx) artiklis PowerShelli cmdlet-käsud, mille abil saate lisada või saada/eemaldamine teatiste jaoks. Siin on mõned näited **Get-AlertRule** cmdlet-käsu abil. 


    PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
        
            Properties :
            Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
            Condition   :
            DataSource :
            EventName             :
            Category              :
            Level                 :
            OperationName         : RunFinished
            ResourceGroupName     :
            ResourceProviderName  :
            ResourceId            :
            Status                : Failed
            SubStatus             : FailedExecution
            Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
            Condition   :
            Description : One or more of the data slices for the Azure Data Factory has failed processing.
            Status      : Enabled
            Name:       : ADFAlertsSlice
            Tags       :
            $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
            Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
            Location   : West US
            Name       : ADFAlertsSlice
    
    PS C:\> Get-AlertRule -res $resourceGroup

            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
            Location   : West US
            Name       : FailedExecutionRunsWest3

    PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0

Käivitage järgmised abi saamiseks käsud kuvamiseks üksikasjad ja Get-AlertRule cmdlet näited. 

    get-help Get-AlertRule -detailed 
    get-help Get-AlertRule -examples


- Kui näete portaali enne teatiste sündmused, kuid te ei saa meiliteatised, kontrollige, kas määratud e-posti aadress on määratud välistelt saatjatelt saabuvaid kirju. Teatiste e-kirju võib olla blokeeritud e-posti teel.

### <a name="alerts-on-metrics"></a>Mõõdikute teatised
Andmete Factory võimaldab teil jäädvustada erinevate mõõdikute ja mõõdikute kohta teatiste loomine. Saate jälgida ja teatiste loomine rakenduses oma andmete factory kohta järgmised mõõdikute sektorite jaoks.
 
- Nurjunud käivitatakse
- Eduka käivitatakse

Need mõõdikud on kasulikud ja võimaldab teil saada ülevaate üldiselt nurjunud ja eduka käivitatakse nende andmete factory. Mõõdikute eraldub iga kord, kui seal on sektorit Käivita. Peal tunni, on need mõõdikud kokku liita ja lükata salvestusruumi kontosse. Seetõttu mõõdikute lubamiseks salvestusruumi konto häälestamine.


#### <a name="enabling-metrics"></a>Lubamine mõõdikute:
Mõõdikute lubamiseks klõpsake andmete Factory tera kaudu järgmist:

**Jälgimine** -> **väärtuseks meetermõõdustik** -> **diagnostikasätete** -> **diagnostika**

![Diagnostika link](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

**Diagnostika** enne **klõpsake** ja valige konto salvestusruumi ja salvestada.

![Diagnostika blade](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Kui salvestatud, võib kuluda kuni tund mõõdikute nähtavad jälgimisega seotud enne, kuna mõõdikute koondamine toimub tunni jaoks.


### <a name="setting-up-alert-on-metrics"></a>Kuidas häälestada mõõdikute teatis:

Klõpsake **andmete Factory mõõdikute** keel: 

![Andmete factory mõõdikute paan](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

**Meetermõõdustik** enne, klõpsake nuppu **+ Lisa teatise** tööriistariba. 
![Andmete factory argumendil blade - teatise lisamine](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Klõpsake lehel **reegli lisamine** tehke järgmised toimingud ja klõpsake nuppu **OK**.
 
- Sisestage teatise nimi (näide: nurjus teatise).
- Sisestage teatise kirjeldus (näide: meilisõnumit saata, kui ilmneb tõrge).
- Valige mõõdiku (nurjunud käivitatakse vs eduka käivitatakse).
- Määrake tingimus ja läve.   
- Määrake soovitud periood. 
- Määrake, kas e-posti saata omanikud, kaasautorid ja lugejad.
- ja palju muud. 

![Andmete factory argumendil blade - teatise lisamine](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Pärast reegli lisamist edukalt, tera suletakse ja kuvatakse **meetermõõdustik** lehel Uus teatis. 

![Andmete factory argumendil blade - teatise lisamine](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Peaksite nägema teatiste arvu **teatiste** paani. Klõpsake paani **teatised** .

![Andmete factory argumendil blade - teatis reeglid](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

**Teatiste** tera, kuvatakse kõik olemasolevad teatised. Teatise lisamiseks klõpsake tööriistaribal nuppu **Lisa teatis** .

![Teatiste reeglid blade](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Teatiste:
Kui reeglit vastab tingimus, saate meilisõnumi teatis aktiveeritud. Kui probleem on lahendatud ja teatiste tingimus ei vasta enam, saate meilisõnumi teatis lahendatud.

Selline käitumine erineb sündmuste kus saadetakse teatis iga tõrke kohta mis reegli kinnitatakse.

### <a name="deploying-alerts-using-powershell"></a>Teatiste PowerShelli kaudu juurutamine
Saate juurutada teatised mõõdikute samamoodi nagu teilgi sündmuste jaoks. 

**Teatiste määratlus:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }
 
Asendage väärtused subscriptionId, resourceGroupName ja dataFactoryName valimis.

*metricName* praegu toetab kahest väärtusest:
- FailedRuns
- SuccessfulRuns

**Juurutamine teatise:**

Teatise juurutamiseks kasutada Azure PowerShelli cmdlet-käsk: **Uus-AzureRmResourceGroupDeployment**, nagu on näidatud järgmises näites:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

Peaksite nägema pärast sõnumi eduka juurutamise:

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded
    
    
    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           


**Lisa-AlertRule** cmdlet-käsu abil Reegli juurutamine. [Lisa-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) teemat üksikasjad ning näited.  

## <a name="move-data-factory-to-a-different-resource-group-or-subscription"></a>Teisalda andmed factory erinevate ressursirühm või tellimusele
Oma andmete factory avalehel **teisaldamine** riba käsunupu abil saate teisaldada erinevate ressursirühm või mõnda muud tellimust andmete factory. 

![Andmete factory teisaldamine](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Saate teisaldada mis tahes seotud ressursid (nt teatised, mis on seotud andmete factory) ka factory andmeid koos.

![Dialoogiboksi teisaldamine](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
