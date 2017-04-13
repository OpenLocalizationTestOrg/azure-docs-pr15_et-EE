<properties
   pageTitle="Lahenduste loomise toimingute haldus komplekti (OMS) | Microsoft Azure'i"
   description="Lahenduste laiendavad toimingud halduse komplekti (OMS) esitada pakkimisel halduse stsenaariumi, mis kliendid saate lisada oma OMS tööruumi.  Selles artiklis esitatud üksikasjad kohta, kuidas saate luua lahendusi kasutada oma keskkonnas või oma klientidele kättesaadavaks."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Lahenduste loomine toimingud halduse komplekti (OMS) (eelvaade)

>[AZURE.NOTE]See on esialgse dokumentatsioonist loomine lahenduste OMS, mis on praegu eelvaade. Mis tahes allpool kirjeldatud skeemi on muutuda.  

Lahenduste laiendavad toimingud halduse komplekti (OMS) esitada pakkimisel halduse stsenaariumi, mis kliendid saate lisada oma OMS tööruumi.  Selles artiklis antakse üksikasjad oma lahendusi, et saate kasutada oma keskkonnas või klientidele kaudu ühenduse loomise kohta.

## <a name="planning-your-management-solution"></a>Teie lahendus kavandamine
Lahenduste haldus OMS kaasata mitu ressursse, mis toetavad kindla haldamise stsenaarium.  Teie lahendus kavandamisel tuleks Keskenduge stsenaarium, mis te soovite saavutada ja kõik nõutavad ressursse, mis toetavad seda.  Iga lahenduse peaks olema autonoomne ja määratleda iga ressurss, mida on vaja, isegi siis, kui üks või mitu ressursid on määratletud ka muid lahendusi.  Kui haldamise lahendus on installitud, luuakse iga ressursi juhul, kui see on juba olemas, ja saate määratleda, mis juhtub ressursid kui lahendus on eemaldatud.  

Näiteks võivad haldamise lahendus on [Azure automatiseerimine käitusjuhendi](../automation/automation-intro.md) , mis kogub andmeid [ajakava](../automation/automation-schedules.md) ja [Vaade](../log-analytics/log-analytics-view-designer.md) , mis pakub erinevate visualiseeringute kogutud andmete kasutamise Log Analytics hoidlasse.  Sama ajakava võib kasutada muu lahendus.  Autorina halduse lahenduse saate määratleda kõigi kolme ressursse kuid määrata, et käitusjuhendi ja vaade peaks automaatselt, eemaldatakse lahendus on eemaldatud.    Saate ka määratleda ajakava kuid määrata, et see peaks jääma kohas, kui lahendus eemaldatud juhuks, kui see oli veel kasutusel, muu lahendus.

## <a name="management-solution-files"></a>Failide haldamine lahendus
Lahenduste rakendatakse [Ressursihalduse](../resource-manager-template-walkthrough.md)malle.  Õppida, kuidas Autor lahenduste peamine ülesanne on õppida, kuidas [autor malli](../resource-group-authoring-templates.md).  Selles artiklis antakse kordumatu üksikasjad malle, mida kasutatakse lahenduste ja määratlemine tüüpilised lahenduse ressursid.

Halduse lahenduse faili põhistruktuur on sama, mis [Ressursihaldur Mall](resource-group-authoring-templates.md#template-format) , mis on järgmine.  Iga järgmistes jaotistes kirjeldatakse kõrgeima taseme elemendid ja ja nende sisu lahendus.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameetrid

[Parameetrite](../resource-group-authoring-templates.md#parameters) on väärtused, mis nõuavad kasutaja halduse lahenduse installimisel.  Kõik lahendused on standardne parameetrid ja saate lisada täiendavad parameetrid vastavalt vajadusele oma kindla lahenduse.  Kuidas kasutajad annab parameetrite väärtused, kui teie lahendus installimisel sõltub kindla parameetri ja kuidas lahendus on installitud.


Kui kasutaja installib teie lahendus [Azure'i turuplatsi](operations-management-suite-solutions.md#finding-and-installing-solutions) või [Azure Kiirjuhend Mallid](operations-management-suite-solutions.md#finding-and-installing-solutions) , palutakse valige [OMS tööruumi ja automatiseerimise konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account)kaudu.  Neid kasutatakse asustamiseks iga standard parameetrite väärtused.  Kasutaja on palutakse pakuvad otse standard parameetrite väärtused, kuid palutakse sisestada mis tahes täiendavaid parameetrite väärtused.

Kui kasutaja installib teie lahendus [mõne muu meetodi](operations-management-suite-solutions.md#finding-and-installing-solutions), määrama väärtuse standard kõik parameetrid ja kõik täiendavad parameetrid.

Valimi parameeter on näidatud allpool.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Järgmine tabel kirjeldab parameeter atribuute.

| Atribuut | Kirjeldus |
|:--|:--|
| tüüp        | Parameetri andmetüüp. Sõltub andmete sisestamise kontroll, kuvatakse kasutajale.<br><br>bool - rippmenüüst<br>string - tekstiväli<br>int - tekstiväli<br>securestring - parooli väli<br> |
| kategooria    | Valikuline parameeter kategooria.  Parameetrite sama kategooria on rühmitatud. |
| juhtelement     | Päringustringi parameetrite lisafunktsioone.<br><br>kuvatakse kuupäev ja kellaaeg – kuupäeva ja kellaaja juhtelement.<br>GUID - Guid väärtus luuakse automaatselt ja parameeter ei kuvata. |
| kirjeldus | Valikuline parameeter kirjeldus.  Kuvatakse teave kommentaarimulli parameetri kõrval. |


### <a name="standard-parameters"></a>Normaliseeritud parameetrid
Järgmises tabelis on loetletud kõik haldamise lahendusi normaliseeritud parameetrid.  Need väärtused täidetakse kasutaja asemel küsimine neid, kui teie lahendus on installitud Azure'i turuplatsi või Kiirjuhend mallid.  Kasutaja peab võimaldama väärtused neid, kui lahendus on installitud muul viisil.

>[AZURE.NOTE]Azure'i turuplatsi ja kiirjuhend Mallid kasutajaliidese ootab parameetrite nimed tabelis.  Kui kasutate muu parameetrite nimed siis kasutajal palutakse neid ja nad ei ole automaatselt luua.


| Parameetri | Tüüp | Kirjeldus |
|:--|:--|:--|
| account_name       | string |  Azure'i automaatika konto nimi. |
| pricingTier       | string | Hinnad taseme Log Analytics tööruumi ja Azure automatiseerimine konto. |
| regionId          | string | Piirkonna konto Azure automatiseerimine. |
| solutionName      | string | Lahenduse nimi. |
| workspaceName     | string | Logi Analytics tööruumi nimi. |
| workspaceRegionId | string | Piirkonna Log Analytics tööruumi. |





### <a name="sample"></a>Näidis
Järgmine on näide parameetri üksus lahenduse.  See hõlmab kõiki standard parameetrite ning kaks täiendavaid parameetreid sama kategooria.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Viitate parameetrite väärtused muude elementide lahendus süntaks **parameetrite (parameetri nimi)**.  Näiteks tööruumi nimega juurdepääsu tuleks kasutada **parameters('workspaceName')** 

## <a name="variables"></a>Muutujad

**Muutujate** elemendi sisaldab väärtusi, mida te kasutate ülejäänud haldamise lahendus.  Need väärtused ei satuks kasutaja installimist lahendus.  Need on mõeldud pakkuda autori ühes kohas, kus nad saavad hallata väärtusi, mida võib kasutada mitu korda kogu lahendus. Oma lahendus muutujad **ressursid** elementi need hardcoding erinevalt, tuleks paigutada kõik väärtused teatud.  See muudab koodi loetavamaks ja abil saab hõlpsasti muuta need väärtused uuemad versioonid.

Järgmine on näide **muutujate** elemendi kasutatud lahenduste tüüpilised parameetritega.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Viitate muutujaga lahenduse süntaks **muutujate (muutuja nimi)**kaudu.  Näiteks SolutionName muutuja juurdepääsu tuleks kasutada **variables('solutionName')** 


## <a name="resources"></a>Ressursid

**Ressursside** elemendi määratleb erinevaid ressursse, mis sisaldab teie lahendus.  See on suurim osa mall.  Ressursid on määratletud järgmise ülesehitusega.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Sõltuvused
**Ei leia dependsOn** elementide määrab [sõltuvus](../resource-group-define-dependencies.md) mõne muu ressursi.  Kui lahendus on installitud, ei looda ressursi seni, kuni kõik sõltuvustega loonud.  Näiteks teie lahendus võib, [alustage mõne käitusjuhendi](operations-management-suite-solutions-resources-automation.md#runbooks) kui see on installitud, [töö ressursi](operations-management-suite-solutions-resources-automation.md#automation-jobs)abil.  Töö ressursi oleks sõltuvad käitusjuhendi ressurss veenduge, et käitusjuhendi luuakse töö luuakse.

### <a name="oms-workspace-and-automation-account"></a>OMS tööruumi ja automatiseerimise konto
Lahenduste jaoks on vaja [OMS tööruumi](../log-analytics/log-analytics-manage-access.md) sisaldama vaadete ja [automatiseerimise konto](../automation/automation-security-overview.md#automation-account-overview) tegevusraamatud ja seotud ressursid.  Need on saadaval ressursside kohta lahendus on loodud ja peaks olema määratletud sama lahendust.  Kasutaja saab [määrata tööruumi ja konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) kui nad oma lahenduse juurutamine, kuid autorina võiksite kaaluda järgmist.


## <a name="solution-resource"></a>Lahendus ressurss
Iga lahenduse jaoks on vaja ressursside kirje **ressursse** elementi, mis määratleb sama lahendust.  See on teatud tüüpi **Microsoft.OperationsManagement/solutions**.  Järgmine on näide lahenduse ressursi.  Selle erinevate elementide on kirjeldatud allpool.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Lahenduse nimi
Lahenduse nimi peab olema kordumatu Azure tellimuse. Soovitatav kasutada väärtus on järgmine.  Seda kasutatakse **SolutionName** base nime ja **workspaceName** parameeter nimega muutujat veenduge, et nimi on kordumatu.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

See oleks lahendada, et umbes selline nimi.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Sõltuvused
Lahenduse ressursi olema iga muu ressursiga lahendus [sõltuvus](../resource-group-define-dependencies.md) , kuna need tuleb enne lahenduse loomist olemas.  Tehke seda, lisades iga ressursi kirjet **ei leia dependsOn** elementi.


### <a name="properties"></a>Atribuudid
Ressursi lahendus on atribuutide järgmises tabelis.  See hõlmab ressursside viidatud ja ohjata lahenduse, mis määratleb, kuidas hallatakse ressursi pärast lahendus on installitud.  Iga lahenduse ressurss olema loetletud **referencedResources** või **containedResources** atribuut.

| Atribuut | Kirjeldus |
|:--|:--|
| workspaceResourceId | OMS tööruumi vormi ID * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<tööruumi nimega\>*. |
| referencedResources   | Lahendus, mida ei tohiks eemaldada lahenduse eemaldamisel ressursside loend. |
| containedResources   | Loendi ressursside lahenduse, mis tuleks eemaldada, kui lahendus on eemaldatud. |

Ülaltoodud näites on lahendus on käitusjuhendi, ajakava ja vaade.  Ajakava ja käitusjuhendi on *viidatud* **Atribuudid** elementi, nii et need on eemaldatud, kui lahendus on eemaldatud.  Vaade on *esitatud* nii, et lahendus eemaldamisel eemaldanud.


### <a name="plan"></a>Kavandamine
Lahenduse ressursi **leping** ettevõttel on atribuutide järgmises tabelis. 

| Atribuut | Kirjeldus |
|:--|:--|
| Nimi          | Lahenduse nimi. |
| versioon       | Autori määratud lahenduse versiooni. |
| toote       | Lahendus tuvastamiseks kordumatu string. |
| Publisheri     | Avaldaja lahendus. |


## <a name="other-resources"></a>Muud ressursid
Saate avada üksikasjad ja ressursse, mis on levinud lahendusi järgmistes artiklites näited.

- [Vaated ja armatuurlauad](operations-management-suite-solutions-resources-views.md)
- [Automaatika ressursid](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Halduse lahenduse testimine
Enne oma halduse lahenduse juurutamine on soovitatav, kasutades [Test-AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell)testida.  See kinnitada oma lahenduse faili ja te ei leia probleeme enne, kui proovite kasutada seda abi.



## <a name="next-steps"></a>Järgmised sammud

- Teave [Loome Azure'i ressursihaldur](../resource-group-authoring-templates.md)malle.
- Otsingu [Azure'i Kiirjuhend mallide](https://azure.microsoft.com/documentation/templates) näidised eri ressursihaldur malle.
- [Vaadete halduse lahendusega](operations-management-suite-solutions-resources-views.md)lisamise üksikasju.
- Üksikasju lisamise [automatiseerimine ressursse haldamise lahendus](operations-management-suite-solutions-resources-automation.md).
 