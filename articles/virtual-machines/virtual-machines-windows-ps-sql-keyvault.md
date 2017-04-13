<properties
    pageTitle="Konfigureerige SQL Server Azure'i võtme Vault integreerimine Azure VMs (ressursihaldur)"
    description="Saate teada, kuidas automatiseerida konfiguratsiooni SQL serveri krüptimise Azure'i klahvi Vault jaoks. See teema selgitab, kuidas kasutada SQL serveri virtuaalmasinates ressursihaldur abil loodud Azure'i klahvi Vault integreerimine."
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
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Konfigureerige SQL Server Azure'i võtme Vault integreerimine Azure VMs (ressursihaldur)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-ps-sql-keyvault.md)
- [Klassikaline](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Ülevaade
On mitu SQL serveri krüptimise funktsioone, nt [läbipaistev andmete krüptimine (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [veeru taseme krüptimise (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)ja [krüptimise varukoopia](https://msdn.microsoft.com/library/dn449489.aspx). Krüptimise vormid nõuavad haldamine ja talletada krüptimiseks kasutatavat cryptographic võtmed. Azure'i klahvi Vault (AKV) teenus on mõeldud parandada turvalisust ja nende klahvide turvaline ja tugevalt saadaval kohas haldus. [SQL serveri Connector](http://www.microsoft.com/download/details.aspx?id=45344) võimaldab SQL Server Azure'i klahvi hoidlast nende abil.

Kui teil töötab SQL serveri asutusesiseste masinad seal [järgmised juhised](https://msdn.microsoft.com/library/dn198405.aspx)aitavad teil juurde pääseda Azure'i klahvi Vault asutusesisese SQL serveri teie arvutist. Kuid SQL Server Azure'i VM, säästate aega *Azure'i klahvi Vault integreerimine* funktsiooni abil.

Kui see funktsioon on lubatud, see automaatselt installib SQL serveri konnektor, konfigureerib EKM pakkuja juurde pääseda Azure'i klahvi Vault ja loob, kui soovite lubada juurdepääsu oma vault mandaati. Kui eelnevalt mainitud kohapealse dokumentatsiooni juhiseid vaadanud, saate vaadata, et see funktsioon automatiseerib juhiseid 2 – 3. Ainus oleks vaja veel teha käsitsi on loomiseks olulisi hoidla- ja võtmed. Seal on automatiseeritud kogu häälestamine oma SQL-i VM. Kui see funktsioon on selle häälestamise lõpetanud, saab käivitada T-SQL-lausete alustamiseks krüptimise oma andmebaasid või varukoopiaid nagu tavaliselt.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Lubamine ja konfigureerimine AKV integreerimine
Saate lubada AKV integreerimise ajal ettevalmistamise või konfigureerimine olemasoleva VMs.

### <a name="new-vms"></a>Uue VMs
Kui on ettevalmistamise uute SQL serveri virtuaalse masina koos ressursihaldur, Azure portaali leiate Azure'i klahvi Vault integreerimise lubamise etapi. Azure'i klahvi Vault funktsioon on saadaval ainult Enterprise, arendaja ja SQL serveri väljaanded hindamiseks.

![SQL Azure'i võtme Vault integreerimine](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Üksikasjaliku selgituse ettevalmistamise, leiate teemast [sätte virtuaalse masina SQL Server Azure'i portaal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Olemasoleva VMs
Olemasoleva SQL serveri virtuaalmasinates, valige SQL serveri virtuaalne arvuti. Valige **SQL Server configuration** jaotise **sätted** tera.

![SQL-i AKV integreerimine olemasoleva vms](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

**SQL Server configuration** labale nuppu **Redigeeri** jaotises automatiseeritud klahvi Vault integreerimine.

![Olemasoleva vms SQL-i AKV integreerimise konfigureerimine](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Kui olete lõpetanud, klõpsake muudatuste salvestamiseks **SQL Server configuration** tera allservas nuppu **OK** .

>[AZURE.NOTE] Samuti saate konfigureerida AKV integreerimine malli abil. Lisateabe saamiseks lugege teemat [Azure klahvi Vault integreerimine Azure Kiirjuhend malli](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
