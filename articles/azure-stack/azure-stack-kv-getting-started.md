<properties
    pageTitle="Alustamine: Azure'i virnas võtme Vault | Microsoft Azure'i"
    description="Azure'i virnas klahvi Vault kasutamise alustamine"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Klahv Vault töötamise alustamine

Selles jaotises kirjeldatakse toiminguid, kuidas luua võlvkelder, võtmed ja saladusi haldamine, samuti lubada kasutajate või rakenduste ühendamine autonoomsest võlvkelder Azure'i virnas toimingud. Järgmiste juhiste korral eeldatakse rentniku tellimus on olemas ja KeyVault teenus on registreeritud tellimuse sees. Näide käsud põhinevad KeyVault cmdlet-käsud saadaval Azure PowerShelli SDK osana.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Rentniku tellimuse Vault toimingute lubamine 

Enne kui saate väljastada vastu mis tahes vault, peate veenduge, et teie tellimus on lubatud vault toimingute. Saate kinnitada, et väljastanud PowerShelli käsk:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Ülaltoodud käsu väljund aruande "Registreeritud", "Registreerimine" oleku iga rea jaoks.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Kui see ei ole, tuleks kutsute registreerida KeyVault teenuse tellimuse järgmine käsk:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

Ja järgmine tekst on käsk:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Kui kuvatakse tõrketeade: "*tellimuse see pole registreeritud teenuses Azure klahvi Vault*" rakendatakse KeyVault cmdlet-käskude Palun kinnitage, et teil on lubatud KeyVault ressursi pakkuja kohta ülaltoodud juhiseid.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Azure'i virnas talletamiseks ja haldamiseks cryptographic võtmed ja saladused kõva container (võlvkelder) loomine

Võlvkelder loomiseks rentniku peaksite kõigepealt looma ressursirühma. Järgmine PowerShelli käske luua ressursirühma ja seejärel võlvkelder selle ressursirühma. Näiteks sisaldab ka selle cmdlet-käsk tüüpilised väljund.

### <a name="creating-a-resource-group"></a>Ressursirühma loomine:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Väljund:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Võlvkelder loomine:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Väljund:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Selle cmdlet-käsu väljund kuvatakse soovitud klahv hoidla, mis äsja loodud atribuudid. On kaks kõige olulisemad atribuudid.

-   **Vault nimi**: näites on **vault010**. Kasutate selle klahvi Vault cmdletid nimi.

-   **Vault URI**: näites on https://vault010.vault.azurestack.local. Rakendustes, mis kasutavad teie vault kaudu oma REST API peate kasutama selle URI.

Azure'i konto on nüüd volitatud toiminguid selle võtme vault. Veel on keegi teine.


## <a name="operating-on-keys-and-secrets"></a>Kiirklahvid ja saladusi opsüsteem

Pärast võlvkelder loomist järgige selle all juhised loomiseks võtmed ja saladusi haldamine:

### <a name="creating-a-key"></a>Klahvi loomine

Kasutage selleks, et luua klahvi, **Lisa-AzureKeyVaultKey** kohta järgmises näites. Pärast eduka loomine, väljund cmdlet vastloodud olulisi üksikasju.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

*Lisa-AzureKeyVaultKey* cmdlet-käsu väljund on järgmine:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Nüüd saate selle klahvi loodud või üles laadida, kasutades oma URI Azure'i klahvi Vault, otsida. **KeyVaultKeyName001/https://vault010.vault.azurestack.local:443/klahvide** abil saate alati praegune versioon; ja **https://vault010.vault.azurestack.local:443, klahvid, keyVaultKeyName001 ja 86062b02b10342688f3b0b3713e343ff** abil saate selles teatud versiooni.

### <a name="retrieving-a-key"></a>Klahvi toomine

**Get-AzureKeyVaultKey** abil saate tuua klahvi ja üksikasjade kohta järgmises näites:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Järgnevalt on Get-AzureKeyVaultKey väljund

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Salajane seadmine

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Väljund

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Salajane toomine

    Get-AzureKeyVaultSecret -VaultName vault010

Väljund

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Teie olulisi vault ja klahvi või salajane saab nüüd rakenduste kasutamiseks valmis.
Tuleb lubada rakenduste neid kasutada.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Lubada kasutada klahv või salajane 

Lubada juurdepääsu (TAB) või salajane autoriloomingut, kasutage cmdlet Set -**AzureRmKeyVaultAccessPolicy** .

Näiteks kui teie vault nimi on *ContosoKeyVault* ja soovite lubada taotlus on *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed* *Kliendi ID* ja soovite lubada dekrüptida ning logige sisse oma võlvkelder abil, käivitage järgmised:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Kui soovite lubada sama rakenduse lugeda saladusi oma võlvkelder, käivitage järgmised:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Järgmised sammud
[Klahv Vault parooliga VM juurutamine](azure-stack-kv-deploy-vm-with-secret.md)

[Klahv Vault sertifikaadiga VM juurutamine](azure-stack-kv-push-secret-into-vm.md)