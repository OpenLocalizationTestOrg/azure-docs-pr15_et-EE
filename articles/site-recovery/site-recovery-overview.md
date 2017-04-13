<properties
    pageTitle="Mis on saidi taastamine? | Microsoft Azure'i"
    description="Annab ülevaate Azure saidi taastamise teenuse ja juurutamise stsenaariumide kokkuvõte."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Mis on saidi taastamine?

Tere tulemast Azure'i saidi taastamine! Selles artiklis antakse kiirülevaade saidi taastamise teenust ja kuidas see aitab teie ettevõtte.

Ettevõtte vajab järjepidevuse ja suurõnnetuste taastamine (BCDR) strateegia, mis hoida rakendused, töökoormus ja andmete turvalised ja saadaval plaanitud ja plaanimata tööseisakute ajal ja taastab tavalise tööpäeva tingimustele nii kiiresti kui võimalik. Saidi taastamine on Azure'i teenus, mis aitab see strateegia.

Saidi taastamine orchestrates kopeerimine füüsilise kohapealsed serverid ja virtuaalmasinates töökoormus. Saaksite korrata serverid ja VMs kaudu esmane andmekeskuse pilveteenusesse (Azure) või teisene andmekeskusesse. Katkestuste ilmnemisel esmane saidi teil ei õnnestu üle teisene saidile hoida rakendused ja töökoormus kättesaadavad. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid.

## <a name="site-recovery-in-the-azure-portal"></a>Saidi taastamine Azure'i portaalis

Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine. Azure'i ressursihaldur mudel ja klassikaline teenuste haldus mudel. Azure'i on kaks portaalide – [klassikaline Azure portaali](https://manage.windowsazure.com/) klassikaline juurutamise mudeli toetava ja [Azure portaali](https://portal.azure.com) nii klassikaline ja ressursihaldur mudelite toega.

- Saidi taastamine on saadaval nii klassikaline portaali ja Azure portaali.
- Azure'i klassikaline portaalis saate toetavad saidi taastamine mudeliga klassikaline teenuste haldus.
- Azure'i portaalis toetab klassikaline mudel või ressursihaldur juurutuste. 

Selle artikli teave kehtib klassikaline- ja Azure portaali juurutuste. Erinevused on märkida vajaduse korral.


## <a name="why-deploy-site-recovery"></a>Miks kasutada saidi taastamine?

Siit, mida saidi taastamine teha oma ettevõtte jaoks.

- **Lihtsustage BCDR**– saate hakkama dispersioonanalüüs, Tõrkesiirde ja taastamine mitme töökoormus Azure'i portaalis ühes kohas. Saidi taastamine orchestrates kopeerimise ja Tõrkesiirde, kuid ei intercept rakenduse andmete või olla mis tahes teavet.
- **Sisesta paindlik dispersioonanalüüs**– saidi taastamise abil saate ise töötab toetatud Hyper-V VMs, VMware VMs ja Windows/Linux füüsilise serveri töökoormus.
- **Käivita lihtne dispersioonanalüüs testimine**– pakub saidi taastamine testi failovers toetama katastroofi taastamine harjutused tootmise keskkondades mõjutamata.
- **Ei õnnestu üle ja taastamine**– saate käivitada kavandatud failovers koos null andmekao oodatud katkestuste eest või planeerimata failovers koos minimaalsete andmekao (olenevalt dispersioonanalüüs sagedus) katastroofideks ootamatut. Pärast Tõrkesiirde, saate failback esmane saitidele. Saidi taastamine pakub taastamine lepingud, mis võivad hõlmata skripte ja Azure automatiseerimine töövihikute nii, et saate kohandada Tõrkesiirde ja mitmekihilise rakenduste taastamine.
- **Teisene andmekeskuse elimineerimiseks**– saate ise töökoormus, Azure, mitte saidi. See kaob maksumuse ja keerukuse teisene andmekeskuse haldamine. Kopeeritud andmed salvestatakse Azure Storage, paindlikkuse, mis pakub. VMs on loodud kopeeritud andmed Tõrkesiirde korral.
- **Integreerida olemasolevate BCDR tehnoloogiatega**– saidi taastamine integreerub BCDR muid funktsioone. Näiteks saate saidi taastamise SQL serveri taustväärtus ettevõtte töökoormus, sh kohalikke tugi SQL serveri AlwaysOn kättesaadavus rühma Tõrkesiirde haldamiseks, kaitsmiseks.

## <a name="what-can-i-replicate"></a>Mida saab kopeerida?

Siit leiate kokkuvõtte, mida saate ise saidi taastamise abil.

![Kohapealse kohapealse](./media/site-recovery-overview/asr-overview-graphic.png)

**JUHENDIST** | **PALJUNDADA** 
---|---
Kohapealse VMware VMs töötavate töökoormus | [Azure'i](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Saidi](site-recovery-vmware-to-vmware.md)
Kohapealse Hyper-V VMs töötavate töökoormus hallatava VMM pilved  | [Azure'i](site-recovery-vmm-to-azure.md)<br/><br/> [Saidi](site-recovery-vmm-to-vmm.md) 
Kohapealse Hyper-V VMs töötavate töökoormus hallatava VMM pilve sees, SAN salvestusruum|  [Saidi](site-recovery-vmm-san.md)
Kohapealse Hyper-V VMs ilma VMM töötavate töökoormus | [Azure'i](site-recovery-hyper-v-site-to-azure.md)
Kohapealse füüsilise Windows/Linux serverites töökoormus | [Azure'i](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Saidi](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Millist töökoormus kaitsta?

Saidi taastamine võimaldab rakenduse arvestada BCDR, nii, et töökoormus ja rakendused edasi töötada iga kord ühtemoodi katkestuste korral. Saidi taastamine pakub.

- **Rakenduse ühtsete hetktõmmiseid**– masinad ise rakenduse ühtsete hetktõmmiseid kasutamise ühe või mitme taseme rakendused. Lisaks ketta andmehõive rakenduse ühtsete hetktõmmiseid jäädvustada jäädvustada kõik andmed mällu ja kõigi tehingute käigus.
- **Lähedal sünkroonse dispersioonanalüüs**– pakub saidi taastamine dispersioonanalüüs sagedus on kuni 30 sekundit Hyper-V ja pidev dispersioonanalüüs jaoks VMware.
- **Paindlik taastamise**– saate luua ja kohandada taastamise plaanid välise skripte ja käsitsi toimingud. Integreerimine Azure automatiseerimine tegevusraamatud võimaldavad taastada ka kogu taotluse virnas ühe hiireklõpsuga.
- **SQL serveri AlwaysOn integreerimine**– saate hallata kättesaadavus rühma Tõrkesiirde saidi taastamine taastamise lepingutes.
- **Automaatika teek**– rikkaliku Azure automatiseerimine teegi pakub tootmise valmis, rakenduse kohased skripte, et saate alla laadida ja integreeritud saidi taastamine.
- **Lihtne võrgu haldus**– Täpsemad võrgu haldus saidi taastamine ja Azure lihtsustab rakenduse võrgu nõuetele, sh broneerimist IP-aadressid, koormus soolise konfigureerimine ja integreerimine Azure liikluse haldur tõhusa võrgu switchovers jaoks.


## <a name="next-steps"></a>Järgmised sammud

- Lugege rohkem [mis töökoormus saate saidi taastamise kaitsta?](site-recovery-workload.md)
- Lisateavet saidi taastamine arhitektuuri [Kuidas saidi taastamine toimib?](site-recovery-components.md)
 
