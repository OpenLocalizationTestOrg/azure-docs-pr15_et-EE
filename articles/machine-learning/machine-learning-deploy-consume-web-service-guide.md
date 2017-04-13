<properties
    pageTitle="Azure'i masina õ Web Services: Juurutus- ja tarbimine | Microsoft Azure'i"
    description="Ressursside juurutamine ja tarbimine veebiteenuste jaoks."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure'i masina õ Web Services: Juurutus- ja tarbimine

Azure'i masina Õppekeskuse abil saate juurutada masina-õppe töövood ja mudelite nimega veebiteenused. Järgmiste veebiteenuste saab kasutada siis helistada arvutisse-õppe mudelid rakendustest teha prognoose reaalajas või kogumitena Interneti kaudu. Veebiteenused on RESTful, saate neid erinevates keeltes ja platvormid, nt .NET ja Java, programmeerimise ja rakendused, nagu Exceli helistada.

Järgmistest jaotistest linke juhendavad tutvustused, koodi ja dokumendid, mis aitavad teil alustada.

## <a name="deploy-a-web-service"></a>Veebiteenuse juurutamine

### <a name="with-azure-machine-learning-studio"></a>Azure'i masina õ Studio

Seadme õ Studio ja Microsoft Azure'i masina õ veebiteenuste portaali aitavad teil juurutada ja hallata veebiteenuse koodi kirjutamata.

Järgmised lingid pakuvad üldist teavet selle kohta, kuidas juurutada uus veebiteenus:

* Uus web teenus, mis põhineb Azure'i ressursihaldur juurutamise kohta ülevaate saamiseks vt [Deploy uus veebiteenus](machine-learning-webservice-deploy-a-web-service.md).
* Lühiülevaade veebiteenuse juurutamise kohta leiate teemast [Deploy teenuse Azure seadme õ web](machine-learning-publish-a-machine-learning-web-service.md).
* Täielik lühiülevaade selle kohta, kuidas luua ja juurutada veebiteenuse leiate [kiirtutvustus samm 1: seadme õ tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md).
* Teatud edastamine veebiteenusele juurutamine näiteid, lugege teemat:

    * [Kiirtutvustus juhis 5: Juurutada Azure seadme õ veebiteenuse](machine-learning-walkthrough-5-publish-web-service.md)
    * [Juurutamise veebiteenuse mitme piirkonna](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web services ressursi pakkujaga API-de (Azure'i ressursihaldur API)

Azure'i masina õ ressursi pakkuja veebiteenuste jaoks võimaldab juurutus- ja veebiteenuste REST API kõned abil. Lisateabe saamiseks MSDN-is leiate viide [Arvutisse õ Web Service (muu)](https://msdn.microsoft.com/library/azure/mt767538.aspx) .

### <a name="with-powershell-cmdlets"></a>PowerShelli cmdlet-käskude abil

Azure'i masina õ ressursi pakkuja veebiteenuste jaoks võimaldab juurutus- ja veebiteenuste PowerShelli cmdlet-käskude abil.

Cmdlet-käskude kasutamiseks peate esmalt logite PowerShelli keskkonnas oma Azure'i konto [Lisamine – AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdleti abil. Kui olete varem selliseid PowerShelli käske, mis põhinevad ressursihaldur sisse helistada, lugege teemat [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Eksportida oma sõnastikupõhise katse, kasutage [selle proovi kood](https://github.com/ritwik20/AzureML-WebServices). Kui loote .exe faili koodi, saate tippida:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Rakendus töötab loob teenuse JSON veebimalli. Malli abil saate juurutada veebiteenuse, peate lisama järgmine teave:

* Salvestusruumikonto nimi ja võti

    Saate avada salvestusruumikonto nimi ja võti [Azure portaali](https://portal.azure.com/) või [Azure klassikaline portaalis](http://manage.windowsazure.com/).
* Kohustuse lepingu ID

    Saate lepingu ID portaalist [Azure seadme õ veebiteenuste](https://services.azureml.net) sisse logida ja klõpsates lepingu nimi.

Lisage need JSON malli laste *Atribuudid* sõlme *MachineLearningWorkspace* sõlm nimega samal tasemel.

Siin on näide:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Vt järgmisi artikleid ja proovi kood täiendavad üksikasjad:

* MSDN-i viide [Azure seadme õ cmdlet-käsud]( https://msdn.microsoft.com/library/azure/mt767952.aspx)
* Github valimi [kiirtutvustus](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt)

## <a name="consume-the-web-services"></a>Veebiteenuseid tarbib

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Azure'i masina õ veebiteenuse kasutajaliides (testimine)

Saate kontrollida oma veebiteenuse Azure seadme õ veebiteenuste portaalist. See hõlmab päringu vastuse teenuse (RRS) ja liidesed paketi täitmise teenuse (BES).

* [Uue veebiteenuse juurutamine](machine-learning-webservice-deploy-a-web-service.md)
* [Azure'i masina õ web teenuse juurutamine](machine-learning-publish-a-machine-learning-web-service.md)
* [Kiirtutvustus juhis 5: Juurutada Azure seadme õ veebiteenuse](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Excelist

Saate alla laadida Exceli mall, mis tarbib veebiteenuse:

* [Azure'i masina õ veebiteenuse, Excelist tarbimine](machine-learning-consuming-from-excel.md)
* [Exceli lisandmooduli Azure seadme õ veebiteenuste jaoks](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>ÜLEJÄÄNUD vastavalt kliendi

Azure'i masina õ veebiteenused on rahulik API-d. Saab kasutada nende API-de erinevad platvormid, nagu .NET, Python, R, Java jne. Oma [Microsoft Azure'i masina õ veebiteenuste portaali](https://services.azureml.net) veebiteenuse **tarbida** leht on proovi kood, mis aitavad teil alustada. Lisateavet leiate teemast [Azure seadme õ veebiteenus, mis on kasutusele võetud Õppekeskuse katse seadmes peab olema](machine-learning-consume-web-services.md).

