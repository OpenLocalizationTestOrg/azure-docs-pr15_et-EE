<properties
   pageTitle="Sisemise koormuse koormusetasakaalustusteenuse, mis rakenduses ressursihaldur malli abil luua | Microsoft Azure'i"
   description="Saate teada, kuidas luua sisemise koormuse koormusetasakaalustusteenuse, mis rakenduses ressursihaldur malli abil"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Saate luua ka sisemise koormuse koormusetasakaalustusteenuse malli abil

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Malli abil klõpsake juurutamine

Proovi Mall saadaval avaliku hoidla kasutab parameetri faili, mis sisaldab vaikimisi stsenaarium, mis on kirjeldatud ülal loomiseks kasutatud väärtused. Selle malli abil klõpsake juurutada, [seda](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)linki juurutamiseks käsku **Deploy Azure**, asendada vaikimisi parameetrite väärtused vajadusel ja järgige juhiseid portaalis.

## <a name="deploy-the-template-by-using-powershell"></a>Malli PowerShelli abil

PowerShelli abil allalaaditud malli, järgige alltoodud juhiseid.

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../../articles/powershell-install-configure.md) ja järgige juhiseid kõik viis selleks Azure'i sisse logida ja valige oma tellimuse.
2. Parameetrite faili allalaadimiseks oma kohalikule kettale.
3. Faili redigeerimiseks ja salvestage see.
4. Käivitage **Uus-AzureRmResourceGroupDeployment** cmdlet-käsk loomine malli abil ressursirühma.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Malli abil Azure'i CLI

Malli abil Azure'i CLI, järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
2. Käsu **azure config režiimi** ressursihaldur lülitumine, nagu allpool näidatud.

        azure config mode arm

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    New mode is arm

3. Parameetri faili avada, valige selle sisu ja salvestage see oma arvutis olevasse faili. Selle näite puhul me salvestatud faili parameetrite *parameters.json*.

4. **Azure'i rühma juurutamise loomine** juurutada uue sisemise koormuse koormusetasakaalustusteenuse Mall ja parameetri failid saate alla laadida ja muuta kohal abil käsu käivitamine. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Järgmised sammud

[Laadi koormusetasakaalustusteenuse jaotuse režiimi kasutamine allika IP osaleja konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)



