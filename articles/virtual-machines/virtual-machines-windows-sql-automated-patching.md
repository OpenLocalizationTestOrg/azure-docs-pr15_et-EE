<properties
    pageTitle="Automaatse lappimine SQL serveri vms (ressursihaldur) | Microsoft Azure'i"
    description="Selgitab funktsiooni automatiseeritud lappimine jaoks SQL serveri Virtuaalmasinates töötab Azure ressursihaldur abil."
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
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automaatse lappimine SQL Server Azure'i virtuaalmasinates (ressursihaldur)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-automated-patching.md)
- [Klassikaline](virtual-machines-windows-classic-sql-automated-patching.md)

Automaatne lappimine luuakse hoolduse jaoks on Azure virtuaalse masina SQL Server töötab. Automatiseeritud värskendusi saab installida ainult akna hoolduse ajal. SQL Server, see rescriction tagab süsteemi värskendused ja mis tahes seotud taaskäivitamist tekkida kõige paremini võimalik ajal andmebaasi. Automaatne lappimine sõltub [SQL Server IaaS Agent laiendamine](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel. Selles artiklis tavaversiooni, Kuva [Automatiseeritud lappimine SQL Server Azure'i Virtuaalmasinates Classic](virtual-machines-windows-classic-sql-automated-patching.md).

## <a name="prerequisites"></a>Eeltingimused

Saate kasutada automatiseeritud lappimine, kaaluge järgmine kohustuslik tarkvara.

**Operatsioonisüsteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL serveri versioon**:

- SQL Server 2012
- SQL serveri 2014
- SQL Server 2016

**Azure'i PowerShelli**:

- Kui soovite konfigureerida automaatse lappimine PowerShelliga [installige uusim Azure PowerShelli käske](../powershell-install-configure.md) .

>[AZURE.NOTE] Automaatne lappimine tugineb SQL Server IaaS Agent laiendamine. Praeguse SQL-i virtuaalse masina Galerii pilte lisada sellele laiendile vaikimisi. Lisateabe saamiseks vt [SQL Server IaaS Agent laiendamine](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Sätted

Järgmises tabelis kirjeldatakse saab konfigureerida automaatse lappimine soovitud suvandid. Tegelik konfigureerimistoimingute erineda sõltuvalt sellest, kas kasutate Azure portaali või Azure'i Windows PowerShelli käske.

|Säte|Võimalikud väärtused|Kirjeldus|
|---|---|---|
|**Automaatse lappimine**|Lubada või keelata (keelatud)|Lubamine või keelamine automatiseeritud lappimine on Azure virtuaalse masina jaoks.|
|**Hoolduse ajakava**|Iga päev, Esmaspäev, Teisipäev, Kolmapäev, Neljapäev, Reede, Laupäev, pühapäev|Ajakava allalaadimine ja installimine Windows, SQL serveri ja Microsoft virtual arvuti.|
|**Hoolduse start tund**|0-24|Kohaliku alguskellaaeg virtuaalse masina värskendada.|
|**Hoolduse akna kestus**|30-180|Minutid, mille lubatud alla laadida ja värskenduste installimise lõpuleviimiseks.|
|**Paikade kategooria**|Oluliste|Kategooria värskendused alla laadima ja installima.|

## <a name="configuration-in-the-portal"></a>Portaali konfigureerimine
Azure portaali abil saate konfigureerida automaatse lappimine ajal ettevalmistamise või olemasoleva vms.

### <a name="new-vms"></a>Uue VMs
Azure portaali abil saate konfigureerida automaatse lappimine, kui loote uue SQL serveri virtuaalse masina ressursihaldur juurutamise mudeli.

Valige **SQL Serveri sätted** labale **automatiseeritud lappimine**. Järgmine Azure portaali pilt kuvatakse **SQL-i automaatse lappimine** tera.

![Automaatse paikamise SQL Azure'i portaalis](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Konteksti, lugege teemat täieliku [Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md)kohta.

### <a name="existing-vms"></a>Olemasoleva VMs
Olemasoleva SQL serveri virtuaalmasinates, valige SQL serveri virtuaalne arvuti. Valige **SQL Server configuration** jaotise **sätted** tera.

![SQL-i automaatne paikamine olemasoleva VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Klõpsake **SQL Server configuration** labale automatiseeritud, lappimine jaotises **redigeerimine** nuppu.

![Olemasoleva VMs SQL-i automaatse lappimine konfigureerimine](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Kui olete lõpetanud, klõpsake muudatuste salvestamiseks **SQL Server configuration** tera allservas nuppu **OK** .

Kui esimest korda on lubada automaatse lappimine, konfigureerib Azure SQL Server IaaS Agent taustal. Sel ajal, võidakse kuvada Azure portaali automatiseeritud lappimine konfigureeritud. Oodake mõni minut, keda installitud, konfigureeritud. Pärast, mis kajastab Azure portaali uued sätted.

>[AZURE.NOTE] Samuti saate konfigureerida automaatse lappimine malli abil. Lisateabe saamiseks lugege teemat [Azure Kiirjuhend malli automatiseeritud lappimine](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Konfiguratsiooni PowerShelli abil

Pärast ettevalmistamise oma SQL-i VM, PowerShelli abil saate konfigureerida automaatse lappimine.

Järgmises näites kasutatakse PowerShelli konfigureerimiseks automatiseeritud lappimine mõne olemasoleva SQL serveri VM. Käsk **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** konfigureerib automaatvärskenduste hooldustööd uues aknas.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Selle näite põhjal, järgmises tabelis kirjeldatakse kasulikku mõju sihtkohas Azure VM.

|Parameetri|Efekti|
|---|---|
|**DayOfWeek**|Iga Neljapäev installitud plaastrid.|
|**MaintenanceWindowStartingHour**|Alustage uuendused 11:00 am.|
|**MaintenanceWindowsDuration**|Plaastrid peab olema installitud 120 minutit. Algusaja põhjal, nad lõpule viima 1:00 pm.|
|**PatchCategory**|See parameeter on võimalik ainult **tähtis**.|

See võib kuluda mitu minutit installida ja konfigureerida SQL Server IaaS Agent.

Automaatse lappimine keelamiseks käitage sama skripti ilma selle **-lubamine** **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**-parameeter. Puudumisel on **-lubamine** parameetri signaale käsk funktsiooni keelamiseks.

## <a name="next-steps"></a>Järgmised sammud

Muude saadaolevate automatiseerimise toimingute kohta leiate teavet teemast [SQL Server IaaS Agent laiendamine](virtual-machines-windows-sql-server-agent-extension.md).

Töötab SQL Server Azure'i VMs kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
