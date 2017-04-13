<properties
    pageTitle="Juurutamine VM abil Azure'i virnas klahvi Vault sertifikaadiga | Microsoft Azure'i"
    description="Siit saate teada, kuidas juurutada VM ja annavad Azure'i virnas klahvi hoidlast sert"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Luua VMs ja kaasa tuua klahvi hoidlast serdid

Azure'i virnas, VMs juurutatud kaudu Azure'i ressursihaldur ja nüüd saate salvestada serdid Azure'i virnas klahvi võlvkelder. Seejärel Azure'i virnas (Microsoft.Compute ressursi pakkuja olge täpne) sunnib need üheks oma VMs VMs juurutamisel. Sertide saab kasutada paljudes olukordades, sh SSL-i ja krüptimise serti autentimist.

Selle meetodi abil saate hoida serdi turvalised. Nüüd on pole VM pilt, või rakenduse failid või muude mitteusaldusväärses asukohas. Võtme vault vastav Accessi poliitikas seadmisega saate määrata, kes pääseb juurde teie sert. Teine kasu on kõik ühes kohas Azure'i virnas klahvi võlvkelder sertide saate hallata.

Siin on kiire ülevaade protsessi:

-   Teil on vaja serdi pfx-vormingus.

-   Saate luua võtme võlvkelder (kasutades malli või Järgmine näidis skript).

-   Veenduge, et on sisse lülitatud EnabledForDeployment aktiveerimine.

-   Laadige üles sert, kui on salajane.

## <a name="deploying-vms"></a>VMs juurutamine

Selle valimi skripti võtme vault loob ja salvestab serdi talletatud kohalik kataloog, et klahv vault nimega salajase pfx-faili.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Esimene osa skripti loeb pfx-fail ja seejärel talletab selle JSON objekt faili sisu base64 kodeeringuga. Seejärel JSON objekt on ka base64 kodeeringuga.

Järgmiseks loob uue ressursirühma ja seejärel loob võtme vault. Pange tähele viimase käsu New-AzureKeyVault parameetri "-EnabledForDeployment", mis annab juurdepääsu Azure (spetsiaalselt Microsoft.Compute ressursi pakkuja) lugemiseks saladusi võti võlvkelder juurutuste.

Viimase käsu salvestab base64 kodeeritud JSON objekti lihtsalt salajase võtme võlvkelder.

Siin on eelneva skripti väljundi.

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Nüüd on valmis kasutama VM malli. Pange tähele salajane väljund (nagu eelmisel väljund roheline) URI.

Peate malli asub siin. Parameetrite huvipakkuvad (lisaks tavaline VM parameetrid) on vault nime, vault ressursirühm ja salajane URI. Muidugi, saate alla laadida GitHub ja muutke vastavalt vajadusele.

See VM juurutamisel serveripoolse Azure'i VM sert.
Opsüsteemis Windows, lisatakse seotud privaatvõti pole eksporditav serdid pfx-fail. Serdi LocalMachine serdi asukoha serdi poe, et kasutaja on lisatud. Serdi faili paigutatakse Linuxi /var/lib/waagent kataloogi, selle asemel faili nime all &lt;UppercaseThumbprint&gt;.crt jaoks soovitud X509 serdifail, ja &lt;UppercaseThumbprint&gt;.prv privaatvõti jaoks.
Mõlemad failid on vormindatud .pem.

Rakenduse tavaliselt leiab serdi abil soovitud sõrmejälje ja muutmist ei vaja.

## <a name="retiring-certificates"></a>Pensionile serdid


Eelmise jaotise oleme näidanud teile, kuidas oma olemasoleva vms uut serti. Kuid oma vana sert on endiselt VM ja ei saa eemaldada. Turvalisuse lisamiseks saate muuta vana salajane atribuuti "Keelatud", kui soovite, et ka siis, kui vana malli proovib luua VM serdi vana versiooniga, siis. Siin on teatud salajane versiooni keelatava häälestusest.

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Kokkuvõte


Selle uue meetodiga saate hoida eraldi VM pilt või rakenduse last sert. Nii, et oleme eemaldanud ühe punkti käes.

Serdi saate ka pikendanud ja klahvi Vault ilma uuesti koostamiseks VM pilt või juurutamise rakendusepaketi üles laadida. Rakendus vajab küll kaasas uue URI selle serdi uus versioon.

Eraldades serdi VM või rakenduse last, me nüüd vähendada töötajatele, kes pääseb otse sert on arv.

On lisatud kasu, on nüüd ühte käepärasesse klahvi võlvkelder kõik teie sertifikaatide kohta, sh kõik versioonid, aja jooksul juurutatud haldamiseks.

## <a name="next-steps"></a>Järgmised sammud

[Klahv Vault parooliga VM juurutamine](azure-stack-kv-deploy-vm-with-secret.md)

[Lubage rakenduse avamiseks klahvi Vault](azure-stack-kv-sample-app.md)