<properties
   pageTitle="Kuidas sildistamiseks VM | Microsoft Azure'i"
   description="Siit saate teada, virtuaalse masina Windows Azure'i ressursihaldur juurutamise mudeli abil loodud sildistamise kohta"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Kuidas sildistamiseks virtuaalse masina Windows Azure


Selles artiklis kirjeldatakse sildistamiseks virtuaalse masina Windows Azure läbi ressursihaldur juurutamise mudeli erineval viisil. Sildid on kasutaja määratletud võti ja väärtuse paarideks, mille saab paigutada otse ressursi või ressursirühma. Azure'i toetab praegu kuni 15 siltide ressursside ja ressursirühma kohta. Siltide võib panna ressursi loomise ajal või lisatakse mõne olemasoleva ressursi. Pange tähele, et sildid on toetatud ressursse, mis on loodud ressursihaldur juurutamise mudeli ainult kaudu. Kui soovite sildistamiseks Linux virtuaalse masina, vaadake, [Kuidas sildistamiseks Azure virtuaalse masina Linux](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Sildistamine PowerShelli abil

Luua, lisamine ja kustutamine siltide PowerShelli, teil on vaja kõigepealt [PowerShelli keskkonnas Azure'i ressursihaldur][]häälestamine kaudu. Kui olete häälestamise lõpule viinud, saate paigutada siltide Arvuta, võrgu ja salvestusruumi ressursside loomine või pärast ressursi loomist PowerShelli kaudu. Selles artiklis keskendutakse vaatamise ja redigeerimise siltide Virtuaalmasinates panna.

Esmalt avage virtuaalse masina kaudu soovitud `Get-AzureRmVM` cmdlet-käsk.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Kui arvuti virtuaalne sisaldab juba sildid, siis kuvatakse kõik sildid oma ressursi.

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Kui soovite lisada silte PowerShelli kaudu, saate selle `Set-AzureRmResource` käsk. Pange tähele värskendamisel sildid läbi PowerShelli, sildid värskendatakse kogu. Seega kui lisate ühe sildi ressurss, mis on juba sildid, peate kaasata kõik sildid, mida soovite paigutada ressurss. Allpool on näide täiendavaid silte lisada ressursi PowerShelli cmdlet-käskude abil.

Esimene cmdlet-käsu määrab panna *MyTestVM* *$tags* muutuja, siltide abil soovitud `Get-AzureRmResource` ja `Tags` atribuut.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Teine käsk kuvab antud muutuja sildid.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Kolmas käsk lisab *$tags* muutuja on täiendavad silt. Märkus kasutamine on **+=** uue /-väärtuse paari *$tags* loendisse lisamiseks.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Neljas käsk määrab siltide määratletud *$tags* muutuja antud ressursi. Selles näites on MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Viienda käsk kuvatakse kõik sildid ressurss. *Nagu näete, nüüd määratletakse sildi koos *MyLocation* väärtusena.*

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

PowerShelli kaudu sildistamise kohta lisateabe saamiseks vaadake [Azure'i ressursi cmdlet-käsud][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Järgmised sammud

* Oma Azure ressursse sildistamise kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade][] ja [Korraldada oma Azure ressursse siltide abil][].
* Kuidas siltide abil saate hallata oma kasutamine Azure ressursse, leiate [Azure'i arve mõistmine][] ja [saada teadmisi oma Microsoft Azure'i ressursside tarbimine][].

[PowerShelli keskkonnas Azure'i ressursihaldur]: ../powershell-azure-resource-manager.md
[Azure'i ressursi cmdlet-käsud]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure'i ressursihaldur ülevaade]: ../azure-resource-manager/resource-group-overview.md
[Siltide abil saate korraldada oma Azure ressursse]: ../resource-group-using-tags.md
[Azure'i arve mõistmine]: ../billing/billing-understand-your-bill.md
[Saada ülevaate oma Microsoft Azure'i ressursside tarbimine]: ../billing-usage-rate-card-overview.md
