<properties
   pageTitle="Ressursside REST API-ga ja malli juurutamine | Microsoft Azure'i"
   description="Kasutage Azure'i ressursihaldur ja ressursihaldur REST API on ressursse Azure. Ressursside on määratletud ressursihaldur malli."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Ressursside ressursihaldur mallide ja ressursihaldur REST API juurutamine

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-template-deploy.md)
- [Azure'i CLI](resource-group-template-deploy-cli.md)
- [Portaal](resource-group-template-deploy-portal.md)
- [REST API-GA](resource-group-template-deploy-rest.md)

Selles artiklis selgitatakse, kuidas kasutada ressursihaldur REST API ressursihaldur malle juurutada Azure oma ressursse.  

> [AZURE.TIP] Tõrke silumine juurutamisel abi järgmistest teemadest.
>
> - [Vaate juurutamise toimingute REST API-ga](resource-manager-troubleshoot-deployments-rest.md) kohta teavet, mis aitab teil saada tõrkeotsing oma tõrge
> - [Tõrkeotsing levinud vigade ressursid Azure'i Azure ressursihaldur juurutamisel](resource-manager-common-deployment-errors.md) juurutuse levinud vigade lahendamise kohta teavet

Malli võib olla, kas kohalik fail või välise faili, mis on saadaval URI kaudu. Kui malli asub salvestusruumi konto, saate piirata juurdepääsu Mall ja pakuvad juurutamisel märgiks ühiskasutusega juurdepääsu allkirja (SAS).

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>REST API-ga juurutamine
1. Määrake [levinud parameetrite ja päised](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), sh autentimine märkide.
2. Kui teil pole olemasoleva ressursi rühma, luua ressursirühma. Sisestage oma tellimuse id, nimi uue ressursirühma ja asukohta, et peate oma lahenduse. Lisateavet leiate teemast [loomine ressursirühma](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Kinnitage juurutamise enne selle käivitades [Valideeri malli juurutamine](https://msdn.microsoft.com/library/azure/dn790547.aspx) toiming. Juurutamise testimisel anda parameetrid täpselt samuti nagu teeksite täitmisel juurutamise (näidatud järgmise juhise juurde).

3. Looge juurutamine. Sisestage oma tellimuse id, ressursirühma juurutada, nimi ja juurutamise lingi oma malli nimi. Mallifail kohta leiate teavet teemast [parameetri fail](#parameter-file). REST API ressursirühma loomise kohta leiate lisateavet teemast [loomine malli juurutamine](https://msdn.microsoft.com/library/azure/dn790564.aspx). Pange tähele, et **režiim** on seatud **Inkrementmuutus**. Täieliku juurutamise käivitamiseks seatud **režiimi** **lõpuleviimine**. Olge ettevaatlik, nagu saate kustutada tahtmatult ressursse, mis pole teie malli täieliku režiimi kasutades.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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
              }
            }
          }
   
      Kui soovite vastuse sisu sisse logida, taotluse sisu või mõlemad, lisage **debugSetting** kutse.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Saate häälestada kontole salvestusruumi kasutada märgiks ühiskasutusega juurdepääsu allkirja (SAS). Lisateabe saamiseks vt teemat [delegeerivad ühiskasutusse Accessi allkirjastatud](https://msdn.microsoft.com/library/ee395415.aspx).

4. Saada malli juurutamise olekut. Lisateavet leiate teemast [malli juurutamise kohta teabe saamiseks](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Järgmised sammud
- Näiteks juurutamine .net-i kliendi teek abil, vt [Deploy ressursid .NET teekide ja malli abil](virtual-machines/virtual-machines-windows-csharp-template.md).
- Parameetrite määratlemine malli, lugege teemat [funktsiooniga Mallid](resource-group-authoring-templates.md#parameters).
- Oma lahenduse juurutamine viibite juhised leiate teemast [arendamine ja testi keskkonnas Microsoft Azure](solution-dev-test-environments.md).
- Turvaline väärtused läbida viite KeyVault kasutamise kohta lisateavet [turvaline väärtuste juurutamisel edasi](resource-manager-keyvault-parameter.md).
