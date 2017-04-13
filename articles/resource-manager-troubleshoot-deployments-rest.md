<properties
   pageTitle="Juurutamise toimingute REST API-ga vaadata | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada Azure ressursihaldur REST API probleemide ressursihaldur juurutamise kaudu."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Vaate juurutamise toimingute Azure'i ressursihaldur REST API-ga

> [AZURE.SELECTOR]
- [Portaal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShelli](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure'i CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-GA](resource-manager-troubleshoot-deployments-rest.md)

Kui olete saanud tõrke ressursid juurutamisel Azure, võite vaadata täpsemat teavet juurutamise toimingud, mis olid täidetud. REST API pakub toimingud, mis võimaldavad teil tõrgete otsimine ja määratleda võimalikud parandused.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Saate vältida tõrkeid oma malli ja taristu enne juurutamise kontrollimine. Samuti saate sisse logida täiendava ja vastuse teabe juurutamine, mis võib olla kasulik hiljem tõrkeotsingu käigus. Kontrollimine ja logiteabe koosolekukutsete ja kutsele vastamise kohta leiate teemast [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Tõrkeotsing REST API-ga

1. Juurutada oma ressursse [loomine malli juurutamise](https://msdn.microsoft.com/library/azure/dn790564.aspx) toiminguga. Teave, mis võib olla kasulik silumine säilitamiseks atribuudi **debugSetting** JSON taotluse **requestContent** ja/või **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",      
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Vaikimisi **debugSetting** väärtuseks on seatud **pole**. **DebugSetting** väärtuse määramisel hoolega teabetüüp on läbides käigus juurutamist. Logimise kohta leiate lisateavet taotluse või vastuse, võib potentsiaalselt tundliku loomuga andmeid juurutamise toimingute kaudu nähtavaks tegemine. 

2. Saate teavet juurutamine toiminguga [malli juurutamise kohta teabe saamiseks](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Vastuse, Pange tähele, eriti **provisioningState** , **correlationId** ja **tõrge** elemendid. **CorrelationId** kasutatakse seotud sündmustel silma peal hoidmiseks ja võib olla kasulik tehnilise toe poole ja tõrkeotsingu töötamisel.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Saate teavet juurutamise toimingute toiminguga [loendis kõik malli juurutamise toimingud](https://msdn.microsoft.com/library/azure/dn790518.aspx) . 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Vastus sisaldab taotlus ja/või vastuse teabe põhjal, mis teie määratud atribuudi **debugSetting** käigus juurutamist.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Sündmuste toomine auditilogide toiminguga [loendi halduse sündmused tellimuse](https://msdn.microsoft.com/library/azure/dn931934.aspx) juurutamiseks.

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Järgmised sammud

- Kindla juurutamise tõrgete kõrvaldamise, lugege teemat [ressursid Azure'i Azure ressursihaldur juurutamisel levinud vigade lahendamine](resource-manager-common-deployment-errors.md).
- Muud tüüpi toimingute jälgimiseks auditilogide kasutamise kohta leiate teemast [auditi toimingud koos ressursihaldur](resource-group-audit.md).
- Kinnitage oma juurutuse enne selle käivitamisel, lugege teemat [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).
