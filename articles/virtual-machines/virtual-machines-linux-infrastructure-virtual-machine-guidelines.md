<properties
    pageTitle="Linux Virtuaalmasinates juhised | Microsoft Azure'i"
    description="Lisateavet ja rakendamist suuniseid üheks Azure'i virtuaalmasinates Linuxi kasutamise kohta"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Virtuaalmasinates juhised

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

See artikkel keskendub mõistmine nõutud kavandamise toimingute loomise ja haldamise virtuaalmasinates (VM) oma Azure keskkonnas.

## <a name="implementation-guidelines-for-vms"></a>Rakendamise juhised VMs
Otsuseid:

- Kui palju VMs vajate oma erinevate rakenduse astme ja komponentide oma taristu?
- Mis CPU ja mälu ressursse tuleb iga VM ja millised on salvestusruumi nõuded?

Ülesanded:

- Määratleda töökoormus rakenduse ja ressursside VMs nõudmine.
- Ressursi nõudmistele vastav VM suurus ja salvestusruumi tüüpi iga VM joondada.
- Määratleda erinevate ja komponentide oma taristu ressursi rühm.
- Määratleda oma VM nimetamistava.
- Saate luua oma VMs Azure'i CLI, web portaali, või ressursihaldur mallide abil.

## <a name="virtual-machines"></a>Virtuaalmasinates

Üks põhikomponentide oma Azure keskkonnas on tõenäoliselt VMs. See on, kui käivitate rakendused, andmebaasid, autentimisteenuste jne.

See on oluline mõista [VM erineva suurusega](virtual-machines-linux-sizes.md) õigesti suuruse muutmiseks keskkonna jõudlus ja maksumus perspektiivi. Kui teie VMs pole piisavalt protsessorituuma või mälu, kannatab sõltumata sellest, kuidas see on loodud ja väljatöötatud rakenduse jõudlust. Kui otsustate, mille suurus VM oma infrastruktuuri iga komponendi jaoks, lugege läbi soovitatud töökoormus iga VM seeria alguspunktina. Saate pärast juurutamise [VM suurust muuta](virtual-machines-linux-change-vm-size.md) .

Salvestusruumi on oluline roll VM jõudlust. Saate kõrge I/O töökoormus ja tippväärtus jõudluse, mis kasutavad SSD ketast Standard salvestusruumi, mis kasutab tavalise valmistamata ketast, või Premium mälu. Kui edasiarendus VM seal on maksumus salvestusruumi Keskmine valimisel peaksite arvesse võtma. [Salvestusruumi taristu juhiseid artiklis](virtual-machines-linux-infrastructure-storage-solutions-guidelines.md) mõistmaks, kuidas kujundada hoidmise oma vms optimaalse jõudluse kohta saate lugeda.


## <a name="resource-groups"></a>Ressursi rühmad
Näiteks VMs on loogiliselt rühmitatud hõlpsamaks haldamine ja hooldus [Azure'i ressursi rühmade](../azure-resource-manager/resource-group-overview.md)kasutamine. Ressursi rühmade abil saate luua, hallata ja jälgida ressursse, mis moodustavad rakenduse. Saate rakendada ka [Rollipõhine pääsu juhtelementide](../active-directory/role-based-access-control-what-is.md) juurdepääsu teistele ainult need nõua ressursse oma meeskonna sees. Aega oma ressursi rühmad ja rollimääranguid kavandada. On tegelikult kujundada ja rakendada ressursi rühmad, kindlasti [Ressursi rühmade juhised artiklist](virtual-machines-linux-infrastructure-resource-groups-guidelines.md) kuidas kõige paremini mõista teie VMs rajamise erinevad meetodid.


## <a name="templates"></a>Mallid 
Saate koostada määratletud deklaratiivseid JSON faile luua oma VMs mallid. Mallid tavaliselt ka koostada nõutav salvestusruumi, võrgunduse, võrgu liidesed, IP tegelemine, jne koos ise VMs. Mallide abil saate luua järjepidevate, korratavad keskkonnas arengu ja testimine eesmärgil tootmise keskkondades hõlpsalt kopeerida ja vastupidi. Lugege lisateavet [koostamise ja mallide kasutamine](../azure-resource-manager/resource-group-overview.md#template-deployment) mõista, kuidas saate neid kasutada, loomine ja juurutamine teie VMs kohta.


## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 