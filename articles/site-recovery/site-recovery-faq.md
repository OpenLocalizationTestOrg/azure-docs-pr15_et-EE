<properties
    pageTitle="Azure'i saidi taastamine: Korduma kippuvad küsimused | Microsoft Azure'i"
    description="Selles artiklis käsitletakse populaarsed küsimused Azure saidi taastamise kohta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure'i saidi taastamine: Korduma kippuvad küsimused (KKK)

See artikkel sisaldab korduma kippuvaid küsimusi Azure saidi taastamise kohta. Kui teil on küsimusi pärast selle artikli lugemist, postitage need [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Üldine

### <a name="what-does-site-recovery-do"></a>Mida teeb saidi taastamine?

Saidi taastamine aitab äri järjepidevus ja avariijärgne taastamine (BCDR) strateegia kavandamine, orkesteroinnin ja dispersioonanalüüs kohapealse virtuaalmasinates ja füüsilise serveri Azure'i või sekundaarne andmekeskuse automatiseerimine. [Lisateavet leiate teemast](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Mida saate saidi taastamise kaitsta?

- **Hyper-V virtuaalmasinates**: saidi taastamine saate kaitsta mis tahes töökoormus töötab Hyper-V VM.
- **Füüsilise serveri**: saidi taastamine saate kaitsta füüsilise serveri operatsioonisüsteemi Windows või Linux.
- **VMware virtuaalmasinates**: saidi taastamine saate kaitsta mis tahes töökoormus töötab Vmware'i VM.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Kas saidi taastamine toetab Azure ressursihaldur mudeli?

Lisaks saidi taastamine Azure'i klassikaline portaalis, on saidi taastamine Azure'i portaalis toega ressursihaldur jaoks. Enamik juurutamise stsenaariumid on Azure saidi taastamine portaalis sujuv juurutamise kogemus ja saate ise VMs ja füüsilise serveri klassikaline salvestusruumi või ressursihaldur salvestusruumi. Siin on toetatud juurutuste.

- [Paljundada VMware VMs või füüsilise serveri Azure'i Azure portaalis](site-recovery-vmware-to-azure.md)
- [Paljundada Hyper-V VMs VMM pilved Azure'i Azure portaalis](site-recovery-vmm-to-azure.md)
- [Paljundada Azure'i portaalis Azure Hyper-V VMs (ilma VMM)](site-recovery-hyper-v-site-to-azure.md)
- [Paljundada Hyper-V VMs VMM pilved saidi Azure portaalis](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Mida pean Hyper-v korraldab dispersioonanalüüs saidi taastamine?

Hyper-V hosti server vajalik sõltub juurutamise stsenaariumi. Vaadake Hyper-V eeltingimused:

- [Imitatsiooniga Azure Hyper-V VMs (ilma VMM)](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Imitatsiooniga Azure Hyper-V VMs (koos VMM)](site-recovery-vmm-to-azure.md#before-you-start)
- [Imitatsiooniga Hyper-V VMs teisene andmekeskusega](site-recovery-vmm-to-vmm.md#before-you-start)

- Kui te olete imitatsiooniga teisene andmekeskusesse, lugege artiklit [külaline toetatud operatsioonisüsteemide Hyper-V vms](https://technet.microsoft.com/library/mt126277.aspx).
- Kui te olete imitatsiooniga Azure'i, saidi taastamine toetab kõiki külaline operatsioonisüsteemide jaoks ei [toeta Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kaitsta VMs kui Hyper-V töötab operatsioonisüsteem klient?

Ei, peab asuma VMs Hyper-V hosti server, mis on toetatud Windows Serveri arvutisse. Kui teil on vaja kaitsta klientarvuti võib korrata seda füüsilise seadme [Azure'i](site-recovery-vmware-to-azure.md) või [sekundaarne andmekeskuse](site-recovery-vmware-to-vmware.md).


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Millist töökoormus saate saidi taastamine kaitsta?

Saidi taastamise abil kaitsta toetatud VM või füüsilise server töötab enamik töökoormus. Saidi taastamine toetab rakenduse arvestada dispersioonanalüüs, nii, et rakenduste nutikad oleku taastada. See integreerub Microsoft rakendusi nagu SharePointi, Exchange'i, Dynamics, SQL serveri ja Active Directory ja tihedat koostööd ees müüjad, sh Oracle, SAP-i, IBM ja punane rolli. [Lisateavet leiate teemast](site-recovery-workload.md) töökoormus kaitsmise kohta.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V hosts vaja olema VMM pilved?

Kui soovite ise teisene andmekeskusesse, siis Hyper-V VMs peab olema majutab Hyper-V serverid asuvad VMM pilve. Kui soovite ise Azure, saate VMs Hyper-V hosti serverites koos või ilma VMM pilved korrata. [Lisateavet vt](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Saidi taastamine VMM abil saate juurutada kui mul on ainult üks VMM server?

Jah. Saate teha ise VMs Hyper-V serverid Azure VMM pilves, või saaksite korrata VMM parkimise sama serveri vahel. Kohapealse kohapealse kopeerimise abil, soovitame teil on VMM server esmaseid ja teiseseid saitidel.  [Lisateave](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Milliseid füüsilise serveri kaitsta?

Saate ise füüsilise serverites, kus töötab Windows ja Linux, Azure'i või saidi. [Vaadake, kuidas](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) operatsioonisüsteemi nõuded.  Kas te olete imitatsiooniga füüsilise serveri Azure'i või saidi kehtivad samad tingimused.

Pange tähele, et füüsilise serveri käivitavad nimega VMs Azure kui oma kohapealses serveris läheb alla. Failback kohapealse füüsilise server pole praegu toetatud, kuid te ei pruugi tagasi Hyper-V või VMware töötab.


### <a name="what-vmware-vms-can-i-protect"></a>Millist VMware VMs kaitsta?

VMware VMs kaitsmiseks peate vSphere hypervisor ja virtuaalmasinates töötab VMware tööriistad. Soovitame, et teil on VMware vCenter server on hypervisors haldamiseks. [Lisateavet leiate teemast](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) imitatsiooniga VMware serverite ja VMs Azure'i või saidi jaoks täpse nõuete kohta.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Saate hallata oma saidi taastamine harukontorite avariitaastet?

Jah. Kui korraldab kopeerimise ja Tõrkesiirde teie harukontorite saidi taastamise abil saate ühendatud korraldamise ja kõik haru office töökoormus vaates ühes keskses kohas. Saate failovers käivitada ja hallata avariitaastet kõigi kaudu oma peakontor, ilma külastamise haru.

## <a name="security"></a>Turvalisus

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Andmete kopeerimine saadetakse saidi taastamise teenust?

Ei, saidi taastamine ei intercept kopeeritud andmed ja ei sisalda teavet, mis töötab teie virtuaalmasinates või füüsilise serveri.
Andmete kopeerimine vahetavad kohapealse Hyper-V hosts, VMware hypervisors, või füüsilise serveri ja Azure salvestusruumi või sekundaarne saidile. Saidi taastamine ei ole võimalik intercept andmeid. Ainult metaandmete vaja korraldab kopeerimise ja Tõrkesiirde saadetakse saidi taastamise teenust.

Saidi taastamine on ISO 27001: 2013, 27018, HIPAA DPA sertifitseeritud ja tegeleb SOC2 ja FedRAMP TORKA.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Vastavuse huvides isegi meie kohapealse metaandmete peab jääma geograafilised piirkonna. Saate saidi taastamine aidake meil?

Jah. Saidi taastamise hoidla loomisel piirkonnas me tagada, et kõik metaandmete, mis tuleb lubada ja korraldab kopeerimise ja Tõrkesiirde jääb piirkonnas kasutaja geograafilise äärist.

### <a name="does-site-recovery-encrypt-replication"></a>Kas saidi taastamine krüptida dispersioonanalüüs?

Virtuaalmasinates ja füüsilise serveri, imitatsiooniga kohapealse saitide krüptimise transport vahel on toetatud. Virtuaalmasinates ja füüsilise serveri imitatsiooniga Azure'i, nii krüptimise transport ja krüptimise-veebisaidil-ülejäänud (Azure'i) on toetatud.


## <a name="replication"></a>Dispersioonanalüüs


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Kas mõni eeltingimuste imitatsiooniga Azure'i virtuaalmasinates?

Soovite paljundada Azure'i virtuaalmasinates peaksid vastama [Azure nõuetele](site-recovery-best-practices.md#virtual-machines).

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Saate ma ise Azure'i virtuaalmasinates Hyper-V loomine 2?

Jah. Saidi taastamine teisendab loomine 2 genereerimine 1 Tõrkesiirde ajal. Veebisaidil failback masina teisendatakse tagasi loomine 2. [Lisateavet vt](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Kui ma ise Azure kuidas maksta Azure'i vms?

Ajal tavaline kopeerimine, andmed kopeeritakse geograafilise liigne Azure storage ja te ei pea tasuma mis tahes Azure'i IaaS virtuaalse masina, pakkudes oluline eelis. Tõrkesiirde käivitamisel Azure saidi taastamine loob automaatselt IaaS Azure'i virtuaalmasinates ja pärast, et teile esitatakse arve Arvuta ressursid, mida saate kasutada Azure.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Kas on olemas ka SDK automatiseerimiseks ASR töövoo saab kasutada?

Jah. Saidi taastamine töövoogude Rest API-ga, PowerShelli või Azure SDK automatiseerida. Praegu toetatud stsenaariumid juurutamine saidi taastamine PowerShelli kaudu:

- [Paljundada Hyper-V VMs VMMs pilved Azure PowerShelli klassikaline](site-recovery-deploy-with-powershell.md)
- [Ise Hyper-V VMs VMMs pilved, et Azure'i PowerShelli ressursihaldur](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Paljundada Hyper-V VMs ilma VMM Azure PowerShelli klassikaline](site-recovery-hyper-v-site-to-azure-classic.md)
- [Azure'i PowerShelli ressursihaldur Hyper-V VMs ilma VMM paljundada](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Kui ma ise Azure mis tüüpi kontot salvestusruumi vaja?

- **Azure'i klassikaline portaali**: kui juurutamist saidi taastamine Azure'i klassikaline portaalis, peate mõne [standardse geograafilise liigne salvestusruumi konto](../storage/storage-redundancy.md#geo-redundant-storage). Premium salvestusruumi pole praegu toetatud. Konto peab olema sama piirkonna saidi taastamise hoidla.

- **Azure'i portaalis**: kui juurutamist saidi taastamine Azure'i portaalis, peate konto LRS või GRS salvestusruumi. Soovitame GRS nii, et andmed on olles piirkondliku katkestuste ilmnemisel või kui esmane piirkond ei saa taastada. Konto peab olema sama piirkonna vault taastamise teenused. Premium mälu on toetatud ainult siis, kui te olete imitatsiooniga VMware VMs või füüsilise serveri.

### <a name="how-often-can-i-replicate-data"></a>Kui sageli ma ise andmeid?

- **Hyper-v** Hyper-V VMs saab paljundada iga 30 sekundi järel, 5 või 15 minutit. Kui olete häälestanud SAN dispersioonanalüüs on sünkroonse kopeerimine.
- **VMware ja füüsilise serverid:** Dispersioonanalüüs sagedus pole oluline. Kopeerimine on pidev.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Saate pikendada olemasoleva taastamine saidilt kopeerimine teise ja kolmanda taseme saidile?

Laiendatud või aheldatud dispersioonanalüüs ei toetata. See funktsioon [tagasiside](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)Foorum koosolekukutse.


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Teha ka ühenduseta kopeerimisel ma ise Azure esimest korda?

See ei toetata. See funktsioon [tagasiside Foorum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)koosolekukutse.


### <a name="can-i-exclude-specific-disks-from-replication"></a>Teatud ketast saate välistamine dispersioonanalüüs?

See on toetatud, kui olete [imitatsiooniga VMware VMs ja füüsilise serveri](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) Azure, Azure portaalis.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Saate ma ise virtuaalmasinates dünaamiline ketast?

Dünaamiliste ketast on toetatud, kui Hyper-V virtuaalmasinates imitatsiooniga. Need on toetatud ka siis, kui imitatsiooniga VMware VMs ja füüsilise masinad Azure. Operatsioonisüsteemi ketas peab olema lihtne ketas.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Saate lisada olemasolevasse rühma kopeerimine uue masina?

Uued masinad lisamine dispersioonanalüüs rühmi on toetatud. Selleks valige dispersioonanalüüs rühm (alates 'Paljundatud üksuste' blade) ja paremklõpsake Valige kontekstimenüü rühma kopeerimine ja valige vastav variant.

![Dispersioonanalüüs rühma lisamine](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Saate ma throttle läbilaskevõimet eraldatud Hyper-V kopeerimise liikluse jaoks?

Jah. Lugege lisateavet kohta pidurdamise läbilaskevõime juurutamise artiklitest:

- [Valmisoleku kavandamine imitatsiooniga VMware VMs ja füüsilise serveri jaoks](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Võimsus imitatsiooniga Hyper-V VMs VMM pilved kavandamine](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Võimsus imitatsiooniga Hyper-V VMs ilma VMM kavandamine](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Tõrkesiirde


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Kui ma suuda üle Azure, kuidas ma pääsen Azure'i virtuaalmasinates pärast Tõrkesiirde?

Pääsete Azure VMs turvalist Interneti-ühenduse kaudu, -saidilt VPN üle või üle Azure'i ExpressRoute. Peate ette valmistada mitmeid asju, et ühendada. Lugege rohkem.

- [Pärast Tõrkesiirde VMware VMs või füüsilise serveri Azure VMs ühendamine](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Pärast Tõrkesiirde Hyper-V vms VMM pilved Azure VMs ühendamine](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Pärast Tõrkesiirde Hyper-V vms ilma VMM Azure VMs ühendamine](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Kui ma ei saa üle Azure, kuidas Azure'i veenduge, et minu andmetel on olles?

Azure'i on mõeldud paindlikkuse. Saidi taastamine juba insenerirajatis Tõrkesiirde teisene Azure andmekeskusesse Azure'i SLA vastavalt vajadusel. Sel juhul me veenduge, et teie metaandmete ja võlvid jäävad samasse geograafilist piirkonda oma vault valitud.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Kui olen imitatsiooniga vahel kahes andmekeskuses mis juhtub, kui mu esmane andmekeskuse kogemusi mõne ootamatu katkestuste?

Võite käivitada planeerimata Tõrkesiirde saidi kaudu. Saidi taastamine ei pea ühenduvuse esmane saidilt on tõrkesiire sooritamiseks.

### <a name="is-failover-automatic"></a>On Tõrkesiirde automaatselt?

Tõrkesiirde pole automaatne. Saate algatada ühe klõpsuga portaalis failovers või [Saidi taastamise PowerShelli](https://msdn.microsoft.com/library/dn850420.aspx) abil saate käivitada Tõrkesiirde. Tagasi ei on lihtne toiming saidi taastamine portaalis.

Automatiseerimiseks saate võib kasutada kohapealse Orchestrator või Toiminguhalduri virtuaalse masina tõrke tuvastamiseks ja seejärel käivitamine on tõrkesiire SDK abil.

- [Lisateavet vt](site-recovery-create-recovery-plans.md) taastamise kohta.
- [Lisateavet vt](site-recovery-failover.md) Tõrkesiirde kohta.
- [Lisateavet](site-recovery-failback-azure-to-vmware.md) vastasel tagasi VMware VMs ja füüsilise serveri


## <a name="service-providers"></a>Teenuse pakkujad


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ma olen teenusepakkuja. Kas saidi taastamise töö sihtotstarbeline ja ühiskasutuses taristu mudelite?

Jah, saidi taastamine toetab nii sihtotstarbeline ja ühiskasutuses taristu mudelid.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Teenusepakkuja on minu saidi taastamise teenuse ühiskasutusse rentniku ID?

Ei. Rentniku identiteedi jääb anonüümse. Rentnikud teie ei pea juurdepääs saidi taastamine portaali. Ainult teenuse pakkuja administraatori suhtleb portaali.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Rentniku rakenduse andmete kunagi läheb Azure?

Kui imitatsiooniga teenuse pakkuja kuulub saitide vahel, läheb rakenduse andmete Azure'i kunagi. Andmed on krüptitud-teel ja kopeeritud otse teenuse pakkuja saitide vahel.

Kui te olete imitatsiooniga Azure'i, saadetakse rakenduse andmete Azure talletusruumi, kuid mitte saidi taastamise teenust. Andmed on krüptitud teel ja jääb Azure krüptitud.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Minu rentnikud saadetakse arve, mis tahes Azure'i teenuste jaoks?

Ei. Azure arvelduse seos on otse pakkuja juures. Teenuse pakkujad ei vastuta oma rentnikke teatud arved loomine.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Kui ma olen imitatsiooniga Azure'i, läheb vaja Azure'i virtuaalmasinates käivitada igal ajal?

Ei, konto Azure mäluruumi teie tellimus on kopeeritud andmed. Test Tõrkesiirde (DR drill) või on tegelik Tõrkesiirde täitmisel loob saidi taastamine virtuaalmasinates automaatselt teie tellimus.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Kas kui ma korrata Azure rentnikutasemel eraldamise tagada?

Jah.

### <a name="what-platforms-do-you-currently-support"></a>Millist platvormid tehke te praegu toetab?

Azure'i paketi pilve platvormi süsteemi, toetame ja System Center vastavalt juurutuste (2012 ja uuemad versioonid). [Lisateavet leiate](https://technet.microsoft.com/library/dn850370.aspx) Azure'i paketi ja saidi taastamine integreerimise kohta.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Kas te toetavad ühe Azure'i paketi ja ühe VMM server juurutuste?

Jah, saate ise Azure'i virtuaalmasinates Hyper-V või teenuse pakkuja saitide vahel.  Pange tähele, et kui te ise teenuse pakkuja saitide vahel, integreerimine Azure käitusjuhendi pole saadaval.


## <a name="next-steps"></a>Järgmised sammud

- Lugege [saidi taastamine ülevaade](site-recovery-overview.md)
- Lisateavet [saidi taastamine arhitektuur](site-recovery-components.md)  
