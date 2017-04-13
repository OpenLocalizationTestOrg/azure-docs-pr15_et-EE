<properties
    pageTitle="Automaatse varundamise jaoks SQL serveri Virtuaalmasinates (ressursihaldur) | Microsoft Azure'i"
    description="Selgitatakse automaatse varundamise funktsiooni SQL Server töötab rakenduses Azure'i Virtuaalmasinates ressursihaldur abil. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automaatse varundamise SQL Server Azure'i virtuaalmasinates (ressursihaldur)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-automated-backup.md)
- [Klassikaline](virtual-machines-windows-classic-sql-automated-backup.md)

Automaatse varundamise konfigureerib [Microsoft Azure'i varundus hallatavate](https://msdn.microsoft.com/library/dn449496.aspx) automaatselt kõik olemasolevate kui ka uute andmebaaside Azure VM, mis töötab SQL Server 2014 Standard- või suurettevõtete jaoks. See võimaldab teil konfigureerida tavaline andmebaasi varundamine, mis kasutavad püsival Azure'i bloobimälu. Automaatse varundamise sõltub [SQL Server IaaS Agent laiendamine](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel. Selles artiklis tavaversiooni, Kuva [Automaatse varundamise SQL Server Azure'i Virtuaalmasinates Classic](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Eeltingimused

Automaatse varundamise kasutamiseks võtke arvesse järgmine kohustuslik tarkvara.

**Operatsioonisüsteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL serveri versioon/versioon**:

- SQL serveri 2014 Standard
- SQL serveri 2014 Enterprise

**Andmebaasi konfigureerimine**:

- Target (sihtkoht) andmebaaside peab täielikult mudeli kasutamine

**Azure'i PowerShelli**:

- Kui soovite konfigureerida automaatse varundamise PowerShelliga [installige uusim Azure PowerShelli käske](../powershell-install-configure.md) .

>[AZURE.NOTE] Automaatse varundamise tugineb SQL Server IaaS Agent laiendamine. Praeguse SQL-i virtuaalse masina Galerii pilte lisada sellele laiendile vaikimisi. Lisateabe saamiseks vt [SQL Server IaaS Agent laiendamine](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Sätted

Järgmises tabelis kirjeldatakse saab konfigureerida automaatse varundamise soovitud suvandid. Tegelik konfigureerimistoimingute erineda sõltuvalt sellest, kas kasutate Azure portaali või Azure'i Windows PowerShelli käske.

|Säte|Vahemik (vaikeväärtus)|Kirjeldus|
|---|---|---|
|**Automaatse varundamise**|Lubada või keelata (keelatud)|Lubamine või keelamine automaatse varundamise Azure VM, mis töötab SQL Server 2014 Standard- või suurettevõtete jaoks.|
|**Säilitusperiood**|1-30 päeva (30 päeva)|Varukoopia säilitamise päevade arv.|
|**Salvestusruumi konto**|Azure storage konto (salvestusruumi konto jaoks määratud VM loodud)|Azure'i salvestusruumi konto automaatse varundamise failide talletamine bloobimälu kasutada. Ümbris luuakse selles asukohas varukoopia kõik failid salvestada. Varufaili nimetamistava sisaldab kuupäeva, kellaaja ja masina nimi.|
|**Krüptimine**|Lubada või keelata (keelatud)|Lubamine või keelamine krüptimine. Kui krüptimine on lubatud, kasutatakse taastamine varukoopia serdid asuvad määratud salvestusruumi kontot kasutades sama nimetamistava sama automaticbackup ümbrises. Kui parooli muutub, luuakse uus sert, mille parooli, kuid vana serdi jääb taastamine eelneva varukoopiad.|
|**Parooli**|Parooli tekst (pole)|Parooli krüptimise võtmed. See kehtib ainult nõutav, kui krüptimine on lubatud. On krüptitud varukoopia taastamiseks peab teil olema õiget parooli ja seotud sert, mida kasutati ajal võeti varukoopia.|

## <a name="configuration-in-the-portal"></a>Portaali konfigureerimine
Saate konfigureerida automaatse varundamise ajal ettevalmistamise või olemasoleva vms Azure'i portaal.

### <a name="new-vms"></a>Uue VMs
Azure'i portaal abil saate konfigureerida automaatse varundamise, kui loote uue SQL serveri 2014 virtuaalse masina ressursihaldur juurutamise mudel.

Valige **SQL Serveri sätted** labale **automaatse varundamise**. Järgmine Azure portaali pilt kuvatakse **SQL-i automaatse varundamise** tera.

![Portaalis Azure SQL-i automaatse varundamise konfigureerimine](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Konteksti, lugege teemat täieliku [Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md)kohta.

### <a name="existing-vms"></a>Olemasoleva VMs
Olemasoleva SQL serveri virtuaalmasinates, valige SQL serveri virtuaalne arvuti. Valige **SQL Server configuration** jaotise **sätted** tera.

![SQL-i automaatse varukoopia olemasoleva VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Klõpsake **SQL Server configuration** labale automaatse varukoopia jaotises **redigeerimine** nuppu.

![Olemasoleva VMs SQL-i automaatse varundamise konfigureerimine](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Kui olete lõpetanud, klõpsake muudatuste salvestamiseks **SQL Server configuration** tera allservas nuppu **OK** .

Kui teil on lubada automaatse varundamise esimest korda, konfigureerib Azure SQL Server IaaS Agent taustal. Sel ajal, võidakse kuvada Azure portaali automaatse varundamise konfigureerinud. Oodake mõni minut, keda installitud, konfigureeritud. Pärast seda Azure portaali kajastuvad uued sätted.

>[AZURE.NOTE] Samuti saate konfigureerida automaatse varundamise malli abil. Lisateavet leiate teemast [Azure Kiirjuhend malli automaatse varundamise](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfiguratsiooni PowerShelli abil

Pärast ettevalmistamise oma SQL-i VM, PowerShelli abil saate konfigureerida automaatse varundamise.

Järgmises näites PowerShelli automaatse varundamise on konfigureeritud lubama olemasoleva SQL serveri 2014 VM. Käsk **AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig** konfigureerib varukoopiate salvestamiseks seotud virtuaalse masina Azure storage konto automaatse varundamise sätteid. Neid varukoopiaid säilitatakse 10 päeva. Käsk **Set-AzureRmVMSqlServerExtension** värskendab määratud Azure VM neid sätteid.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

See võib kuluda mitu minutit installida ja konfigureerida SQL Server IaaS Agent.

Krüptimise lubamiseks muuta eelmise skripti edasi **EnableEncryption** parameetri koos **CertificatePassword** parameetri parooli (secure stringi). Järgmise skripti võimaldab automaatse varundamise sätted eelmises näites ja lisab krüptimine.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Automaatse varundamise keelamiseks käitage sama skripti ilma selle **-lubamine** käsk **AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig** -parameeter. Puudumisel on **-lubamine** parameetri signaale käsk funktsiooni keelamiseks. Nagu installiga võib kuluda mitu minutit keelamiseks automaatse varundamise.

>[AZURE.NOTE] SQL Server IaaS Agent eemaldamine ei eemalda eelnevalt konfigureeritud automaatse varundamise sätted. Keelake automaatse varundamise enne keelamine või SQL Server IaaS Agent desinstallimine.

## <a name="next-steps"></a>Järgmised sammud

Automaatse varundamise konfigureeritakse Azure VMs õnnestus varukoopia. Seega on oluline üle [vaadata õnnestus varukoopia dokumentatsioonist](https://msdn.microsoft.com/library/dn449496.aspx) käitumine ja mõju.

Saate otsida täiendavaid varundamine ja taastamine järgmine teema SQL Server Azure'i VMs juhised: [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-backup-recovery.md).

Muude saadaolevate automatiseerimise toimingute kohta leiate teavet teemast [SQL Server IaaS Agent laiendamine](virtual-machines-windows-sql-server-agent-extension.md).

Töötab SQL Server Azure'i VMs kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
