<properties
    pageTitle="PowerShelli kasutamine varundamise ja taastamise rakendused rakendus"
    description="Saate teada, kuidas varundamine ja taastamine Azure'i rakendust Service rakenduse PowerShelli kasutamine"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>PowerShelli kasutamine varundamise ja taastamise rakendused rakendus

> [AZURE.SELECTOR]
- [PowerShelli](app-service-powershell-backup.md)
- [REST API-GA](../app-service-web/websites-csm-backup.md)

Saate teada, kuidas Azure'i PowerShelli kasutamine varundamise ja taastamise [rakendused rakendus](https://azure.microsoft.com/services/app-service/web/). Lisateavet web appi varukoopiaid, nõuded ja piirangud, sh teemast [veebirakenduse teenuses Azure rakenduse varundamiseks](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Eeltingimused
PowerShelli abil saate hallata oma rakenduse varukoopiate, vajate järgmist:

- **SAS URL-i** , mis võimaldab, mis on Azure Storage container lugemis- ja. SAS URL-ide selgituse leiate [SAS andmemudeli mõistmine](../storage/storage-dotnet-shared-access-signature-part-1.md) . Teemast [Azure PowerShelli kasutamine Azure Storage](../storage/storage-powershell-guide-full.md) näiteid Azure Storage PowerShelli kaudu haldamise.
- Kui soovite oma veebirakenduse koos andmebaasi varundamine **A andmebaasi ühendusstringi** .

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Kuidas luua SAS URL-i web appi varukoopia cmdlet-käskude kasutamine
PowerShelli abil saab luua SAS URL-i. Siin on näide sellest, kuidas luua üks, mida saab kasutada koos cmdlet-käskudega, mida selles artiklis käsitletakse.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Installige Azure'i PowerShelli 1.3.2 või suurem

Teemast [Azure PowerShelli kasutamine Azure ressursihaldur](../powershell-install-configure.md) juhiseid mooduli installimiseks ja Azure PowerShelli kaudu.

## <a name="create-a-backup"></a>Varukoopia loomine

Web appi varukoopia loomiseks saate kasutada cmdlet-käsu New-AzureRmWebAppBackup.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

See loob varukoopia nimi on automaatselt loodud. Kui soovite varukoopia nimi, kasutage BackupName valikuline parameeter.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Kaasata andmebaasi varundamine, esmalt andmebaasi varukoopia säte cmdlet-käsu New-AzureRmWebAppDatabaseBackupSetting abil luua ja seejärel lisage see säte andmebaaside parameetrit cmdleti New-AzureRmWebAppBackup. Andmebaaside parameetri aktsepteerib massiivi andmebaasi sätted, mis võimaldab teil rohkem kui ühe andmebaasi varundamine.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Varukoopiate hankimine

Get-AzureRmWebAppBackupList cmdlet-käsk tagastab massiivi kõik varukoopiate web app. Peate esitama web app ja selle ressursi rühma nime.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Teatud varukoopia saamiseks kasutage Get-AzureRmWebAppBackup cmdlet-käsk.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Saate ka toru web appi objekti, mis tahes varukoopia halduse cmdletid mugavuse.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Automaatvarunduse plaanimine

Saate ajastada intervalliga automaatselt juhtub varukoopiaid. Varunduse ajakava konfigureerimiseks kasutada cmdlet-käsu Redigeeri-AzureRmWebAppBackupConfiguration. Selle cmdlet-käsu suunab mitmed parameetrid:

- **Nimi** – veebirakenduse nime.
- **ResourceGroupName** - ressursirühm, mis sisaldab veebirakenduse nime.
- **Pesa** - valikuline. Web appi pesa nimi.
- **StorageAccountUrl** - Azure Storage container, kasutada selle varukoopiate salvestamiseks mõeldud SAS URL.
- **FrequencyInterval** – kui sageli tuleks varukoopiaid arvväärtus. Peab olema positiivne täisarv.
- **FrequencyUnit** - korral, kui sageli tuleks varukoopiaid. Suvandid on tund ja päev.
- **RetentionPeriodInDays** - automaatse varundamise funktsiooni salvestatakse automaatselt kustutamise enne mitu päeva.
- **StartTime** - valikuline. Kellaaeg, millal peaks algama automaatse varundamise funktsiooni. Varukoopiate alustada kohe, kui see on tühi. Peab olema on kuupäev ja kellaaeg.
- **Andmebaaside** - valikuline. Massiivi DatabaseBackupSettings andmebaaside varundada.
- **KeepAtLeastOneBackup** - valikuline parameeter sisse lülitatud. Lisage see, kui üks varukoopia tuleks hoida alati salvestusruumi konto, olenemata sellest, kuidas vana on.

Järgmine on näide sellest, kuidas kasutada cmdlet-käsu.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Praeguse varukoopia ajakava saamiseks kasutage Get-AzureRmWebAppBackupConfiguration cmdlet-käsk. See võib olla kasulik muutmise ajakava, mis on juba konfigureeritud.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Web appi varukoopia põhjal taastamine

Varukoopia põhjal taastamine web appi, kasutage taastamine-AzureRmWebAppBackup cmdlet-käsk. Lihtsaim viis kasutada cmdlet-käsu on cmdlet-käsk Get-AzureRmWebAppBackup või cmdlet-käsk Get-AzureRmWebAppBackupList tuua Triibu varukoopia objekt.

Kui teil on varukoopia objekti, saate selle toru taastamine-AzureRmWebAppBackup cmdlet-käsk. Määrake kirjuta vahetamise parameetri näitamaks, et kavatsete kirjutada oma veebirakenduse sisu varukoopia sisuga. Kui varukoopia sisaldab andmebaase, taastatakse ka need andmebaasid.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Järgmine on näide kõigi parameetrite määramisega taastamine – AzureRmWebAppBackup kasutamise kohta.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Varukoopia kustutamine

Varukoopia kustutamiseks kasutada cmdlet-käsu Eemalda-AzureRmWebAppBackup. See eemaldab salvestusruumi konto varukoopia. Määrake oma rakenduse nimi ja selle ressursirühma ID-d varundamise, mille soovite kustutada.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Samuti saate varukoopia objekti toru Eemalda-AzureRmWebAppBackup cmdlet selle kustutada.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
