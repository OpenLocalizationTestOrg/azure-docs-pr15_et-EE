<properties
    pageTitle="Häälestamiseks WinRM Accessi Virtuaalmasinates Azure'i ressursihaldur | Microsoft Azure'i"
    description="Kuidas seadistada WinRM Accessi kasutamiseks koos mõne Azure'i ressursihaldur virtuaalse masina"
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
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Virtuaalmasinates Azure'i ressursihaldur WinRM juurdepääsu seadistamine

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>Windows Azure'i Teenusehaldus vs Azure'i ressursihaldur kaughalduse

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudeli

* Ülevaate Azure'i ressursihaldur, lugege käesoleva [artikli](../azure-resource-manager/resource-group-overview.md)
* Azure'i Teenusehaldus ja Azure ressursihaldur erinevused, lugege käesoleva [artikli](../resource-manager-deployment-model.md)

Võtme vahe häälestamise WinRM konfiguratsiooni kaks virnadena vahel on, kuidas serdi saab installitud VM. Azure'i ressursihaldur virnas serdid on modelleerida nagu võti Vault ressursi pakkuja hallatavad ressursse. Seetõttu peab kasutaja anda oma serti ja laadige see võti Vault enne selle kasutamist VM.

Siin on mõned toimingud, mida peate tegema häälestamiseks VM WinRM Ühenduvus

1. Võtme Vault loomine
2. Iseallkirjastatud serdi loomine
3. Klahv Vault oma iseallkirjastatud serdi üleslaadimine
4. Teie iseallkirjastatud serdi klahvi võlvkelder URL-i hankimine
5. Viide oma iseallkirjastatud serdid URL-i VM loomisel

## <a name="step-1-create-a-key-vault"></a>Samm 1: Loo võtme Vault

Saate kasutada funktsiooni all käsku Loo võti Vault

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Samm 2: Iseallkirjastatud serdi loomine
Saate luua iseallkirjastatud serdi PowerShelli skripti abil

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Samm 3: Key Vault oma iseallkirjastatud serdi üleslaadimine

Enne serdi üleslaadimine klahvi Vault juhises loodud 1, tuleb ümber Microsoft.Compute ressursi pakkuja mõistate vorming. Selle all PowerShelli skripti võimaldab teil seda teha

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Samm 4: Teie iseallkirjastatud serdi klahvi võlvkelder URL-i hankimine

Microsoft.Compute ressursi pakkuja peab salajane võti Vault sees URL-i ajal ettevalmistamise VM. See võimaldab Microsoft.Compute ressursi pakkuja salajane alla laadida ja luua võrdväärse serdi VM.

>[AZURE.NOTE]Salajane URL-i peab sisaldama versioon ka. Näide URL näeb välja umbes https://contosovault.vault.azure.net:443/saladused contososecret 01h9db0df2cd4300a20ence585a6s7ve all


#### <a name="templates"></a>Mallid

Lingi URL malli abil saate selle all kood

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShelli

URL-i abil saate selle all PowerShelli käsu

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Juhis 5: Viidata oma iseallkirjastatud serdid URL-i VM loomisel

#### <a name="azure-resource-manager-templates"></a>Azure'i ressursihaldur Mallid

Mallide VM loomisel serdi saab viidatud saladusi jaotis ja winRM jaotist allpool:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Ülaltoodud valimi malli leiate siit [201-vm – winrm-keyvault -](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows) Windows

Selle malli lähtekoodi leiate [github](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShelli

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Samm 6: Ühenduse VM
Enne kui saate luua ühenduse VM peate veenduge, et teie arvuti on konfigureeritud WinRM kaughalduse. Käivitage PowerShelli administraatorina ja käivitada selle all käsk veendumaks, et häälestamist.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Võib juhtuda, veendumaks, et WinRM teenus töötab, kui eespool ei tööta. Selle abil saate teha`Get-Service WinRM`

Kui häälestamine on tehtud, saate luua ühenduse VM abil selle all käsk

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate