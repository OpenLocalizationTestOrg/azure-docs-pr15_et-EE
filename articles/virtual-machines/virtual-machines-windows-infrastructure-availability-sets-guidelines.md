<properties
    pageTitle="Kättesaadavus seadmine juhised | Microsoft Azure'i"
    description="Teavet ja rakendamist suuniseid Azure taristu teenused kättesaadavus komplekti kasutamise kohta."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Kättesaadavus määrab juhised

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

See artikkel keskendub kättesaadavus komplektide tagada rakenduste kindlasti juurdepääsetav kavandatud või planeerimata sündmuste kavandamise nõutud toimingute mõistmine.

## <a name="implementation-guidelines-for-availability-sets"></a>Rakendamise juhised kättesaadavus komplektid

Otsuseid:

- Mitu-saadavus komplekti jaoks vaja läheb erinevad rollid ja astme oma rakenduse infrastruktuuri?

Ülesanded:

- Iga rakenduse taseme vajate VMs arvu määratlemine.
- Kohandada viga või värskendada domeenide rakenduse kasutamiseks vajalikkuse kindlaksmääramine.
- Nõutav kättesaadavus komplekti, kasutades nimetamistava ja mis määratlevad VMs asu neid. VM võivad asuda ainult ühe kättesaadavus määramine. 

## <a name="availability-sets"></a>Kättesaadavus komplektid

Azure, paigutatakse virtuaalmasinates (VM) loogiline rühmitamine, nimetatakse ka kättesaadavus määramine. VMs loomisel on kättesaadavus määratud Azure'i platvormi jaotab need paigutuse aluseks infrastruktuuri üle. Peaks olema kavandatud hooldustööde sündmuse Azure'i platvormi või mõne aluseks oleva riistvara / taristu viga, kasutamine kättesaadavus seab tagab, et vähemalt üks VM jääb töötava.

Hea tava, tuleks rakenduste ei asu ühe VM. Kättesaadavus komplekti, mis sisaldab ühe VM ei saada mis tahes kaitse kavandatud või planeerimata sündmuste sees Azure'i platvormi kaudu. [Azure'i SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) nõuab saadavus määramine lubada VMs jaotamise aluseks infrastruktuuri üle kahe või enama VMs.

Aluseks infrastruktuuri Azure on jagatud domeenid ja viga domeenide loendi värskendamiseks. Need domeenid on määratletud, mis hosts levinud update tsükli või sarnane füüsilise infrastruktuuri ühiskasutus, nt power ja võrgunduse jagamine. Azure'i jaotab automaatselt teie VMs seadmine domeenides säilitada kättesaadavus ja tõrketaluvust saadavus. Sõltuvalt rakenduse ja VMs kättesaadavus on määratud arv suurust, saate reguleerida arv domeene, mida soovite kasutada. Lugege lisateavet [haldamise kättesaadavuse ja värskendamise ja viga domeenide kasutamise](virtual-machines-windows-manage-availability.md)kohta.

Oma rakenduse taristu kujundamisel tuleb ka planeerida astme rakendus, mida kasutate. Rühma VMs, mis on sama eesmärk sisse-saadavus määrab, näiteks seada oma ees vms, kus töötab IIS-i olemasolu. Luua eraldi kättesaadavus, määrata oma tagaandmebaas vms, kus töötab SQL Server. Eesmärk on tagada, et iga osa teie taotlus on kaitstud mõne kättesaadavus määramine ja vähemalt ühe korra eksemplari alati jääb käivitatud.

Koormus soolise saab kasutada iga rakenduse kiht töö kõrval on kättesaadavus määramine ja veenduge, et liikluse marsruuditakse alati käivitatud eksemplari ette. Ilma laadi koormusetasakaalustusteenuse, võib teie VMs Jätka kogu planeerimata ja kavandatud hooldustöid sündmusi, kuid ei pruugi teie lõppkasutajad lahendamiseks ette võtta, kui esmane VM pole saadaval.


## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 