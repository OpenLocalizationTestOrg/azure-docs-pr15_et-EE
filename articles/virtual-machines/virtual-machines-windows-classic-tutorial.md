<properties
    pageTitle="Luua VM klassikaline portaalis | Microsoft Azure'i"
    description="Luua virtuaalse masina Windows Azure'i klassikaline portaalis."
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
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Töötab Windows Azure'i klassikaline portaali loomine

> [AZURE.SELECTOR]
- [Azure'i klassikaline portaal](virtual-machines-windows-classic-tutorial.md)
- [PowerShelli: Klassikaline juurutamine](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [nende toimingute abil ressursihaldur juurutamise mudeli](virtual-machines-windows-hero-tutorial.md) **uue Azure portaali**. 

Selle õpetuse näitab, kuidas luua Azure'i virtuaalarvuti (VM) opsüsteemi Windows Azure'i klassikaline portaalis. Kasutame Windows Server pildi näide, kuid see on ainult üks Azure pakub palju pilte. Pange tähele, et pilt valikutele sõltuvad teie tellimus. Näiteks võib olla saadaval MSDN tellijad Windowsi töölaua pildid.

Selles jaotises kirjeldatakse, kuidas kasutada **Galeriist** valik Azure klassikaline portaalis virtuaalse masina loomiseks. See suvand pakub lisavalikute konfiguratsiooni, kui **Kiiresti luua** võimalus. Näiteks, kui soovite liituda virtuaalse masina virtual võrku, peate suvandi **Galeriist** .

Saate luua ka [oma piltide](virtual-machines-windows-classic-createupload-vhd.md)kasutamise VMs. See ja teiste meetodite kohta leiate jaotisest [võimalust luua Windowsi virtuaalse masina](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Video kiirtutvustus

Siin on lühiülevaade selle õpetuse.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Virtuaalse masina loomine

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [luua ressursihaldur juurutamise näidise VM](virtual-machines-windows-hero-tutorial.md) uue Azure'i portaalis. 

- Logige sisse virtuaalse masina. Lisateabe saamiseks vaadake [Windows Server töötab sisse logida](virtual-machines-windows-classic-connect-logon.md).

- Manusta kettal andmete talletamiseks. Saate lisada tühja ketast nii ketast, mis sisaldavad andmeid. Juhised leiate teemast [meilisõnumile Windowsi virtuaalse masina loodud klassikaline juurutamise mudeli andmete ketast](virtual-machines-windows-classic-attach-disk.md).
