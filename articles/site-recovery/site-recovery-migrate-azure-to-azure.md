<properties
    pageTitle="Migreerimine IaaS Azure'i virtuaalmasinates Azure ühe piirkonna teise saidi taastamine | Microsoft Azure'i"
    description="Azure'i saidi taastamise abil saate migreerida IaaS Azure'i virtuaalmasinates piirkonniti Azure teise."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migreerimine IaaS Azure'i virtuaalmasinates Azure piirkondade Azure'i saidi taastamine

## <a name="overview"></a>Ülevaade

Tere tulemast Azure'i saidi taastamine! Kasutage seda artiklit, kui soovite migreerida Azure VMs Azure'i piirkondade vahel. Enne alustamist, Pange tähele järgmist.

- Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi. Migreerimise Põhitoimingud on samad, kas olete konfigureerimise saidi taastamine, ressursside halduris või klassikaline. Kuid Kasutajaliidese juhiseid ja kuvatõmmised selles artiklis on olulised Azure portaali.
- **Praegu saate ainult migreerida ühest piirkonnast teise. Teil võib nurjuda VMs Azure'i piirkonniti teisele üle, kuid ei saa ei need tagasi uuesti.**
- Selles artiklis antud juhiseid migreerimise põhinevad juhised imitatsiooniga füüsilise seadme Azure. See sisaldab linke juhiseid [korrata VMware VMs või füüsilise serveri Azure'i](site-recovery-vmware-to-azure.md), mis kirjeldab, kuidas ise füüsiline server Azure'i portaalis.
- Kui häälestate saidi taastamine klassikaline portaalis, järgige üksikasjalikke juhiseid [selle](site-recovery-vmware-to-azure-classic.md)artikli. **Kasutage enam** selle [pärand artiklis](site-recovery-vmware-to-azure-classic-legacy.md)antud juhiseid.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Eeltingimused

Siin on selles juurutamiseks peate:

- **Konfiguratsiooniserveri**: kohapealse VM, mis töötab Windows Server 2012 R2, mis toimib konfiguratsiooniserveri. Installite liiga (sh protsess server ja serveri) saidi taastamine komponendid selle VM. Lugege lisateavet [stsenaarium arhitektuuri](site-recovery-vmware-to-azure.md#scenario-architecture) ja [konfigureerimine serveris eeltingimused](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS virtuaalmasinates**: The VMs, mida soovite migreerida. Migreerite need VMs, kohtlemine füüsilise masinad.

## <a name="deployment-steps"></a>Juurutamise juhised

Selles jaotises kirjeldatakse juurutamise juhised uue Azure portaali. Kui teil on vaja järgmist juurutamise saidi taastamise klassikaline portaalis, vaadake [käesoleva artikli](site-recovery-vmware-to-azure-classic.md).

1. [Loo võlvkelder](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Deploy serveri konfiguratsioon](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Pärast juurutamist konfiguratsiooniserveri, märkige ruut saate suhelda koos VMs, mida soovite migreerida.
4. [Häälestamine dispersioonanalüüs sätted](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Dispersioonanalüüs poliitika loomine ja konfigureerimine serveris määrata.
5. [Mobiilsus teenuse installimine](site-recovery-vmware-to-azure.md#step-6-replication-application). Iga VM, mida soovite kaitsta peab mobiilsus teenuse installitud. See teenus saadab protsessi serveriga. Mobiilsus teenuse saate käsitsi installida või lükata ja installitakse automaatselt protsess server kui VM kaitse on lubatud. Tõuketeatised installi selle teenuse lubamiseks peaks olema konfigureeritud tulemüüri reeglid VMs, mida soovite migreerida.
6. [Lubada kopeerimine](site-recovery-vmware-to-azure.md#enable-replication). Lubada kopeerimine vms, mida soovite migreerida. Saate teada, mida soovite migreerida abil privaatne IP-aadress on virtuaalmasinates Azure'i IaaS virtuaalmasinates. Leiate Azure'i armatuurlaual virtuaalse masina selle aadressi. Kui lubate dispersioonanalüüs, saate seada seadme tüüp VMs nimega füüsilise masinad.
7. [Planeerimata Tõrkesiirde käivitada](site-recovery-failover.md#run-an-unplanned-failover). Kui algset kopeerimine on lõpule jõudnud, saate käivitada planeerimata Tõrkesiirde piirkonniti Azure teise. Soovi korral saate luua taastamis ja käivitage planeerimata Tõrkesiirde, migreerida mitme virtuaalmasinates piirkondade vahel. [Lisateavet leiate teemast](site-recovery-create-recovery-plans.md) taastamise kohta.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet kopeerimise stsenaariumid teiste [mis on Azure saidi taastamise?](site-recovery-overview.md)
