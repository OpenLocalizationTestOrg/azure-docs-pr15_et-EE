<properties
    pageTitle="Mallide käsurea Azure'i virnas juurutamine | Microsoft Azure'i"
    description="Saate teada, kuidas on mitu platvormi käsurea liides (CLI) abil saate juurutada mallide sees on ClientVM või pärast Azure virnas ühendamine VPN-i abil."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Mallide käsureal Azure'i virnas juurutamine

Käsurea abil saate juurutada Azure virnas POC Azure'i ressursihaldur mallid. Azure'i ressursihaldur Mallid juurutada ja ettevalmistamise rakenduse ühe koordineeritud kasutusel kõik ressursid.

## <a name="download-template"></a>Malli allalaadimiseks        
Mõne juurutus, nii et CLI testimiseks alla laadida failide azuredeploy.json ja azuredeploy.parameters.json [salvestusruumi konto näide malli loomine](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Malli juurutamine
Liikuge kausta, kus on need failid alla ja käivitage järgmine käsk malli käsitleda:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

See käsk juurutamine ressursi rühma **cliRG** Azure virnas POC vaikeasukoha mall.

## <a name="validate-template-deployment"></a>Kinnitage malli juurutamine
Selle ressursi rühma ja salvestusruumi konto vaatamiseks saate kasutada järgmisi käske:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Järgmised sammud

[Kasutajaõiguste haldamine](azure-stack-manage-permissions.md)
