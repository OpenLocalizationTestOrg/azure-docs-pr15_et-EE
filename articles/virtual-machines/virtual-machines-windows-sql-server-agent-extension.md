<properties
    pageTitle="SQL Serveri Agent laiend SQL serveri vms (ressursihaldur) | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse, kuidas hallata SQL serveri agent laiend, mis automatiseerib teatud SQL serveri haldustoimingute. Nendeks automaatse varundamise, automaatse lappimine ja Azure klahvi Vault integreerimine. Selles teemas kasutab ressursihaldur juurutamise režiimi."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Serveri Agent laiend SQL serveri vms (ressursihaldur)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassikaline](virtual-machines-windows-classic-sql-server-agent-extension.md)

Azure'i virtuaalmasinates Administreerimine automatiseerida töötab SQL Server IaaS Agent laiend (SQLIaaSExtension). Selles teemas antakse ülevaade laiend, samuti juhised installimist, oleku ja eemaldamist ei toeta teenust.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel. Selles artiklis tavaversiooni, Kuva [SQL Serveri Agent laiend SQL serveri VMs klassikaline](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Toetatud teenused

SQL Server IaaS Agent laiend toetab Administreerimine järgmisi toiminguid:

| Funktsioonide haldamine | Kirjeldus |
|---------------------|-------------------------------|
| **SQL-i automaatse varundamise** | Automatiseerib kõigile andmebaasidele varukoopiate plaanimine vaikimisi VM SQL Serveri eksemplari. Lisateavet leiate teemast [automaatse varundamise SQL Server Azure'i Virtuaalmasinates (ressursihaldur)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL-i automaatse lappimine** | Konfigureerib hoolduse ajal, mis teie VM võib kuluda kohas nii, et saate vältida värskenduste tippväärtus aegadel jaoks oma töökoormus. Lisateavet leiate teemast [automatiseeritud lappimine SQL Server Azure'i Virtuaalmasinates (ressursihaldur)](virtual-machines-windows-sql-automated-patching.md).|
| **Azure'i võtme Vault integreerimine** | Võimaldab teil automaatselt installida ja konfigureerida Azure klahvi Vault oma SQL serveri VM. Lisateabe saamiseks vt [Konfigureerimine Azure'i klahvi Vault integreerimine SQL Server Azure'i VMs (ressursihaldur)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Eeltingimused

Nõuded SQL Server IaaS Agent laiend kasutamine oma VM:

**Operatsioonisüsteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL serveri versiooni**:

- SQL Server 2012
- SQL serveri 2014
- SQL Server 2016

**Azure'i PowerShelli**:

- [Laadige alla ja konfigureerimine uusimate Azure PowerShelli käskude](../powershell-install-configure.md)

## <a name="installation"></a>Installimine

SQL Server IaaS Agent laiend installitakse automaatselt, kui olete ette üks SQL serveri virtuaalse masina galerii pildid.

Kui loote mõnda OS ainult Windows Server virtuaalse masina, saate laiendamine käsitsi installida, **Set-AzureVMSqlServerExtension** PowerShelli cmdleti abil. Näiteks järgmine käsk installib laiendamine on ainult OS Windows Server VM ja selle nimed "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Kui värskendate uusima versiooni SQL-i IaaS Agent laiend, tuleb taaskäivitada virtuaalne arvuti pärast värskendamist laiendamine.

>[AZURE.NOTE] Kui installite SQL Server IaaS Agent laiend Windows Server VM käsitsi, peate kasutama ja hallata oma funktsioone PowerShelli käskude abil. Portaali kasutajaliides on saadaval ainult SQL serveri galerii pildid.

## <a name="status"></a>Olek

Veenduge, et installitud on laiendamine üheks võimaluseks on Azure'i portaalis agent oleku vaatamine. Valige **Kõik sätted** virtuaalse masina tera ja seejärel klõpsake **laiendid**. Näha peaks olema loetletud **SQLIaaSExtension** laiendamine.

![SQL Server IaaS Agent laiend Azure'i portaalis](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Saate kasutada ka **Get-AzureVMSqlServerExtension** Azure PowerShelli cmdlet-käsk.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Eelmise käsu kinnitab agent on installitud ja üldise oleku teave. Samuti saate teatud olek teavet automaatse varundamise lappimine järgmised käsud.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Eemaldamine   

Azure'i portaalis, saate desinstallida, klõpsates kolmikpunkti **laiendid** enne oma virtuaalse masina atribuutide laiendamine. Klõpsake nuppu **Kustuta**.

![Desinstallige SQL Server IaaS Agent laiend Azure'i portaalis](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Saate kasutada ka **Eemalda-AzureRmVMSqlServerExtension** PowerShelli cmdlet-käsk.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Järgmised sammud

Teenuse laiendamine ei toeta kasutama hakata. Lisateavet leiate teemadest viidatud [toetatud teenuste](#supported-services) käesoleva artikli jaotisest.

Töötab SQL Server Azure'i Virtuaalmasinates kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
