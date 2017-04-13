<properties
    pageTitle="Windowsi Virtuaalmasinates ülevaade | Microsoft Azure'i"
    description="Siit saate teada, loomise ja haldamise virtuaalmasinates Windows Azure kohta."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Virtuaalmasinates Windows Azure ülevaade

Azure'i Virtuaalmasinates (VM) on üks mitut tüüpi [nõudmisel, skaleeritav ressursside](../app-service-web/choose-web-site-cloud-service-vm.md) Azure pakub. Kui teil on vaja rohkem kontrolli IT-keskkond kui muud Valikud pakuvad valite tavaliselt VM. See artikkel annab teile teavet, mida peaks kaaluma enne loomist VM, kuidas luua selle ja kuidas seda hallata.

Azure'i VM-i kaudu saate Virtualization ilma osta ja säilitada füüsilise riistvara, mis töötab see. Siiski peate endiselt säilitada VM ülesannete, nt konfigureerimise, lappimine ja tarkvara, mis töötab see installimine.

Azure'i virtuaalmasinates saab kasutada mitmel viisil. Mõned näited:

- **Arendus ja testimine** – Azure VMs pakkumine on kiire ja lihtne viis luua arvutis teatud konfiguratsioone koodi ja testida rakenduse.
- **Rakenduste pilveteenuses** – Kuna nõudmisel rakenduse müügitehingu võib muutuda, võib mõttekas economic käitamist Azure VM. Maksate eest VMs, kui neid vajate ja sulgeda neid, kui te pole.
- **Laiendatud andmekeskuse** – virtuaalse võrgu Azure'i virtuaalmasinates saab ühendada hõlpsalt oma ettevõtte võrku.

Arvu VMs, mis kasutab rakenduse saab skaala üles ja välja ükskõik on nõutav teie vajadustele.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Mida on vaja mõelda enne loomise VM?

Alati on hulgaliselt [kujundamine](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) kui koostate välja rakenduse infrastruktuur Azure. VM aspektidega on oluline silmas pidada enne alustamist.

- Rakenduse ressursside nimed
- Ressursside talletamise asukoht
- VM suurus
- VMs, mille saab luua maksimaalne arv
- Operatsioonisüsteem, mida käitatakse VM
- Pärast käivitamist VM konfigureerimine
- Seotud ressursid, mis tuleb VM

### <a name="naming"></a>Nime andmise

Virtuaalse masina on määratud selle [nime](virtual-machines-windows-infrastructure-naming-guidelines.md) ja see on konfigureeritud osana operatsioonisüsteemi arvuti nimi. VM nimi võib olla kuni 15 märki.

Kui kasutate operatsioonisüsteemi ketta loomiseks Azure, arvuti nimi ja virtuaalse masina nimi on samad. Kui saate [üles laadida ja kasutada oma pilt](virtual-machines-windows-upload-image.md) , mis sisaldab eelnevalt konfigureeritud operatsioonisüsteem ja selle abil saate luua virtuaalse masina, nimed võivad olla erinevad. Soovitame kui saadate oma pildifaili, saate teha arvuti nimi operatsioonisüsteemi ja virtuaalse masina nimi sama.

### <a name="locations"></a>Asukohad

Kõik ressursse, mis on loodud Azure levitatakse üle mitme [piirkondades](https://azure.microsoft.com/regions/) kogu maailmas. Tavaliselt piirkonna nimetatakse **asukoht** VM loomisel. VM, saate määrata asukoha, virtuaalse kõvaketta talletamiseks.

Selles tabelis on mõned viisid, kuidas saate asukoht saadaolevate asukohtade loendit.

| Meetod | Kirjeldus |
| ------ | ----------- |
| Azure'i portaal | Valige asukoht loendist VM loomisel. |
| Azure'i PowerShelli | Kasutage käsku [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| REST API-GA | Kasutage toimingut [loendist asukohad](https://msdn.microsoft.com/library/dn790540.aspx) . |

### <a name="vm-size"></a>VM suurus

[Suurus](virtual-machines-windows-sizes.md) VM, mida kasutate määratakse töökoormus, mille soovite käivitada. Seejärel valige soovitud suurusega määratleb tegurid, näiteks töötlemise power, mälu ja salvestusruumi võimsus. Azure pakub laia suuruses, mis toetab mitut tüüpi kasutab.

Azure'i kulude mõne [tunni hind](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) on VM suurus ja operatsioonisüsteem. Osaline tundi Azure tasud ainult kasutatud minutite arv. Salvestusruumi hinnaga ja lisandu eraldi.

### <a name="vm-limits"></a>VM piirangud

Teie tellimus on vaikimisi [limiitide](../azure-subscription-service-limits.md) kohas, mis võivad mõjutada juurutamise palju vms oma projekti. Praeguse tellimuse kohta alusel on 20 VMs müüginäitajaid piirkonna kohta. Saate tõsta, esitades tugi Piletite, paluda suurendada.

### <a name="operating-system-disks-and-images"></a>Operatsioonisüsteemi ketast ja pildid

Virtuaalmasinates saate talletada oma operatsioonisüsteemi (OS) ja andmete [virtuaalse kõvaketta (VHDs)](virtual-machines-windows-about-disks-vhds.md) . Piltide, saate valida OS installimiseks kasutada ka VHDs. 

Azure'i pakub palju [turuplatsi pilte](https://azure.microsoft.com/marketplace/virtual-machines/) kasutada erinevate versioonide ja Windows Serveri opsüsteemid tüüpi. Turuplatsi piltide tuvastatakse pilt Publisheri, pakkumine, sku ja versioon (tavaliselt versioon on määratud Viimane). 

Selles tabelis on kuvatud mõned viisid, leiate teavet pilt.

| Meetod | Kirjeldus |
| ------ | ----------- |
| Azure'i portaal | Väärtused automaatselt määratud teie jaoks, kui valite pildi kasutamiseks. |
| Azure'i PowerShelli | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -asukoht "asukoht"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -asukoht "asukoht"-Publisher "publisherName"<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -asukoht "asukoht"-Publisher "publisherName"-"offerName" pakkumine |
| REST API-d | [Loendi pildi tootjad](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Pakub loendi pilt](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Loendi pildi SKU-de jaoks](https://msdn.microsoft.com/library/mt743701.aspx) |

Soovi korral saate üles [laadida ja oma pildi kasutamine](virtual-machines-windows-upload-image.md) ja kui teete, publisher nimi, pakkumise ja sku pole kasutatud.

### <a name="extensions"></a>Laiendid

VM [laiendid](virtual-machines-windows-extensions-features.md) anda oma VM lisavõimalused postituse juurutuse konfigureerimise ja automatiseeritud tööülesanded kaudu.

Need tavalised toimingud suunamist laiendid abil:

- **Kohandatud skriptide käitamiseks** – [Kohandatud skript laiend](virtual-machines-windows-extensions-customscript.md) abil saate konfigureerida töökoormus VM käitamist kui VM on ette valmistatud.
- **Deploy ja hallata konfiguratsioone** – [PowerShelli soovitud oleku konfiguratsioon (DSC) laiendamine](virtual-machines-windows-extensions-dsc-overview.md) aitab seadistada DSC klõpsake VM haldamiseks konfiguratsioone ja keskkonnas.
- **Diagnostika andmete kogumine** – [Azure diagnostika laiend](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) aitab teil konfigureerida VM koguda diagnostika andmeid, mida saate kasutada rakenduse seisundi jälgimine.

### <a name="related-resources"></a>Seotud ressursid

Selles tabelis ressursside kasutavad VM ja olemas või luua VM loomisel on vaja.

| Ressurss | Nõutav | Kirjeldus |
| -------- | -------- | ----------- |
| [Ressursirühm](../azure-resource-manager/resource-group-overview.md) | Jah | VM peavad sisaldama ressursirühma. |
| [Salvestusruumi konto](../storage/storage-create-storage-account.md) | Jah | VM peab salvestusruumi konto, mida soovite salvestada selle virtuaalse kõvaketta. |
| [Virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) | Jah | VM peab olema virtuaalse võrgu liige. |
| [Avaliku IP-aadress](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Ei | VM võib olla eemalt juurde määratud avaliku IP-aadress. |
| [Võrgu liidese](../virtual-network/virtual-network-network-interface-overview.md) | Jah | VM peab võrgu liidese võrgus suhelda. |
| [Andmete ketast](virtual-machines-windows-attach-disk-portal.md) | Ei | VM võivad hõlmata andmeid ketast salvestusruumi võimaluste laiendamiseks. |

## <a name="how-do-i-create-my-first-vm"></a>Kuidas luua oma esimese VM?

Teil on mitu valikut oma VM loomise kohta. Mida teha valik sõltub selle keskkonnas. 

Selles tabelis on ära toodud teavet, mis aitab teil alustada oma VM loomine.

| Meetod | Artikkel |
| ------ | ------- |
| Azure'i portaal | [Töötab Windows portaali loomine](virtual-machines-windows-hero-tutorial.md) |
| Mallid | [Windowsi virtuaalse masina ressursihaldur malli loomine](virtual-machines-windows-ps-template.md) |
| Azure'i PowerShelli | [Luua VM Windows PowerShelli abil](virtual-machines-windows-ps-create.md) |
| Kliendi SDK-d | [Azure'i ressursid abil C# juurutamine](virtual-machines-windows-csharp.md) |
| REST API-d | [Loomine ja värskendamine VM](https://msdn.microsoft.com/library/mt163591.aspx) |

Saate loodame, et see juhtub kunagi, kuid aeg-ajalt läheb midagi valesti. Kui olukord juhtub, vaadake [tõrkeotsing ressursihaldur juurutamise probleemid loomine Windows Azure virtuaalse masina](virtual-machines-windows-troubleshoot-deployment-new-vm.md)teave.

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Kuidas hallata VM, mis loodud?

VMs saab hallata Brauseripõhine portaali käsurea tööriistade toega skriptimine või otse API-de kaudu. Mõned tüüpilised haldamise toiminguid, mida võib teha saavad teavet VM, VM, hallata kättesaadavus ning varukoopiaid sisselogimist.

### <a name="get-information-about-a-vm"></a>VM kohta teabe saamine

Järgmises tabelis kuvatakse mõned viisid, kuidas saate teavet VM.

| Meetod | Kirjeldus |
| ------ | ----------- |
| Azure'i portaal | Menüü jaoturi klõpsake **Virtuaalmasinates** ja seejärel valige loendist VM. Enne VM, on teil juurdepääs ülevaate leiate säte väärtused ja mõõdikute jälgimine. |
| Azure'i PowerShelli | PowerShelli kaudu haldamise VMs kohta leiate teavet teemast [haldamine Azure'i Virtuaalmasinates ressursihaldur ja PowerShelli abil](virtual-machines-windows-ps-manage.md). |
| REST API-GA | Kasutage [teabe saamiseks VM](https://msdn.microsoft.com/library/mt163682.aspx) toiming VM kohta teabe saamiseks. |
| Kliendi SDK-d | Kasutamise C# VMs haldamise kohta leiate teemast [Azure ressursihaldur ja C# abil hallata Azure'i Virtuaalmasinates](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Logige VM

Kasutate Azure'i portaalis [Kaugtöölaua (RDP) seansi käivitamine](virtual-machines-windows-connect-logon.md)nuppu Ühenda. Asjad, mida saab valesti mõnikord minna, kui proovite kasutada kaugühenduse. Kui olukord juhtub, lugege teemat [tõrkeotsing kaugtöölaua ühendusi opsüsteemi Windows Azure'i virtuaalarvuti](virtual-machines-windows-troubleshoot-rdp-connection.md)abi teave.

### <a name="manage-availability"></a>Kättesaadavus haldamine

On oluline, et aru saada, kuidas [tagada kõrge kättesaadavus](virtual-machines-windows-manage-availability.md) rakenduse. Selle konfiguratsiooni hõlmab, luua mitme VMs tagamaks, et vähemalt üks töötab.

Selleks, et saada meie 99,95 VM teenuselepingu juurutamise, peate juurutada kahe või enama VMs töötab teie töökoormus sees on [kättesaadavus](virtual-machines-windows-infrastructure-availability-sets-guidelines.md). Selle konfiguratsiooni tagab oma VMs on jaotatud viga mitu domeenides ja peale hosts erinevate hooldustööd Windowsiga juurutatud. Täielik [Azure'i SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) selgitatakse Azure'i tagatud kättesaadavus kogu.
 
### <a name="back-up-the-vm"></a>VM varundamine
 
[Taastamise teenused vault](../backup/backup-introduction-to-azure-backup.md) kasutatakse andmete ja varad Azure varundus-ja Azure saidi taastamine. Saate kasutada taastamise teenused vault [juurutada](../backup/backup-azure-vms-automation.md)ja hallata varukoopiate ressursihaldur juurutatud vms PowerShelli kaudu. 

## <a name="next-steps"></a>Järgmised sammud

- Kui teie eesmärk on töötada Linux VMs, vaadake [Azure ja Linux](virtual-machines-linux-azure-overview.md).
- Lisateave ümber häälestada oma taristu [näide Azure'i taristu kiirtutvustus](virtual-machines-windows-infrastructure-example.md)suuniste kohta.
- Veenduge, et järgite [Head tavad, kus töötab Windows Azure VM](virtual-machines-windows-guidance-compute-single-vm.md).


