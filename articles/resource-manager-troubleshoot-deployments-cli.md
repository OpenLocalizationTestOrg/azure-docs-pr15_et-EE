<properties
   pageTitle="Juurutamise toimingute Azure'i CLI vaadata | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada Azure CLI probleemide ressursihaldur juurutamise kaudu."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Vaate juurutamise toimingute Azure'i CLI

> [AZURE.SELECTOR]
- [Portaal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShelli](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure'i CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-GA](resource-manager-troubleshoot-deployments-rest.md)

Kui olete saanud tõrke ressursid juurutamisel Azure, võite vaadata täpsemat teavet juurutamise toimingud, mis olid täidetud. Azure'i CLI pakub käsud, mis võimaldavad teil tõrgete otsimine ja määratleda võimalikud parandused.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Saate vältida tõrkeid oma malli ja taristu enne juurutamise kontrollimine. Samuti saate sisse logida täiendava ja vastuse teabe juurutamine, mis võib olla kasulik hiljem tõrkeotsingu käigus. Kontrollimine ja logiteabe koosolekukutsete ja kutsele vastamise kohta leiate teemast [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Kasutage auditilogid tõrkeotsing

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Tõrked juurutamine vaatamiseks tehke järgmist:

1. Auditilogide vaatamiseks **azure logi kuvamine** käsu käivitamine. Saate lisada soovitud **--viimase-juurutamise** suvand, et tuua ainult Logi viimase juurutamiseks.

        azure group log show ExampleGroup --last-deployment

2. **Azure'i Logi Kuva** käsk tagastab palju teavet. Tõrkeotsing, soovite tavaliselt toimingud, mida ei saanud keskenduda. Järgmise skripti kasutab suvandi **json -** ja [jq](https://stedolan.github.io/jq/) JSON kasuliku otsimiseks Logi juurutamise ebaõnnestumist.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Saate vaadata **Atribuudid** sisaldab teavet json kohta nurjunud toimingut.

    Saate kasutada funktsiooni **--Paljusõnaline** ja **- vv** suvandid, et kuvada rohkem teavet logid.  Kasutage funktsiooni **--Paljusõnaline** suvand kuvamiseks klõpsake läbida toimingute juhised `stdout`. Kogu ajalugu, kasutage suvandit **- vv** . Sõnumite pakuvad sageli väga oluline vihjeid selle kohta, mis tahes tõrkeid põhjustada.

3. Keskenduda olekuteade nurjunud kirjete, kasutage järgmist käsku:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Juurutamise toimingute abil saate otsida

1. Üldise oleku juurutuse **azure rühma juurutamise Kuva** käsk saada. Järgmises näites on nurjunud juurutamise.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Nurjunud toimingute jaoks juurutamine sõnumi vaatamiseks kasutada:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Järgmised sammud

- Kindla juurutamise tõrgete kõrvaldamise, lugege teemat [ressursid Azure'i Azure ressursihaldur juurutamisel levinud vigade lahendamine](resource-manager-common-deployment-errors.md).
- Muud tüüpi toimingute jälgimiseks auditilogide kasutamise kohta leiate teemast [auditi toimingud koos ressursihaldur](resource-group-audit.md).
- Kinnitage oma juurutuse enne seda, lugege teemat [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).
