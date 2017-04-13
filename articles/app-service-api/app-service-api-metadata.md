<properties
    pageTitle="Rakenduse API rakendused metaandmete API discovery ja koodi genereerimine | Microsoft Azure'i"
    description="Teada, kuidas API rakendused teenuses Azure rakenduse abil ärplema metaandmete API discovery ja koodi genereerimine hõlbustada."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Rakenduse API rakendused metaandmete API discovery ja koodi genereerimine 

[Ärplema 2.0](http://swagger.io/) API metaandmete tugi on rakenduse API rakendused sisse ehitatud. Teil pole ärplema kasutama, kuid kui kasutate seda, saate ära API rakenduste funktsioone, mis on discovery ja tarbimine oleks lihtsam.   

## <a name="swagger-endpoint"></a>Ärplema lõpp-punkti

Saate määrata, mis pakub ärplema 2.0 JSON metaandmete atribuudi API rakenduse API rakenduse lõpp-punkti. Lõpp-punkti võib olla base URL-i API rakenduse või absoluutne URL. Absoluutne URL-ide osutate väljaspool API rakendus. 

Järgneval paljud kliendid (näiteks Visual Studio koodi loomine ja PowerApps meilivoo "Lisa API"), URL-i peab olema avalikult juurdepääsetava (mitte kaitstud kasutaja või autentimise). See tähendab, et kui kasutate rakendust Service autentimist ja soovite seada API määratluse kaudu oma rakenduses, ise, peate kasutama autentimine suvand, mis võimaldab anonüümse liikluse oma API saavutamiseks. Lisateavet leiate teemast [autentimise ja luba rakenduse teenuse API rakendused](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portaali blade

[Azure'i portaali](https://portal.azure.com/) URL-i lõpp-punkti näha ja muuta **API määratlus** enne.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure'i ressursihaldur atribuut

Samuti saate konfigureerida API määratlus URL-i rakendus API käsurea tööriistu, nt [Azure PowerShelli](../powershell-install-configure.md) ja [Azure CLI](../xplat-cli-install.md) [Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md) või [Ressursside Exploreri](https://resources.azure.com/) abil. 

**Ressursi Explorer**, minge **tellimused > {tellimuse} > resourceGroups > {ressursirühma} > pakkujate > Microsoft.Web > saidid > {saidi} > config > web**, ja te näete selle `apiDefinition` atribuudi:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Azure'i ressursihaldur Mall, mis määrab näitena selle `apiDefinition` atribuudi, avage [azuredeploy.json faili Ülesandeloend valimi rakenduses](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Leidke jaotis malli, mis näeb välja nagu eespool näidatud JSON valimi.

### <a name="default-value"></a>Vaikeväärtus

Kui Visual Studio abil saate luua rakenduse API, API määratlus lõpp-punkti väärtuseks automaatselt base URL-i API rakenduse pluss `/swagger/docs/v1`. See on vaike-URL, teenida dünaamiliselt loodud ärplema metaandmete ASP.net-i veebi-API projekti kasutava [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) Nugeti pakett. 

## <a name="code-generation"></a>Koodi loomine

Üks eeliseid, ärplema integreerimine Azure API rakendused on automaatne koodi loomine. Loodud kliendi tunnid oleks lihtsam kirjutada koodi, mis nõuab API rakendus.

Saate luua, kliendi API rakenduse Visual Studio abil või käsurea kaudu. Teavet selle kohta, kuidas luua kliendi tunnid ASP.net-i veebi-API projekti Visual Studios, lugege teemat [API rakendused ja ASP.net-i kasutamise alustamine](app-service-api-dotnet-get-started.md#codegen). Lisateavet selle kohta, kuidas seda teha kõigi toetatud keelte käsurea kaudu näidata GitHub.com [Azure/autorest](https://github.com/azure/autorest) hoidla readme-faili.
 
## <a name="next-steps"></a>Järgmised sammud

Üksikasjaliku juhendi, mis juhatab teid läbi loomine, juurutamine ja tarbimine API rakenduse vt [API Appsi teenuses Azure rakenduse kasutamise alustamine](app-service-api-dotnet-get-started.md).

Kui kasutate Azure API Management API rakendustega, saate importida oma API API halduse ärplema metaandmete. Lisateabe saamiseks vaadake, [Kuidas importida toimingutega Azure'i API Management API määratlus](../api-management/api-management-howto-import-api.md). 
