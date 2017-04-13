<properties
    pageTitle="Klahv Vault võti ja salajane taastada krüptitud vms Azure'i varundamise | Microsoft Azure'i"
    description="Saate teada, kuidas taastada võti Vault võti ja salajane Azure varukoopia PowerShelli abil"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Krüptitud vms Azure'i varundamise klahvi Vault võti ja salajane taastamine
Selles artiklis kirjeldatakse varundamise Azure VM teha taastamine krüptitud Azure'i vms, kui teie võti ja salajane võtme võlvkelder pole olemas. Need juhised saab kasutada ka siis, kui te ei soovi säilitada eraldi koopia key (võti krüptovõtme) ja salajane (BitLockeri krüptovõtme) taastatud VM.

## <a name="pre-requisites"></a>Eeltingimused

1. **Varundus krüptitud VMs** - krüptitud Azure VMs on varundatud kasutamine Azure varukoopia. Vaadake artiklit [varundus ja taaste Azure VMs PowerShelli kaudu haldamise](backup-azure-vms-automation.md) täpsemat teavet varundamise krüptitud Azure VMs.

2. **Konfigureerida Azure klahvi Vault** – veenduge, et võti vault, millele võtmed ja saladused vajavad taastamist on juba olemas. Vaadake [Alustamine Azure'i klahvi Vault](../key-vault/key-vault-get-started.md) artiklist võtme vault haldamise kohta.

## <a name="setup-recovery-services-vault"></a>Taastamise teenuste hoidla häälestamine 
Järgmiste juhiste abil PowerShelli sisse logida ja taastamise teenuste vault konteksti määramine

### <a name="log-in-to-azure-powershell"></a>Azure'i PowerShelli sisse logida 

Azure'i konto abil järgmine cmdlet-käsk sisse logida

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Seadmine taastamine teenuste vault kontekst

Pärast sisselogimist, kasutage järgmist cmdletti saadaolevate tellimuste loendi saamiseks

```
PS C:\> Get-AzureRmSubscription
```

Valige tellimus, mille ressursid on saadaval

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Abil taastamise teenused vault, kus varundus on lubatud krüptitud vms vault konteksti määramine

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Saada taastamine punkti 

Valige soovitud hoidla, mis tähistab krüptitud Azure virtuaalse masina container

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

See ümbris kasutamisel kirje vastavale virtuaalse masina tagasi saada

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Muutuv rp varukoopia valitud üksuse taastamise punkte massiivi hankimine

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Krüptitud virtuaalse masina taastamine
Järgmiste juhiste abil saate taastada krüptitud VM, selle klahvi ja klahvi salajane.

### <a name="restore-key"></a>Võtme taastamine

Massiivi $rp eeltoodud on sorditud vastupidises järjekorras aega taastamine Viimane punkt, index 0. Näide: $rp [0] valib taastamine Viimane punkt.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Pärast selle cmdlet-käsu käivitamist edukalt bloobimälu faili saab loodud määratud kausta arvutis, kus käitatakse. Selle bloobimälu faili tähistab Key krüptitud võti krüptitud kujul.

Klahv tagasi taastamiseks võtme vault abil järgmine cmdlet-käsk. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Salajane taastamine

Salajane andmete taastamine taastamise punkti kohale saadud

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Enne vault.azure.net esindab algse tootenumbri vault nimi tekst. Pärast saladusi teksti / tähistab salajane nimi. 

Saada salajane nimi ja väärtus cmdlet-käsk Käivita ülaltoodud väljund juhuks, kui soovite kasutada sama salajane nimi. Muudel juhtudel peaks $secretname allpool värskendada kasutama salajast uus nimi. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Määrake sildid salajane, juhuks, kui VM taastamiseks kasutada ka. DiskEncryptionKeyFileName sildi väärtus peaks sisaldama nime salajane, mida plaanite kasutada. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
DiskEncryptionKeyFileName väärtus on sama, mis on saadud üle salajane nimi. Väärtus DiskEncryptionKeyEncryptionKeyURL saab võtme hoidlast pärast taastamine võtmed tagasi ja cmdlet-käsk [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) kasutamine   

Määra klahv vault salajane tagasi

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Virtuaalse masina taastamine
Ülaltoodud PowerShelli cmdlet-käskude aitavad teil taastada võti ja salajane tagasi klahv vault, kui varundatud krüptitud VM Azure VM varundamise. Pärast taastamise, leiate artiklist [varundus ja taaste Azure VMs PowerShelli kaudu haldamise](backup-azure-vms-automation.md) krüptitud VMs taastamiseks.
