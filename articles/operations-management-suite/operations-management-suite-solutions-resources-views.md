<properties
   pageTitle="Vaadete toimingud halduse komplekti (OMS) lahenduste | Microsoft Azure'i"
   description="Lahenduste toimingud halduse komplekti (OMS) on üldjuhul ühe või mitme vaadete andmete visualiseerimiseks.  Selles artiklis kirjeldatakse, kuidas eksportida vaate kuvamine disainer ja kaasamine haldamise lahendus. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Vaadete lahenduste toimingud halduse komplekti (OMS) (eelvaade)

>[AZURE.NOTE]See on esialgse dokumentatsioonist loomine lahenduste OMS, mis on praegu eelvaade. Mis tahes allpool kirjeldatud skeemi on muutuda.    

[Lahenduste toimingud halduse komplekti (OMS)](operations-management-suite-solutions.md) on üldjuhul ühe või mitme vaadete andmete visualiseerimiseks.  Selles artiklis kirjeldatakse, kuidas eksportida vaate loodud [Kuvakujundaja](../log-analytics/log-analytics-view-designer.md) ja kaasamine haldamise lahendus.  

>[AZURE.NOTE]Selle artikli näidised kasutada parameetrite ja muutujate, mis on nõutav või levinud lahendusi ja kirjeldatud [loomine lahenduste toimingud halduse komplekti (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et olete juba tuttav kuidas [luua halduse lahenduse](operations-management-suite-solutions-creating.md) ja struktuuri lahenduse faili.


## <a name="overview"></a>Ülevaade

Vaate kaasata halduse lahenduse, saate luua **Ressursi** seda [lahenduse faili](operations-management-suite-solutions-creating.md).  Vaate üksikasjaliku konfiguratsiooni kirjeldav JSON on tavaliselt keerukate küll ja mitte midagi, et tüüpilised lahenduse Autor oleks võimalik luua käsitsi.  Enamlevinud on [Kuvakujundaja](../log-analytics/log-analytics-view-designer.md)vaate loomine, eksportida ja seejärel lisage selle üksikasjalik konfiguratsioon lahendus. 

Vaate lisamine lahenduseks Põhitoimingud on järgmised.  Iga toimingu üksikasjalikumalt järgmistes jaotistes kirjeldatakse.

1. Vaate eksportimine faili.
2. Lahendus ressursi vaate loomine
3. Lisage Kuva üksikasjad.

## <a name="export-the-view-to-a-file"></a>Vaate eksportimine faili
Järgige veebisaidil [Log Analytics Kuvakujundaja](../log-analytics/log-analytics-view-designer.md) vaate eksportimine faili.  Eksporditud fail saab sama [elementide failina lahenduse](operations-management-suite-solutions-creating.md#management-solution-files)JSON-vormingus.  

**Ressursid** elemendi vaade faili on teatud tüüpi **Microsoft.OperationalInsights/workspaces** , mis tähistab OMS tööruumi ressurss.  – See element on alamelement tüüpi **vaateid** , mis tähistab vaate ja see sisaldab üksikasjalikku konfiguratsioon.  Ei – see element üksikasjad kopeerida ja seejärel kopeerige see teie lahendus.


## <a name="create-the-view-resource-in-the-solution"></a>Lahendus ressursi vaate loomine
**Ressursside** elemendi lahenduse faili järgmisest ressursist vaate lisada.  Seda kasutatakse muutujate, mida on kirjeldatud allpool, et peate ka lisama.  Pange tähele, et atribuudid **armatuurlaua** ja **OverviewTile** koos vastavate atribuutide failist eksporditud vaade kirjutate kohatäited.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Lisage järgmiste muutujate [muutujate](operations-management-suite-solutions-creating.md#variables) elementi lahendus faili ja asendada need oma lahenduse väärtused.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Pange tähele, et kogu vaate ressursi võib kopeerige eksporditud vaade faili, kuid peate seda, et teie lahendus töötavad järgmised muudatused.  

- Ressursi vaate **Tüüp** on vaja muuta **Microsoft.OperationalInsights/workspaces** **vaated** .
- Kuva ressursi atribuudi **nimi** on vaja muuta lisada tööruumi nimi.
- Tööruumi sõltuvus tuleb eemaldada, kuna tööruumi ressursi pole määratletud lahendus.
- Atribuut **DisplayName** on vaja lisada vaatesse.  **Id**, **nimi**ja **DisplayName** peab vastama kõik.
- Parameetrite nimed tuleb muuta vastavaks parameetrite nõutav kogum.
- Muutujate tuleb määratleda lahendus ja kasutada sobivad atribuudid.

## <a name="add-the-view-details"></a>Lisage üksikasjade kuvamine
Vaate ressursside vaade eksporditud fail sisaldab kahte elemendid nimega **armatuurlaua** ja **OverviewTile** **Atribuudid** elementi, mis sisaldavad üksikasjalikku konfiguratsiooni vaate.  Kopeerige need kaks elementi ja nende sisu **Atribuudid** elemendi vaade ressursi lahenduse failis. 

## <a name="example"></a>Näide
Järgmises näites kuvatakse näiteks lihtsa lahenduse faili vaade.  Kolmikpunkti (...) on näidatud **armatuurlaua** ja **OverviewTile** sisu ruumi põhjustel.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kõigi üksikasjade loomisega [haldamise lahendusi](operations-management-suite-solutions-creating.md).
- [Automaatika tegevusraamatud teie lahendus on](operations-management-suite-solutions-resources-automation.md)lisada.