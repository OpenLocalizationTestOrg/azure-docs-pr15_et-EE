<properties
    pageTitle="Migreerimine Amazon veebiteenuste virtuaalmasinates Windows Azure'i saidi taastamine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas saate migreerida Windows virtuaalmasinates töötab Amazon Web Services (AWA) Azure'i Azure saidi taastamise abil."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migreerimine Windows virtuaalmasinates Amazon Web Services (AWS) Azure'i Azure saidi taastamine

## <a name="overview"></a>Ülevaade

Tere tulemast Azure'i saidi taastamine. Selles artiklis abil saate migreerida Windowsi eksemplari töötab AWS Azure saidi taastamine. Enne alustamist, Pange tähele järgmist.

- Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi. Migreerimise Põhitoimingud on samad, kas olete konfigureerimise saidi taastamine, ressursside halduris või klassikaline. Kuid Kasutajaliidese juhiseid ja kuvatõmmised selles artiklis on olulised Azure portaali.
- **Praegu saab ainult migreerite AWS Azure. Teil võib nurjuda üle VMs AWS: Azure'i, kuid ei saa ei need tagasi uuesti. Ei ole poolelioleva dispersioonanalüüs.**
- Selles artiklis antud juhiseid migreerimise põhinevad juhised imitatsiooniga füüsilise seadme Azure. See sisaldab linke juhiseid [korrata VMware VMs või füüsilise serveri Azure'i](site-recovery-vmware-to-azure.md), mis kirjeldab, kuidas ise füüsiline server Azure'i portaalis.
- Kui häälestate saidi taastamine klassikaline portaalis, järgige üksikasjalikke juhiseid [selle](site-recovery-vmware-to-azure-classic.md)artikli. **Kasutage enam** selle [pärand artiklis](site-recovery-vmware-to-azure-classic-legacy.md)antud juhiseid.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="prerequisites"></a>Eeltingimused

Siin on selles juurutamiseks peate

- **Konfiguratsiooniserveri**: kohapealse VM, mis töötab Windows Server 2012 R2, mis toimib konfiguratsiooniserveri. Installite liiga (sh protsess server ja serveri) saidi taastamine komponendid selle VM. Lugege lisateavet [stsenaarium arhitektuuri](site-recovery-vmware-to-azure.md#scenario-architecture) ja [konfigureerimine serveris eeltingimused](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **EC2 VM juhtudel**: eksemplari, kus töötab Windows, mida soovite migreerida.

## <a name="deployment-steps"></a>Juurutamise juhised

Selles jaotises kirjeldatakse juurutamise juhised uue Azure portaali. Kui teil on vaja järgmist juurutamise saidi taastamise klassikaline portaalis, vaadake [käesoleva artikli](site-recovery-vmware-to-azure-classic.md).

1. [Loo võlvkelder](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Deploy serveri konfiguratsioon](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Pärast konfiguratsiooniserveri juurutamist kontrollida, kas seda saab koos VMs, mida soovite migreerida.
4. [Häälestamine dispersioonanalüüs sätted](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Dispersioonanalüüs poliitika loomine ja konfigureerimine serveris määrata.
5. [Mobiilsus teenuse installimine](site-recovery-vmware-to-azure.md#step-6-replication-application). Iga VM, mida soovite kaitsta peab mobiilsus teenuse installitud. See teenus saadab protsessi serveriga. Mobiilsus teenuse saate käsitsi installida või lükata ja installitakse automaatselt protsess server kui VM kaitse on lubatud. Tõuketeatised installi selle teenuse lubamiseks peaks olema konfigureeritud tulemüüri reeglid EC2 aknad, mida soovite migreerida. Turberühma EC2 eksemplaride peaks olema järgmisi reegleid:

    ![tulemüüri reeglid](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Lubada kopeerimine](site-recovery-vmware-to-azure.md#enable-replication). Lubada kopeerimine vms, mida soovite migreerida. Võite avastada EC2 linnanimede privaatne IP-aadressid, millele pääsete EC2 konsooli abil.
7. [Planeerimata Tõrkesiirde käivitada](site-recovery-failover.md#run-an-unplanned-failover). Kui algset kopeerimine on lõpule jõudnud, saate käivitada planeerimata Tõrkesiirde AWS Azure'i jaoks iga VM. Soovi korral saate luua taastamis ja käivitage planeerimata Tõrkesiirde, migreerimiseks mitme virtuaalmasinates AWS Azure. [Lisateavet leiate teemast](site-recovery-create-recovery-plans.md) taastamise kohta.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet kopeerimise stsenaariumid teiste [mis on Azure saidi taastamise?](site-recovery-overview.md)
