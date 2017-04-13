<properties
    pageTitle="Lahti andmed kettale Windows VM | Microsoft Azure'i"
    description="Siit saate teada, saidimääratlusest andmete ketas, virtuaalse masina Azure'i ressursihaldur juurutamise mudeli abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Kuidas andmed kettale Windows virtual arvutist eemaldada.


Kui teie andmed kettale, mis on seotud virtuaalse masina enam ei vaja, saate selle hõlpsalt lahti. See eemaldab ketta virtuaalse masina, kuid ei Eemalda hoidlast. 

> [AZURE.WARNING] Kui olete sellelt kettal seda automaatselt ei kustutata. Kui olete tellinud Premium salvestusruumi, jätkab salvestusruumi tasuline jaoks ketas. Lisateabe saamiseks vaadake [hinnakirjad ja arveldamine Premium salvestusruumi kasutamise korral](../storage/storage-premium-storage.md#pricing-and-billing). 

Kui soovite olemasolevad andmed kettal uuesti kasutada, saate selle uuesti virtual samasse arvutisse või teise proovilepingu manustama.  


## <a name="detach-a-data-disk-using-the-portal"></a>Portaalis kettal andmete eemaldamine

1. Valige portaali keskuses **Virtuaalmasinates**.

2. Valige virtuaalse masina, mille soovite eemaldada, ja seejärel klõpsake nuppu **Kõik sätted**andmed kettale.

3. Valige **sätted** labale **ketast**.

4. Valige **ketast** labale andmete ketas, mille soovite eemaldada.

5. Tera jaoks andmed kettale, klõpsake käsku **Eemalda**.


    ![Kuvatõmmis, kus on kuvatud nupp Eemalda.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Ketas salvestusruumi jääb alles, kuid on virtuaalse masina enam juurde.


## <a name="detach-a-data-disk-using-powershell"></a>PowerShelli kasutamine andmete kettal eemaldamine

Selles näites saab esimese käsu virtuaalse masina nimega **MyVM07** **RG11** ressursirühm Get-AzureRmVM cmdlet-käsu abil. Käsu talletab virtuaalse masina **$VirtualMachine** muutujana. 

Teine käsk eemaldab andmed kettale nimega DataDisk3 virtual arvutist. 

Lõplik käsk värskendab olek virtuaalse masina andmed kettale eemaldamise protsessi lõpuleviimiseks.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Lisateavet leiate teemast [Eemalda-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Järgmised sammud

Kui soovite andmed kettale uuesti kasutada, saate seda teha lihtsalt [Lisage see teise VM](virtual-machines-windows-attach-disk-portal.md)
