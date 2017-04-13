<properties
    pageTitle="Azure'i ressursihaldur vastavalt platvormidel käsurea tööriistade Azure Web Appi | Microsoft Azure'i"
    description="Saate teada, kuidas hallata oma Azure'i veebirakenduste uue Azure ressursihaldur vastavalt platvormidel käsurea tööriistade abil."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Azure'i ressursi haldur vastavalt XPlat CLI kasutamise Azure Web App#

> [AZURE.SELECTOR]
- [Azure'i CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure'i PowerShelli](app-service-web-app-azure-resource-manager-powershell.md)

Microsoft Azure'i platvormidel käsurea tööriistad versiooni 0.10.5 väljaandesse on lisatud uus käske. Need käsud anda kasutajale võimalus veebirakenduste haldamine Azure'i ressursihaldur vastavalt PowerShelli käskude abil.

Ressursi rühmade haldamise kohta leiate teemast [Azure CLI Azure ressursid ja ressursside rühmade haldamiseks kasutada](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Rakenduse teenuse haldamise lepingud ##

### <a name="create-an-app-service-plan"></a>Mõni rakendus teenusleping loomine ###
Mõni rakendus teenusleping loomiseks käsu **Azure'i appserviceplan loomine** .

Eri parameetrite kirjeldused on järgmised:

-   **--ressursirühm**: ressursirühm, mis sisaldab äsja loodud rakenduse teenusleping.
-   **--nimi**: teenusleping rakenduse nimi.
-   **--asukoht**: rakenduse lepingu asukoht.
-   **--taseme**: soovitud hinnakirjad sku (Valikud on järgmised: F1 (tasuta). D1 (ühiskasutuses). B1 (tavaline väike), B2 (elementaarne meediumi), ja B3 (Basic suur). S1 (Standard väike), S2 (Standard Keskmine) ja S3 (Standard suur). P1 (Premium väike), P2 (Premium Keskmine) ja P3 (Premium suur).)
-   **--eksemplari**: töötajate rakenduse teenuse lepingu (vaikeväärtus on 1).

Näide kasutada seda cmdlet-käsk:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Loendi olemasoleva rakenduse teenuse lepingud ###

Loendi olemasoleva rakenduse teenuse lepingud, kasutage käsku **Azure'i appserviceplan loend** .

Loetle kõik appi lepingud on jaotises kindla ressursirühma, saate kasutada.

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Mõne kindla rakenduse teenusleping saamiseks kasutage **Azure'i appserviceplan Kuva** käsku:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Mõne olemasoleva rakenduse teenuse leping konfigureerimine ###

Olemasoleva rakenduse teenusleping sätete muutmiseks käsu **Azure'i appserviceplan config** . Saate muuta kasutatava sku ja töötajate arvu 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Mõni rakendus teenusleping mastaapimine ####

Mastaapimiseks mõne olemasoleva rakenduse teenuse leping, saate kasutada.

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Rakenduse teenuse plaani SKU muutmine ####

Saate muuta mõne olemasoleva rakenduse teenuse kava sku, abil.

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Kustutada mõne olemasoleva rakenduse teenuse kavandamine ###

Olemasoleva rakenduse teenusleping kustutamiseks tuleb kõik määratud veebirakenduste teisaldatud või kustutatud esmalt. Seejärel saate kustutada rakenduse teenusleping **azure Web Appis kustutamine** käsu abil.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Rakenduse teenuse veebirakenduste haldamine ##

### <a name="create-a-web-app"></a>Veebirakenduse loomine ###

Veebirakenduse loomiseks käsu **azure Web Appis loomine** .

Eri parameetrite kirjeldused on järgmised:

- **--nimi**: web appi nime.
- **--lepingut**: nime jaoks kasutatud majutada veebirakenduse teenusleping.
- **--ressursirühm**: ressursirühm, mis hostib rakenduse teenusleping.
- **--asukoht**: web appi kohta.

Näide kasutada seda cmdlet-käsk:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Kustutage olemasolev Web App ###

Kustutage olemasolev web app saate kasutada **azure Web Appis kustutamine** käsu, peate määrama veebirakenduse nimi ja ressursside rühma nime.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Loendi olemasoleva Web Apps ###

Olemasoleva veebirakenduste loendis käsu **azure Web Appis loend** .

Loetle kõik veebirakenduste jaotises kindla ressursirühma, saate kasutada.

    azure webapp list --resource-group ContosoAzureResourceGroup

Kindla veebirakenduse saamiseks käsu **Kuva azure Web Appis** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Mõne olemasoleva Web Appi konfigureerimine ###

Saate muuta sätteid ja konfiguratsioone olemasoleva veebirakenduse käsu **set azure Web Appis config** .

Näide (1): muuta php web appi versioon 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Näide (2): lisamine või rakenduse sätete muutmine

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Teada, mis võib olla muude konfiguratsiooni muutnud, kasutage käsu **azure Web Appis config set -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Muuta olemasolevaid Web App ###

#### <a name="restart-a-web-app"></a>Taaskäivitage web app ####

Taaskäivitage web appi, peate määrama veebirakenduse rühma nimi ja ressursside.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Web appi peatamine ####

Web appi peatamiseks Määrake veebirakenduse rühma nimi ja ressursside.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Käivitage web app ####

Web appi käivitamiseks peate määrama rühma nimi ja ressursside web app.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web Appi avaldamise profiilide haldamine ###

Iga web appil avaldamise profiili, mida saab avaldada oma rakendused.

#### <a name="get-publishing-profile"></a>Saada avaldamise profiil ####

Web appi avaldamise profiili saamiseks kasutage:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

See käsk kordab avaldamise profiili kasutajanimi ja parool käsureale.

### <a name="manage-web-app-hostnames"></a>Veebirakenduse hostinimed haldamine ###

Hostname sidumiste veebirakenduse jaoks juhtida käsuga **azure Web Appis config hostinimed**  

#### <a name="list-hostname-bindings"></a>Loendi hostname seosed ####

Praeguse hostname sidumiste web appi saamiseks kasutage:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hostname (hostinimi) sidumiste lisamine ####

Hostname (hostinimi) sidumiste lisamiseks web appi kasutamine

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname (hostinimi) sidumiste kustutamine ####

Hostname (hostinimi) sidumiste kustutamiseks kasutada.

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Järgmised sammud ###
- Azure'i ressursihaldur CLI toe kohta leiate teemast [Azure CLI abil saate hallata Azure materjale ja ressursirühma.](../xplat-cli-azure-resource-manager.md)
- Rakenduse teenuse PowerShelli kaudu haldamise kohta leiate teemast [Azure veebirakenduste haldamine Using Azure Resource Manager-Based PowerShelli.](app-service-web-app-azure-resource-manager-powershell.md)
