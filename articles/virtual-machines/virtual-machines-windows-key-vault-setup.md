<properties
    pageTitle="Klahv Vault virtuaalmasinates Azure'i ressursihaldur häälestamine | Microsoft Azure'i"
    description="Kuidas häälestada klahvi Vault on Azure ressursihaldur virtuaalse masina kasutamiseks."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Klahv Vault virtuaalmasinates Azure'i ressursihaldur häälestamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudeli

Azure'i ressursihaldur virnas, saladused/serdid on modelleerida nagu ressursse, mis on esitatud klahvi Vault ressursi pakkuja. Klahv Vault kohta leiate lisateavet teemast [mis on Azure klahvi Vault?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Selleks klahvi Vault koos ressursihaldur Azure'i virtuaalmasinates, võti Vault atribuudi **EnabledForDeployment** peab olema seatud väärtusele tõene. Saate seda teha erinevate klientides.
>
>2. Klahv Vault tuleb luua samas tellimus ja virtuaalse masina asukohaks.

## <a name="use-powershell-to-set-up-key-vault"></a>PowerShelli kasutamine klahvi Vault häälestamine
PowerShelli abil võtme vault loomiseks vaadake teemat [Alustamine Azure'i klahvi Vault](../key-vault/key-vault-get-started.md#vault).

Uue tootenumbri võlvid, saate selle PowerShelli cmdlet-käsk:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Olemasoleva võtme võlvid, saate selle PowerShelli cmdlet-käsk:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Kontaktteabe CLI klahvi Vault häälestamine
Võtme vault, kasutades käsurea liides (CLI) loomiseks vaadake teemat [haldamine klahvi Vault abil CLI](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

CLI, tuleb luua võtme vault enne juurutamist poliitika määramine. Järgmise käsu abil saate seda teha.

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Mallide abil saate seadistada klahv Vault
Kui kasutate malli, peate määrama selle `enabledForDeployment` atribuut `true` klahvi Vault ressursi.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Muude suvandite võtme vault loomisel mallide abil saate konfigureerida, leiate teemast [loomine võtme vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
