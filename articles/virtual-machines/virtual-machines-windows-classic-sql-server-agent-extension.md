<properties
    pageTitle="SQL Serveri Agent laiend SQL serveri vms (klassikaline) | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse, kuidas hallata SQL serveri agent laiend, mis automatiseerib teatud SQL serveri haldustoimingute. Nendeks automaatse varundamise, automaatse lappimine ja Azure klahvi Vault integreerimine. Selles teemas kasutab juurutamise klassikaline režiim."
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Serveri Agent laiend SQL serveri vms (klassikaline)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassikaline](virtual-machines-windows-classic-sql-server-agent-extension.md)

Azure'i virtuaalmasinates Administreerimine automatiseerida töötab SQL Server IaaS Agent laiend (SQLIaaSAgent). Selles teemas antakse ülevaade laiend, samuti juhised installimist, oleku ja eemaldamist ei toeta teenust.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Selles artiklis ressursihaldur versiooni, leiate [SQL Serveri Agent laiend jaoks SQL serveri VMs ressursihaldur](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Toetatud teenused

SQL Server IaaS Agent laiend toetab Administreerimine järgmisi toiminguid:

| Funktsioonide haldamine | Kirjeldus |
|---------------------|-------------------------------|
| **SQL-i automaatse varundamise** | Automatiseerib kõigile andmebaasidele varukoopiate plaanimine vaikimisi VM SQL Serveri eksemplari. Lisateavet leiate teemast [automaatse varundamise SQL Server Azure'i Virtuaalmasinates (klassikaline)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL-i automaatse lappimine** | Konfigureerib hoolduse ajal, mis teie VM võib kuluda kohas nii, et saate vältida värskenduste tippväärtus aegadel jaoks oma töökoormus. Lisateavet leiate teemast [automaatse lappimine SQL Server Azure'i Virtuaalmasinates (klassikaline)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Azure'i võtme Vault integreerimine** | Võimaldab teil automaatselt installida ja konfigureerida Azure klahvi Vault oma SQL serveri VM. Lisateabe saamiseks vt [Konfigureerimine Azure'i klahvi Vault integreerimine SQL Server Azure'i VMs (klassikaline)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Eeltingimused

Nõuded SQL Server IaaS Agent laiend kasutamine oma VM:

### <a name="operating-system"></a>Operatsioonisüsteem:

- Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>SQL serveri versiooni:

- SQL Server 2012
- SQL serveri 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure'i PowerShelli:

[Laadige alla ja konfigureerimine uusimate Azure PowerShelli käske](../powershell-install-configure.md).

Käivitage Windows PowerShelli ja ühendage see käsk **Lisa-AzureAccount** Azure tellimuse.

    Add-AzureAccount

Kui teil on mitu tellimust, abil **Valige-AzureSubscription** valige tellimus, mis sisaldab teie eesmärk klassikaline VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Selles etapis saate klassikaline virtuaalmasinates ja nende seotud teenuste nimed käsuga **Get-AzureVM** loendit.

    Get-AzureVM

## <a name="installation"></a>Installimine

Klassikaline vms, kasutage PowerShelli installida SQL Server IaaS Agent laiend ja sellega seotud teenused konfigureerimine. **Set-AzureVMSqlServerExtension** PowerShelli cmdlet-käsu abil saate installida laiendamine. Näiteks järgmine käsk installib laiendamine Windows Server VM (klassikaline) ja see nimed "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Kui värskendate uusima versiooni SQL-i IaaS Agent laiend, tuleb taaskäivitada virtuaalne arvuti pärast värskendamist laiendamine.

>[AZURE.NOTE] Klassikaline virtuaalmasinates ei saa installida ja konfigureerida SQL-i IaaS Agent laiend portaali kaudu soovitud suvand.

## <a name="status"></a>Olek

Veenduge, et installitud on laiendamine üheks võimaluseks on Azure'i portaalis agent oleku vaatamine. Valige **Kõik sätted** virtuaalse masina tera ja seejärel klõpsake **laiendid**. Näha peaks olema loetletud **SQLIaaSAgent** laiendamine.

![SQL Server IaaS Agent laiend Azure'i portaalis](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Saate kasutada ka **Get-AzureVMSqlServerExtension** Azure PowerShelli cmdlet-käsk.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Eemaldamine   

Azure'i portaalis, saate desinstallida, klõpsates kolmikpunkti **laiendid** enne oma virtuaalse masina atribuutide laiendamine. Klõpsake nuppu **Kustuta**.

![Desinstallige SQL Server IaaS Agent laiend Azure'i portaalis](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Saate kasutada ka **Eemalda-AzureVMSqlServerExtension** PowerShelli cmdlet-käsk.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Järgmised sammud

Teenuse laiendamine ei toeta kasutama hakata. Lisateavet leiate teemadest viidatud [toetatud teenuste](#supported-services) käesoleva artikli jaotisest.

Töötab SQL Server Azure'i Virtuaalmasinates kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
