<properties
   pageTitle="Kustutamine on Azure kobar ja selle ressursside | Microsoft Azure'i"
   description="Siit saate teada, kuidas täielikult kustutada teenuse struktuuri klaster ressursirühm, mis sisaldab klaster kustutamine või kustutades valikuliselt ressursid."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Teenuse struktuuri kobar Azure ja ressursse, mis kasutab kustutamine

Teenuse struktuuri kobar koosneb palju muud Azure ressursid Lisaks kobar ressursi ise. Nii teenuse struktuuri kobar täielikult kustutada ka peate kustutama kõik koosneb ressursid.
On teil kaks võimalust: Kas kustutada ressursirühm, mis on klaster (mis kustutab kobar ressurss ja muud ressursid, ressursside jaotises) või spetsiaalselt kustutada kobar ressurss ja see on seotud ressursid (kuid mitte ressursi jaotises muud ressursid).

>[AZURE.NOTE] Kobar ressursi **ei** kustutamisel kõik muud ressursid, mis koosneb klaster teenuse struktuuri.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Kustutada kogu ressursirühm (RG), mis on teenuse struktuuri kobar

See on kõige lihtsam viis tagada, et kustutate kõik klaster, sh ressursirühma seotud ressursid. Saate kustutada ressursirühma PowerShelli kaudu või Azure portaali kaudu. Kui teie ressursirühm on ressursse, mis pole seotud teenuse struktuuri kobar, saate kustutada teatud ressursid.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Azure'i PowerShelli kaudu ressursirühma kustutamine

Saate kustutada ka ressursirühma käivitades järgmised Azure PowerShelli cmdlet-käsud. Veenduge, et Azure'i PowerShelli 1.0 või suurem on teie arvutisse installitud. Kui te pole seda teinud enne, järgige teemas kirjeldatud juhised [Kuidas installida ja konfigureerida Azure PowerShelli.](../powershell-install-configure.md)

Avage PowerShelli aken ja käivitage järgmised PS cmdlet-käsud:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Kuvatakse viip, klõpsake kustutamise kinnitamiseks, kui te ei kasuta funktsiooni *-Jõusta* suvand. Kinnituse kustutatakse selle RG ja see sisaldab ressursse.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Azure'i portaalis ressursi rühma kustutamine  

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Liikuge teenuse struktuuri kobar, mille soovite kustutada.
3. Klõpsake lehel kobar Essentialsi ressursi rühma nime.
4. Kuvatakse tabeliveergude **Ressursi rühma Essentialsi** lehe.
5. Klõpsake nuppu **Kustuta**.
6. Sellel lehel ressursirühma kustutamise lõpuleviimiseks järgige juhiseid.

![Ressursirühm kustutamine][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Kustutage kobar ressursside ja ressursse, mis kasutab, kuid mitte ressursi jaotises muud ressursid

Kui teie ressursirühm on ainult ressursse, mis on seotud teenuse struktuuri kobar, mille soovite kustutada, siis on lihtsam kustutada kogu ressursirühma. Kui soovite kustutada valikuliselt ressursirühma ressursside ükshaaval, siis tehke järgmist.

Kui olete juurutanud klaster portaalis või teenuse struktuuri ressursihaldur malli galeriist malli abil, on ressursse, mis kasutab klaster järgmised kaks siltide kodeeritud. Nende abil saate otsustada, millised ressursid, mille soovite kustutada.

***Sildistamine #1:*** Klahv = clusterName väärtus = klaster nimi

***Sildistamine #2:*** Klahv = resourceName väärtus = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Teatud ressursid Azure'i portaalis kustutamine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Liikuge teenuse struktuuri kobar, mille soovite kustutada.
3. Avage Essentialsi enne **Kõik sätted** .
4. Klõpsake **siltide** **Ressursside haldamine** jaotises sätted tera.
5. Klõpsake ühte **Sildid** siltide tera kõik selle sildiga ressursside loendi.

    ![Ressursi Sildid][ResourceTags]

6. Kui olete sildistatud ressursside loendi, klõpsake ressursid ja kustutage need.

    ![Sildistatud ressursid][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Kustutage ressursse, mis on Azure PowerShelli abil

Saate kustutada ühe kaupa ressursse käivitades järgmised Azure PowerShelli cmdlet-käsud. Veenduge, et Azure'i PowerShelli 1.0 või suurem on teie arvutisse installitud. Kui te pole seda teinud enne, järgige teemas kirjeldatud juhised [Kuidas installida ja konfigureerida Azure PowerShelli.](../powershell-install-configure.md)

Avage PowerShelli aken ja käivitage järgmised PS cmdlet-käsud:

```powershell
Login-AzureRmAccount
```
Ressursid soovite kustutada, käivitage järgmised:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Ressursi kobar kustutamiseks käivitage järgmised:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Järgmised sammud
Lugege ka Lisateavet üleminekut klaster ja teenuste eraldamine järgmist:

- [Lisateavet kobar uuendamine](service-fabric-cluster-upgrade.md)
- [Lisateavet eraldamine skaala stateful teenused](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
