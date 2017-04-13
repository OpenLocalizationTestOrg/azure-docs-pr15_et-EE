<properties
   pageTitle="Ressursihaldur toetatud teenuste | Microsoft Azure'i"
   description="Selle ressursi pakkujad, kes toetava ressursihaldur, nende skeemid ja saadaval API versioonid ja regioonid, mille saate majutada ressursside kirjeldatakse."
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
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Ressursihaldur pakkujate ning API versioonid ja skeemid

Azure'i ressursihaldur uue võimaldab teil juurutada ja hallata teenuseid, mis moodustavad rakenduste. Enamik, kuid mitte kõigis teenused toetavad ressursihaldur ja mõned teenused toetavad ressursihaldur ainult osaliselt. Selles teemas antakse toetatud ressursi pakkujate loendit Azure'i ressursihaldur.

Oma ressursse juurutamisel peate teadma piirkonnad toetavad nende ressursside ja milliste API versioonide jaoks on saadaval ressursside. Jaotis [toetatud regioonide](#supported-regions) näitab, kuidas teada saada, millist piirkondade tööd oma tellimust ja ressursse. Jaotis [toetatud API versioonide](#supported-api-versions) näitab, kuidas kindlaks teha, millised API versioone, saate kasutada.

Azure portaali ja klassikaline portaalis toetatud milliseid teenuseid, leiate [Azure'i portaali-saadavus diagrammi](https://azure.microsoft.com/features/azure-portal/availability/). Milliseid teenuseid jooksva tugiressursse, leiate artiklist [teisaldamine ressursid uue ressursirühma või tellimusele](resource-group-move-resources.md).

Järgmises tabelis on loetletud Microsofti teenuste tugi juurutamise ja haldamise kaudu ressursihaldur ja millised mitte. Linki veerus **Kiirjuhend Mallid** saadab päringu Azure'i Kiirjuhend mallide hoidla määratud ressursi pakkuja. Kiirjuhend Mallid on lisatud ja sageli värskendada. Kindla teenuse lingi olemasolu ei tähenda päring tagastab mallide hoidla. Olemas on ka paljud muude tootjate ressursi pakkujad, kes toetava ressursihaldur. Saate teada, kuidas näha ressursi pakkujad jaotises [Ressursi pakkujad ja tüübid](#resource-providers-and-types) .


## <a name="compute"></a>Arvuta

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------------------------ |-------- | ------ | ------ |
| Paketi   | Jah     | [Paketi ülejäänud](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015 / 12 / 01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Container | Jah | [ÜLEJÄÄNUD teenus](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics elutsükli teenused | Jah  |      |        |  |
| Skaala komplektid | Jah | [Ulatuse määramine ülejäänud](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Teenuse struktuuri | Jah  | [Teenuse struktuuri ülejäänud](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtuaalmasinates | Jah | [VM ÜLEJÄÄNUD](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuaalmasinates (klassikaline) | Piiratud | - | - | - |
| Kaugjuhtimise rakenduses | Ei      | -  | - | - |
| Pilveteenustega (klassikaline) | Piiratud (vt allpool) | - | - | - |

Virtuaalmasinates (klassikaline) viitab ressursse, mis on juurutanud läbi klassikaline juurutamise mudeli, mitte ressursihaldur juurutamise mudeli. Üldiselt ei toeta järgmisi ressursse ressursihaldur toimingud, kuid on mõned toimingud, mis on lubatud. Nende juurutamise kohta leiate lisateavet teemast [mõistmine ressursihaldur juurutus- ja klassikaline juurutamise](resource-manager-deployment-model.md). 

Pilveteenustega (klassikaline) saab kasutada koos muude klassikalises ressursside; siiski klassikalises ressursside ära kõik ressursihaldur funktsioonid ja ei ole hea valik tulevaste lahendusi. Selle asemel, võtke arvesse, et muuta oma rakenduse taristu kasutada Microsoft.Compute, Microsoft.Storage ja Microsoft.Network nimeruumid ressursse.


## <a name="networking"></a>Võrgunduse

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | -------  | -------- | ------ | ------ |
| Rakenduse lüüsi | Jah | [Rakenduse lüüsi ülejäänud](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS-I     | Jah | [DNS-I ÜLEJÄÄNUD](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Jah  | [ExpressRoute ülejäänud](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Laadi koormusetasakaalustusteenuse | Jah  | [Laadi koormusetasakaalustusteenuse ülejäänud](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Liikluse haldur | Jah | [ÜLEJÄÄNUD liikluse haldur](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuaalne võrkude | Jah| [Virtuaalse võrgu ülejäänud](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN-lüüsi | Jah | [Võrgu lüüsi ülejäänud](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[ühendused](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Salvestusruumi

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Salvestusruumi | Jah  | [Salvestusruumi ülejäänud](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Salvestusruumi konto](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Jah  |         |        |  |

## <a name="databases"></a>Andmebaasid

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Jah  | [DocumentDB ülejäänud](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis vahemälu | Jah |   | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-andmebaas | Jah | [SQL-andmebaasi ülejäänud](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-eelvaade](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL-i andmebaas | Jah |   |      |


## <a name="web--mobile"></a>Veebi- ja Mobile

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| API rakendused | Jah |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API rakendused](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API haldus | Jah  | [API halduse ülejäänud](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Sisu moderaator | Jah |   |   |   |
| Funktsioon rakendus | Jah |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Loogika rakendused | Jah   | [Töövoo teenuse REST API-ga](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobiilirakenduste | Jah |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobiilse kokkulepete | Jah  |  [Mobiilse kaasamine ülejäänud](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Otsing | Jah  | [Search REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web Apps | Jah |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Kasutusanalüüsi

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | -------  | -------- | ------ | ------ |
| Andmekataloogi | Jah  | [Andmekataloogi ülejäänud](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Andmete Factory | Jah | [Andmete Factory ülejäänud](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Andmeanalüüsi Lake | Jah |   |   [2015-10-01-eelvaade](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Andmesalve Lake | Jah  | [Lake andmesalve ülejäänud](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-eelvaade](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Jah | [HDInsights ülejäänud](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Seadme õpetused | Jah | [Õppekeskuse ülejäänud kohapeal](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016 05-01 eelvaade](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Voo Analytics | Jah  | [Steam Analytics ülejäänud](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Jah | [Power BI manustatud ülejäänud](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Ärianalüüsi

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| Kognitiivne teenused | Jah |  | [2016 02-01 eelvaade](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Asjade internet

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| Sündmuse jaoturi | Jah  | [Sündmuse jaoturi ülejäänud](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Jah | [Asjade jaoturi ülejäänud](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Teatis jaoturi | Jah | [Teate jaoturi ülejäänud](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Meediumid ja CDN-ID

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| CDN-ID | Jah | [CDN ÜLEJÄÄNUD](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Meediumi teenus | Jah | [Media Servicesi ülejäänud](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Hübriid integreerimine

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| BizTalki teenused | Jah |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Teenuse taastamine | Jah | [Saidi ülejäänud taastamine](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Teenuse siini | Jah | [Teenuse siini ülejäänud](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identiteedi ja juurdepääsu juhtimine 

Azure Active Directory töötab koos ressursihaldur lubamine Rollipõhine juurdepääsu reguleerimine tellimuse. Rollipõhine juurdepääsu reguleerimine ja Active Directory kasutamise kohta leiate teemast [Azure Rollipõhine juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Arendaja teenused 

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| Rakenduse ülevaated | Jah  | [Ülevaateid ülejäänud](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.Insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bingi kaardid | Jah  |          |        |  |
| DevTest Labs | Jah |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio konto | Jah   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Haldamine ja turvalisus

| Teenus | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------- | ------ | ------ |
| Automatiseerimine | Jah | [ÜLEJÄÄNUD automatiseerimine](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10 – 31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Võtme Vault. | Jah | [Võtme Vault ülejäänud](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Võtme vault](resource-manager-template-keyvault.md)<br />[Võtme vault salajane](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Töö ülevaated | Jah |          |        |  |
| Ajasti | Jah  | [Ajasti ülejäänud](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016-03-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Turvalisus (eelvaade) | Jah | [Turvalisus ülejäänud](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Ressursihaldur

| Funktsioon | Ressursihaldur lubatud | REST API-GA | Skeemi | Kiirjuhend Mallid |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Luba | Jah | [Lukkude haldamine](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Rollipõhine juurdepääsu reguleerimine](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Ressursi lukustamine](resource-manager-template-lock.md)<br />[Rollimäärangud](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Ressursid | Jah | [Seotud ressursid](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Ressursside lingid](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Ressursi pakkujad ja tüübid

Ressursside juurutamisel peate sageli teabe saamiseks selle ressursi pakkujad ja tuua. Selle teabe REST API-ga, Azure PowerShelli või Azure CLI kaudu saate alla laadida.

Ressursi pakkuja töötamiseks peate selle ressursi pakkuja registreerunud konto. Vaikimisi automaatselt registreeritud palju ressursi pakkujad; Siiski peate käsitsi registreerimiseks mõned ressursi pakkujad. Alltoodud näiteid näitab, kuidas hankida ressursi pakkuja registreerimise olekut ja ressursi pakkuja juures registreerima vajaduse korral.

### <a name="portal"></a>Portaal

Näete loendi toetatud ressursid pakkujate tehke järgmist:

1. Valige **minu õigused**.

    ![Minu õigused](./media/resource-manager-supported-services/my-permissions.png)

2. Valige **ressursi pakkuja olek**

    ![ressursi pakkuja olek](./media/resource-manager-supported-services/resource-provider-status.png)

3. Näete tellimuse ressursi pakkujad täieliku loendi.

    ![ressursi pakkujate loend](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST API-GA

Saada kõik kättesaadav ressursile pakkujad, sealhulgas nende tüüpi asukohti API versioonid ja olek, kasutage [loendi kõigi ressursside teenusepakkujate](https://msdn.microsoft.com/library/azure/dn790524.aspx) toiming. Kui teil on vaja registreerida ressursi pakkuja, leiate [registreerida tellimuse ressursi pakkuja juures](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShelli

Järgmises näites kujutatakse saada kõik saadaolevad ressursi pakkujad.

    Get-AzureRmResourceProvider -ListAvailable
    

Järgmises näites kujutatakse ressursi tüüpi saamiseks ressursile pakkuja.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Väljund on sarnane:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Sisestage ressursi pakkuja registreerimiseks nimeruumi.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure'i CLI

Järgmises näites kujutatakse saada kõik saadaolevad ressursi pakkujad.

    azure provider list
    
Väljund on sarnane:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Faili järgmise käsu abil saate salvestada ressursile pakkuja teave.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Sisestage ressursi pakkuja registreerimiseks nimeruumi.

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Toetatud Regioonide

Juurutamisel ressursid, peate tavaliselt määrata piirkonnas ressursside kohta. Ressursihaldur on kõigis piirkondades toetatud, kuid ressursside juurutamist ei pruugi olla toetatud kõikides piirkondades. Lisaks võib olla piiratud tellimuse, et takistada teatud piirkondadele, mis toetavad ressursi abil. Need piirangud võib olla seotud probleemid oma maksude või poliitika tulemus paigutatakse tellimuse administraatori kasutamine ainult teatud piirkondades. 

Kõigi toetatud regioonide kõigi Azure'i teenuste täieliku loendi leiate [teenuste regiooniti](https://azure.microsoft.com/regions/#services); siiski see loend võib sisaldada regioonid, mis ei toeta teie tellimus. Saate määrata piirkondade ressursile tüüp, portaali, REST API-ga, PowerShelli või Azure CLI toetava tellimuse jaoks.

### <a name="portal"></a>Portaal

Saate vaadata toetatud regioonide ressursi Type järgmiste toimingute abil:

1. Valige **rohkem teenuseid** > **Ressursi Explorer**.

    ![ressursi explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Avage sõlme **pakkujad** .

    ![Valige pakkuja](./media/resource-manager-supported-services/select-providers.png)

3. Valige ressursi pakkuja ja vaadata toetatud regioonide ja API versioonid.

    ![vaate pakkuja](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST API-GA

Leida piirkonnad on kättesaadav ressursile Type teie tellimus, kasutage [loendi kõigi ressursside teenusepakkujate](https://msdn.microsoft.com/library/azure/dn790524.aspx) toiming. 

### <a name="powershell"></a>PowerShelli

Järgmises näites on kujutatud veebisaitide jaoks toetatud regioonide hankimise kohta.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Väljund on sarnane:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure'i CLI

Järgmine näide tagastab kõigi toetatud asukohad iga ressursi tüüp.

    azure location list

Saate filtreerida ka JSON kasuliku nagu [jq](https://stedolan.github.io/jq/)asukoht tulemusi.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Mis annab:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Toetatud API versioonid

Kui juurutate malli, määrake mõni API versioon iga ressursi loomiseks kasutada. API versioon vastab versiooni REST API toimingud, mis on välja ressursi pakkuja. Kuna ressursi pakkuja võimaldab uusi funktsioone, selle välja REST API uus versioon. Seetõttu mõjutab teie määratud malli API versiooni millised atribuudid saate määrata malli. Üldiselt, mida soovite valida API kõige uuem versioon, kui mallide loomise. Olemasoleva Mallid, saate otsustada, kas soovite kasutada API varasema versiooni või värskendada oma malli uusima versiooni uued funktsioonid ära.

### <a name="portal"></a>Portaal

Toetatud API versiooni määratlemise samal viisil saate määrata toetatud Regioonide (varem näidatud).

### <a name="rest-api"></a>REST API-GA

Saate teada, millised API versioonid on saadaval ressursi tüübid, kasutage [loendi kõigi ressursside teenusepakkujate](https://msdn.microsoft.com/library/azure/dn790524.aspx) toimingut. 

### <a name="powershell"></a>PowerShelli

Järgmises näites on kujutatud hankimise saadaval API versioone ressursile tüüp.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Väljund on sarnane:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure'i CLI

Ressursi pakkuja faili järgmise käsu abil saate salvestada teave (sh saadaolevate API versioonid).

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Saate faili avada ja **apiVersions** elemendi otsimine

## <a name="next-steps"></a>Järgmised sammud

- Ressursihaldur mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md).
- Ressursse rakendades kohta leiate teemast [Deploy rakenduse Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).
