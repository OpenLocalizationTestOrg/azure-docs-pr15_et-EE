<properties
    pageTitle="Hostname (hostinimi) ja SSL-i serdi MSDeploy abil web minirakenduse juurutamine"
    description="Azure'i ressursihaldur malli kasutamiseks juurutamiseks web Appi abil MSDeploy ja kohandatud hostname ja SSL-serdi loomine"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Web appi MSDeploy, kohandatud hostname ja SSL-serdi juurutamine

Sellest juhendist tutvustatakse Azure Web App on-lõpuni juurutamise loomise, kasutamine MSDeploy kui ka kohandatud hostname ja SSL-serdi lisamine ARM mall.

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur mallide koostamine](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Valimi rakenduse loomine

Saate juurutamine ASP.net-i web rakenduse. Esimese asjana lihtsa veebirakenduse loomiseks (või võite määrata, et kasutada mõne olemasoleva – sel juhul võite selle sammu vahele jätta).

Avage Visual Studio 2015 ja valige fail > uus projekt. Valige kuvatavas dialoogiboksis Web > ASP.net-i veebirakenduse. Valige Web jaotises Mallid ja valige mall MVC. Valige _Muuda autentimistüüp_ _Ei_autentimist. See on vaid teha valimi rakenduse võimalikult lihtne.

Selles etapis on teil lihtne ASP.net-i web appi oma juurutamise käigus kasutamiseks valmis.

###<a name="create-msdeploy-package"></a>MSDeploy paketi loomine

Järgmise sammuna tuleb juurutada veebirakenduse Azure'i paketi loomine. Selle tegemiseks oma projekti salvestada ja seejärel käivitage järgmine käsurea kaudu:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

See loob ZIP paketi PackageLocation kaustas. Rakendus on nüüd valmis kasutusele võtta, mille saate nüüd välja mõni Azure ressursihaldur Mall selleks, et koostada.

###<a name="create-arm-template"></a>ARM malli loomine
Esmalt Alustame ARM põhimalli, mis loob veebirakenduse ja majutusteenuse leping (Pange tähele, et parameetrite ja muutujate ei kuvata lühike jaoks).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Järgmiseks peate muuta web appi ressurss pesastatud MSDeploy ressursi tegemiseks. See võimaldab teil viide paketi varem loodud ja kindlaks Azure'i ressursihaldur MSDeploy abil saate juurutada paketi Azure Web Appis. Järgnevalt kuvatakse pesastatud MSDeploy ressursi Microsoft.Web/sites ressursi.

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Nüüd te teate, et MSDeploy ressursi võtab **packageUri** atribuut, mis on määratletud järgmiselt:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

See **packageUri** võtab salvestusruumi konto uri, mis viitab salvestusruumi konto, kui saadate oma paketi zip. Azure'i ressursihaldur kogusummaks rippmenüü paketi kohalikult kontolt salvestusruumi malli juurutamisel [Ühiskasutusse Accessi allkirjad](../storage/storage-dotnet-shared-access-signature-part-1.md) . Selle protsessi automatiseeritud lisamine PowerShelli skripti, et paketi üleslaadimine ja kõne Azure'i haldamine API võtmed loomiseks vajalik ja anda need malli parameetrid (*_artifactsLocation* ja *_artifactsLocationSasToken*) kaudu. Te peate saate määratleda kaust ja failinimi paketi laaditakse jaotises salvestusruumi-ümbrisest.

Järgmiseks peate lisama teise pesastatud koodiressursi hostname sidumiste ära kasutada kohandatud domeeni häälestamine. Peate esmalt veenduge, et omanik hostname ja kinnitamata Azure'i omandiõigust-häälestamine teemast [kohandatud domeeninime Azure'i rakendust Service konfigureerimine](web-sites-custom-domain-name.md). Pärast seda saate lisada malli jaotises Microsoft.Web/sites ressursi järgmist:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Lõpetuseks tuleb kõigepealt lisada teise ülemise taseme ressursside Microsoft.Web/certificates. Selle ressursi sisaldab SSL-serdi ja on olemas oma veebirakenduse ja hosting kava samal tasemel.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Peate olema lubatud SSL-serdi seadistamiseks ressurss. Kui teil on kehtiv, et peate ekstraktida stringina base64 pfx baiti. Eraldada selle üheks võimaluseks on kasutada PowerShelli käsk:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Teil võib seejärel edastada see parameetrina ARM juurutamise malli.

Selles etapis ARM mall on valmis.

###<a name="deploy-template"></a>Malli juurutamine

Lõplik juhised kehtivad see kõik kokku panema rakendusse täielik end-to-end. Hõlbustamiseks juurutamise **Deploy-AzureResourceGroup.ps1** PowerShelli skripti, on lisatud Visual Studio hõlbustamiseks üles mõni nõutav malli esemeid Azure'i ressursi rühmaprojekti loomisel saate kasutada. See nõuab teilt loonud salvestusruumi konto, mida soovite kasutada ette valmistada. Selle näite puhul loodud ühine konto jaoks package.zip üles laadida. Skripti kogusummaks AzCopy paketi üleslaadimine salvestusruumi kontole. Saate edastada oma artefakt kaust ja skripti automaatselt üles laadida kõik failid sellesse kataloogi nimega salvestusruumi ümbrises. Pärast kutsudes Deploy-AzureResourceGroup.ps1 tuleb seejärel värskendage SSL-i sidumiste vastendamiseks kohandatud hostname koos SSL-sert.

Järgmine PowerShelli kuvatakse täielik juurutamise helistamiseks Deploy-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Selles etapis rakenduse peaks olema kasutusele võetud ja peaks oskama sirvige selleni https://www.yourcustomdomain.com kaudu
