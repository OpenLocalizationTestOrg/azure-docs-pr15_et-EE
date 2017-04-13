<properties
    pageTitle="Konfigureerige SQL Server Azure'i võtme Vault integreerimine Azure VMs (klassikaline)"
    description="Saate teada, kuidas automatiseerida konfiguratsiooni SQL serveri krüptimise Azure'i klahvi Vault jaoks. See teema selgitab, kuidas virtuaalmasinates loomine klassikaline juurutamise mudel SQL Server Azure'i klahvi Vault integreerimise abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Konfigureerige SQL Server Azure'i võtme Vault integreerimine Azure VMs (klassikaline)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-ps-sql-keyvault.md)
- [Klassikaline](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Ülevaade
On mitu SQL serveri krüptimise funktsioone, nt [läbipaistvaid andmete krüptimine (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [veeru taseme krüptimise (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)ja [varukoopia krüptimine](https://msdn.microsoft.com/library/dn449489.aspx). Krüptimise vormid nõuavad haldamine ja talletada krüptimiseks kasutatavat cryptographic võtmed. Azure'i klahvi Vault (AKV) teenus on mõeldud parandada turvalisust ja nende klahvide turvaline ja tugevalt saadaval kohas haldus. [SQL serveri konnektor](http://www.microsoft.com/download/details.aspx?id=45344) võimaldab SQL Server Azure'i klahvi hoidlast nende abil.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Kui kasutate SQL serveri kohapealse masinad, on [juhised saab juurdepääsu Azure klahvi Vault asutusesisese SQL serveri teie arvutist](https://msdn.microsoft.com/library/dn198405.aspx). Kuid SQL Server Azure'i VM, säästate aega *Azure'i klahvi Vault integreerimine* funktsiooni abil. Selle funktsiooni lubamiseks paar Azure PowerShelli cmdletid, kus automatiseerida konfiguratsiooni, mis on vajalikud SQL-i VM juurde pääseda oma võtme vault.

Kui see funktsioon on lubatud, see automaatselt installib SQL serveri konnektor, konfigureerib EKM pakkuja juurde pääseda Azure'i klahvi Vault ja loob, kui soovite lubada juurdepääsu oma vault mandaati. Kui eelnevalt mainitud kohapealse dokumentatsiooni juhiseid vaadanud, saate vaadata, et see funktsioon automatiseerib juhiseid 2 – 3. Ainus oleks vaja veel teha käsitsi on loomiseks olulisi hoidla- ja võtmed. Seal on automatiseeritud kogu häälestamine oma SQL-i VM. Kui see funktsioon on selle häälestamise lõpetanud, saab käivitada T-SQL-lausete alustamiseks krüptimise oma andmebaasid või varukoopiaid nagu tavaliselt.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV integreerimise konfigureerimine
PowerShelli abil saate konfigureerida Azure klahvi Vault integreerimine. Järgmistes jaotistes ülevaade parameetrid ja seejärel proovi PowerShelli skripti.

### <a name="install-the-sql-server-iaas-extension"></a>Installida SQL Server IaaS laiend

Esmalt [installida SQL Server IaaS laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Sisendparameetrite mõistmine
Järgmises tabelis on loetletud järgmises jaotises PowerShelli skripti käitamiseks nõutavad parameetrid.

|Parameetri|Kirjeldus|Näide|
|---|---|---|
|**$akvURL**|**URL-i võtme vault**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Teenuse turvasubjektinimi**|"fde2b411 - 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Teenuse põhilise salajane**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Mandaadi nimi**: AKV integreerimine loob jooksul SQL Server, võimaldades juurdepääsu võtme vault VM mandaati. Valige selle mandaadi nimi.|"mycred1"|
|**$vmName**|**Virtuaalse masina nimi**: varem loodud VM SQL-i nimi.|"myvmname"|
|**$serviceName**|**Teenuse nimi**: The pilveteenuses nimi, mis on seotud SQL-i VM.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Luba AKV PowerShelli integreerimine
Cmdlet-käsu **New-AzureVMSqlServerKeyVaultCredentialConfig** loob konfiguratsiooni objekti Azure'i klahvi Vault integreerimine funktsiooni. **Set-AzureVMSqlServerExtension** konfigureerib selle integreerimine **KeyVaultCredentialSettings** parameeter. Järgmised toimingud näitab, kuidas neid käske kasutada.

1. Azure'i PowerShellis esmalt konfigureerida sisendparameetrite oma kindlate väärtustega, nagu on kirjeldatud selles teemas eelmiste jaotiste. Järgmise skripti on näide.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Seejärel kasutage järgmist skripti AKV integreerimise lubamiseks ja konfigureerimiseks.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

SQL-i IaaS Agent laiend värskendatakse selle uue konfiguratsiooni SQL-i VM.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
