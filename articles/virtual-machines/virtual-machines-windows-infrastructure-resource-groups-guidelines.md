<properties
    pageTitle="Ressursi pakuvad juhiseid | Microsoft Azure'i"
    description="Teavet ja rakendamist suuniseid Azure taristu teenused ressursirühma kasutamise kohta."
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

# <a name="azure-resource-group-guidelines"></a>Azure'i ressursi rühma juhised

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

See artikkel keskendub loogiliselt keskkonna rajamise ja rühma kõik komponendid ressursi rühmade mõistmine.


## <a name="implementation-guidelines-for-resource-groups"></a>Ressursi rühmade rakendamise juhised

Otsuseid:

- Kas kavatsete rajamise ressursirühma taristu põhikomponentide, või täieliku rakenduse juurutamine?
- Kas vajate juurdepääsu piiramiseks ressursi rühmade Rollipõhine juurdepääsu juhtelementide kasutamine

Ülesanded:

- Määratlemine, mis core taristu komponendid ja ressursside rühma on vaja pühendunud.
- Vaadake kuidas rakendada järjepidevate, korratavad juurutuste ressursihaldur mallid.
- Mida kasutaja juurdepääsu rollid kontroll ressursside rühmade jaoks vajaliku määratleda.
- Saate luua ressursi rühmade abil oma nimetamistava määramine. Saate kasutada Azure PowerShelli või portaali.


## <a name="resource-groups"></a>Ressursi rühmad

Azure'i rühmitate loogiliselt seotud ressursid, nt salvestusruumi kontod, virtuaalse võrgu ja juurutada, hallata ja säilitada need ühtse tervikuna virtuaalmasinates (VMs). Seda moodust hõlbustab juurutada rakendused, säilitades seotud ressursid koos halduse perspektiivi või teised sellele rühmale ressursside juurdepääsu andmiseks. Lugege ressursirühma põhjalikuma ülevaate [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).

Ressursi rühmadele on võimalus mallide kasutamine keskkonna rajamise. Mall on lihtsalt JSON faili, mis kinnitab, salvestusruumi, networking, ja arvutada ressursid. Samuti saate määratleda, mis tahes seotud kohandatud skriptid või konfiguratsioone rakendada. Nende mallide abil luua oma rakenduste järjepidevate, korratavad juurutuste. See lähenemine on lihtne arendatakse keskkonna rajamise ja seejärel sama malli abil saate luua tootmise juurutus, või vastupidi. Mallide kasutamine paremaks mõistmiseks lugege [kiirtutvustus Mall](../resource-manager-template-walkthrough.md) , mis juhendab teid iga toimingu hoone välja malli.

On kaks erinevat võimalusi, ressursside levirühmadega keskkonna kujundamisel.

- Ressursi rühmad iga rakenduse juurutamine, mis ühendab salvestusruumi kontod, virtuaalse võrgu ja alamvõrku, VMs laadida soolise jne.
- Sisaldavad core virtuaalse võrgunduse ja alamvõrku või salvestusruumi kontod tsentraliseeritud ressursi rühmad. Rakenduste on seejärel oma ressursi rühmad, mis sisaldavad ainult VMs koormus soolise, võrgu liidesed jne.

Nagu te skaala välja loomise tsentraliseeritud ressursi rühmade virtuaalse võrgunduse ja alamvõrku on hõlpsam koostada asutusesiseses vahelist võrguühendust hübriid ühenduvuse võimalusi. Alternatiivne lähenemine on iga rakenduse soovite oma virtuaalse võrk, mis nõuab konfigureerimine ja hooldus.  [Rollipõhine juurdepääsu kontroll](../active-directory/role-based-access-control-what-is.md) Varundustöö võimalda juurdepääsu ressursi rühmad. Tootmise rakendused, saate määrata kasutajatele, kes pääsevad nende ressursside või core taristu ressursid saate piirata ainult taristu insenerid nendega töötada. Oma rakenduse omanikud pääsevad oma ressursirühm ja mitte core Azure infrastruktuuri keskkonna rakenduse komponendid. Keskkonna kujundamisel, kaaluge kasutajatele, et kasutamiseks on vaja juurdepääsu ressursside ja kujundamine vastavalt oma ressursside rühma. 


## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 