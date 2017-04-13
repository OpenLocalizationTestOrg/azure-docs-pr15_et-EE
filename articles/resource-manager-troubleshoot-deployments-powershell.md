<properties
   pageTitle="Juurutamise toimingute PowerShelliga vaadata | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse probleemide ressursihaldur kasutuselevõtu Azure PowerShelli abil."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Vaate juurutamise toimingute Azure PowerShelli abil

> [AZURE.SELECTOR]
- [Portaal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShelli](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure'i CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-GA](resource-manager-troubleshoot-deployments-rest.md)

Saate vaadata tegevuse juurutamiseks, Azure PowerShelli kaudu. Teil võib olla kõige enam huvi kuvamist toimingud, kui olete saanud tõrke juurutamisel nii, et see artikkel keskendub vaatamine toimingute kohta, mida ei ole. On toodud PowerShelli cmdlet-käsud, mis võimaldavad teil leidmist vigade ja võimalike paranduste kindlaks teha.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Saate vältida tõrkeid oma malli ja taristu enne juurutamise kontrollimine. Samuti saate sisse logida täiendava ja vastuse teabe juurutamine, mis võib olla kasulik hiljem tõrkeotsingu käigus. Kontrollimine ja logiteabe koosolekukutsete ja kutsele vastamise kohta leiate teemast [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Juurutamise toimingute abil saate otsida

1. Üldise oleku juurutuse saamiseks kasutage käsku **Get-AzureRmResourceGroupDeployment** . Saate filtreerida ainult nurjus juurutuste tulemused.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Mis annab nurjunud juurutuste järgmises vormingus:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Tavaliselt tehakse iga juurutamise mitme toimingute ja iga toimingu tähistav etapi juurutamise käigus. Saate teada, mis läks valesti, juurutamine, peate tavaliselt juurutamise toimingute üksikasjade kuvamiseks. **Get-AzureRmResourceGroupDeploymentOperation**toimingute olekut saate vaadata.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Mis annab mitme toimingute iga üks järgmises vormingus:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Nurjunud toimingute kohta rohkem üksikasju, et tuua **nurjunud** olek toimingute atribuudid.

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Mis annab kõiki nurjunud toiminguid iga üks järgmises vormingus:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Pange tähele selle toimingu jälgimise ID-d. Kasutage mis järgmises etapis teatud keskenduda.

4. Kindla nurjunud toimingut olekuteade saamiseks kasutage järgmine käsk:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Mis annab:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Kasutage auditilogid tõrkeotsing

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Tõrked juurutamine vaatamiseks tehke järgmist:

1. Tuua Logi kirjed, käivitage käsk **Get-AzureRmLog** . **ResourceGroup** ja **olek** parameetrite abil saate tagastada ainult ühe ressursirühma nurjus sündmused. Kui te ei määra algus- ja kellaaja, tagastatakse kirjed viimase tunni.
Näiteks, et tuua nurjunud toimingute jaoks käivitage tunnis:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Saate määrata teatud kuuline ajavahemik. Järgmine näide kuvatakse vaatame nurjunud toiminguid viimase päeva. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Või saate seada täpse algus- ja lõppkellaaeg nurjunud toimingute jaoks.

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Kui käsk tagastab väärtuse liiga palju kirjed ja atribuudid, saate oma Auditeerimispoliitika püüete fookus toomine atribuudi **Atribuudid** . Lisame **DetailedOutput** parameetri tõrketeadete kuvamiseks.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Mis annab atribuudid logi kannete järgmises vormingus:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Nende tulemuste põhjal loome keskenduda Teine element. Saate kirje olekuteade vaadates tulemuste täpsustamiseks.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Mis annab:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Järgmised sammud

- Kindla juurutamise tõrgete kõrvaldamise, lugege teemat [ressursid Azure'i Azure ressursihaldur juurutamisel levinud vigade lahendamine](resource-manager-common-deployment-errors.md).
- Muud tüüpi toimingute jälgimiseks auditilogide kasutamise kohta leiate teemast [auditi toimingud koos ressursihaldur](resource-group-audit.md).
- Kinnitage oma juurutuse enne selle käivitamisel, lugege teemat [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).

