<properties
    pageTitle="Juurutamine Õppekeskuse tööruumi Azure ressursihaldur malli abil arvutisse | Microsoft Azure'i"
    description="Juurutamise tööruumi Azure seadme õppe Azure'i ressursihaldur malli abil"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Õppekeskuse kasutamine Azure ressursihaldur tööruumi masina juurutamine

## <a name="introduction"></a>Sissejuhatus
Azure'i ressursihaldur juurutamise malli salvestab aega, võimaldades scalable võimalus abil juurutada omavahel seotud osa valideerimise abil ja proovige uuesti süsteem. Häälestada Azure seadme õ tööruumid, näiteks peate esmalt konfigureerida Azure storage konto ja seejärel juurutada tööruumis. Kujutage ette, teha seda käsitsi sadu tööruumid. Lihtsam alternatiiv on juurutada Azure seadme õ tööruumi ja kõik sõltuvustega Azure'i ressursihaldur malli kasutamiseks. Selles artiklis tutvustatakse teile selle protsessi samm-sammult. Azure'i ressursihaldur hea ülevaate leiate teemast [Azure ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Juhised: arvuti õ tööruumi loomine
Me Azure ressursi rühma loomine ja seejärel juurutada uue Azure storage konto ja uue Azure seadme õ tööruumi ressursihaldur malli abil. Kui juurutamise on lõpule jõudnud, on meil oluline teave tööruumid, mis on loodud (primaarvõtme, on workspaceID ja tööruumi URL-i) välja printida.

### <a name="create-an-azure-resource-manager-template"></a>Azure'i ressursihaldur malli loomine
Seadme õ tööruumi nõuab Azure storage konto sellega lingitud andmekomplekti talletamiseks.
Järgmised Mall kasutab ressursirühma luua salvestusruumi konto nimi nimi ja tööruumi nimi.  Samuti kasutab salvestusruumi konto nime atribuudi tööruumi loomisel.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Salvestage see mall mlworkspace.json jaotises c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Selle malli põhjal ressursirühma juurutamine
* Avatud PowerShelli
* Installige Azure'i ressursihaldur ja Azure Teenusehaldus moodulid  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Need juhised alla ja installige vajalikud täitke ülejäänud juhised moodulid. Seda tuleb teha kui keskkonnas, kus on käivitamisel PowerShelli käske.   

* Azure'i autentida  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Selles etapis tuleb iga seansi korrata. Pärast autentimist, peaks teie tellimuse teavet kuvada.

![Azure'i konto][1]

Nüüd kus oleme on juurdepääs Azure'i, saame luua ressursirühma.

* Ressursi rühma loomine

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Veenduge, et ressursirühma õigesti ette valmistatud. **ProvisioningState** peaks olema "õnnestus."
Ressursi rühma nimi on kasutab malli loomiseks salvestusruumikonto nimi. Salvestusruumikonto nimi peab olema vahel 3 ja 24 märki ja numbrite ja väiketähed tähtede ainult.

![Ressursirühm][2]

* Ressursi rühma juurutamise kasutamisel juurutada uue arvuti õ tööruumi.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Kui juurutamise on lõpule jõudnud, on lihtne saate juurutada tööruumi atribuudid. Näiteks pääsete esmane võti Turbeloa.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Teine võimalus tuua sõned olemasoleva tööruumi on Invoke-AzureRmResourceAction käsuga. Näiteks saate loendis kõik tööruumid, esmaseid ja teiseseid sõned.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Pärast tööruum on ette valmistatud, saate automatiseerida palju Azure seadme õ Studio ülesandeid [PowerShelli moodul Azure seadme õ](http://aka.ms/amlps)kasutamise.

## <a name="next-steps"></a>Järgmised sammud 
* Lugege lisateavet [Autorlusega Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md). 
* [Azure'i Kiirjuhend mallide hoidla](https://github.com/Azure/azure-quickstart-templates)tutvuda. 
* Vaadake seda videot [Azure'i ressursihaldur](https://channel9.msdn.com/Events/Ignite/2015/C9-39)kohta. 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
