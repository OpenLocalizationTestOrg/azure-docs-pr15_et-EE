<properties
    pageTitle="Automaatse varundamise jaoks SQL serveri Virtuaalmasinates (klassikaline) | Microsoft Azure'i"
    description="Selgitatakse automaatse varundamise funktsiooni SQL Server töötab rakenduses Azure'i Virtuaalmasinates ressursihaldur abil. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automaatse varundamise SQL Server Azure'i virtuaalmasinates (klassikaline)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-automated-backup.md)
- [Klassikaline](virtual-machines-windows-classic-sql-automated-backup.md)

Automaatse varundamise konfigureerib [Microsoft Azure'i varundus hallatavate](https://msdn.microsoft.com/library/dn449496.aspx) automaatselt kõik olemasolevate kui ka uute andmebaaside Azure VM, mis töötab SQL Server 2014 Standard- või suurettevõtete jaoks. See võimaldab teil konfigureerida tavaline andmebaasi varundamine, mis kasutavad püsival Azure'i bloobimälu. Automaatse varundamise sõltub [SQL Server IaaS Agent laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Selles artiklis ressursihaldur versiooni, Kuva [SQL Server Azure'i Virtuaalmasinates ressursihaldur automaatse varundamise](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Eeltingimused

Automaatse varundamise kasutamiseks võtke arvesse järgmine kohustuslik tarkvara.

**Operatsioonisüsteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL serveri versioon/versioon**:

- SQL serveri 2014 Standard
- SQL serveri 2014 Enterprise

>[AZURE.NOTE] SQL Server 2016 automaatse varundamise veel ei toetata.

**Andmebaasi konfigureerimine**:

- Target andmebaaside peate kasutama täielikult mudel.

**Azure'i PowerShelli**:

- [Installige uusim Azure PowerShelli käske](../powershell-install-configure.md).

**SQL Server IaaS laiend**:

- [Installida SQL Server IaaS laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Sätted

Järgmises tabelis kirjeldatakse saab konfigureerida automaatse varundamise soovitud suvandid. Klassikaline vms, kasutage PowerShelli nende sätete konfigureerimiseks.

|Säte|Vahemik (vaikeväärtus)|Kirjeldus|
|---|---|---|
|**Automaatse varundamise**|Lubada või keelata (keelatud)|Lubamine või keelamine automaatse varundamise Azure VM, mis töötab SQL Server 2014 Standard- või suurettevõtete jaoks.|
|**Säilitusperiood**|1-30 päeva (30 päeva)|Varukoopia säilitamise päevade arv.|
|**Salvestusruumi konto**|Azure storage konto (salvestusruumi konto jaoks määratud VM loodud)|Azure'i salvestusruumi konto automaatse varundamise failide talletamine bloobimälu kasutada. Ümbris luuakse selles asukohas varukoopia kõik failid salvestada. Varufaili nimetamistava sisaldab kuupäeva, kellaaja ja masina nimi.|
|**Krüptimine**|Lubada või keelata (keelatud)|Lubamine või keelamine krüptimine. Kui krüptimine on lubatud, kasutatakse taastamine varukoopia serdid asuvad määratud salvestusruumi kontot kasutades sama nimetamistava sama automaticbackup ümbrises. Kui parooli muutub, luuakse uus sert, mille parooli, kuid vana serdi jääb taastamine eelneva varukoopiad.|
|**Parooli**|Parooli tekst (pole)|Parooli krüptimise võtmed. See kehtib ainult nõutav, kui krüptimine on lubatud. On krüptitud varukoopia taastamiseks peab teil olema õiget parooli ja seotud sert, mida kasutati ajal võeti varukoopia.|

## <a name="configuration-with-powershell"></a>Konfiguratsiooni PowerShelli abil

Järgmises näites PowerShelli automaatse varundamise on konfigureeritud lubama olemasoleva SQL serveri 2014 VM. Käsu **New-AzureVMSqlServerAutoBackupConfig** konfigureerib automaatse varundamise sätted salvestada varukoopiate Azure storage konto määratud $storageaccount muutujana. Neid varukoopiaid säilitatakse 10 päeva. Käsk **Set-AzureVMSqlServerExtension** värskendab määratud Azure VM neid sätteid.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

See võib kuluda mitu minutit installida ja konfigureerida SQL Server IaaS Agent.

Krüptimise lubamiseks muuta eelmise skripti edasi EnableEncryption parameetri koos CertificatePassword parameetri parooli (secure stringi). Järgmise skripti võimaldab automaatse varundamise sätted eelmises näites ja lisab krüptimine.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Automaatse varundamise keelamiseks käitage sama skripti ilma selle **-lubamine** **New-AzureVMSqlServerAutoBackupConfig**-parameeter. Nagu installiga võib kuluda mitu minutit keelamiseks automaatse varundamise.

>[AZURE.NOTE] Keelamise ja SQL Server IaaS Agent desinstallimine ei eemalda eelnevalt konfigureeritud hallatavate varundamise sätted. Keelake automaatse varundamise enne keelamine või SQL Server IaaS Agent desinstallimine.

## <a name="next-steps"></a>Järgmised sammud

Automaatse varundamise konfigureeritakse Azure VMs õnnestus varukoopia. Seega on oluline üle [vaadata õnnestus varukoopia dokumentatsioonist](https://msdn.microsoft.com/library/dn449496.aspx) käitumine ja mõju.

Saate otsida täiendavaid varundamine ja taastamine järgmine teema SQL Server Azure'i VMs juhised: [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-backup-recovery.md).

Muude saadaolevate automatiseerimise toimingute kohta leiate teavet teemast [SQL Server IaaS Agent laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

Töötab SQL Server Azure'i VMs kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
