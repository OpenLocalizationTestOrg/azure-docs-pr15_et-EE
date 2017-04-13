<properties
    pageTitle="Võrgu kaardistamine Hyper-V virtuaalse masina kaitse VMM Azure saidi taastamise ettevalmistamine | Microsoft Azure'i"
    description="Saate seadistada võrgu Hyper-V virtuaalse masina kopeerimise on kohapealse andmekeskuse: Azure'i või saidi."
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
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Võrgu kaardistamine Hyper-V virtuaalse masina kaitse VMM Azure saidi taastamise ettevalmistamine

Azure'i saidi taastamine aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri.

Selles artiklis kirjeldatakse võrgu kaardistamine, mis aitab konfigureerite optimaalselt võrgu sätteid, kui te kasutate saidi taastamine korrata Hyper-V virtuaalmasinates asuvad VMM pilved kahe kohapealse andmekeskuste või mõne asutusesisese andmekeskuse ja Azure vahel. Pange tähele, et kui te olete imitatsiooniga Hyper-V VMs ilma VMM pilveteenuses või imitatsiooniga VMware VMs või füüsilise serveri, artiklit pole oluline.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Ülevaade

Võrgu kaardistamine Azure saidi taastamise on juurutatud korrata Hyper-V virtuaalmasinates Azure või sekundaarne andmekeskusesse, Hyper-V koopia või SAN kopeerimise abil saate kasutada.

- **Imitatsiooniga Hyper-V virtuaalmasinates VMM pilved kahe kohapealse andmekeskuste**– Network vastenduse kaartide VM võrgu allikas VMM Server ja VM võrkude target VMM serveris teha järgmist:

    - **Ühenduse loomine virtuaalmasinates pärast Tõrkesiirde**– tagab, et virtuaalmasinates ühendatakse vastav võrgu pärast Tõrkesiirde. Koopia virtuaalse masina ühendatakse target võrgus, mis on vastendatud source network.
    - **Koha koopia virtuaalmasinates hosti serverites**– optimaalselt paigutada koopia virtuaalmasinates Hyper-V hosti serverites. Koopia virtuaalmasinates paigutatakse hosts, millele pääsete juurde vastendatud VM võrgu.
    - **Võrgu kaardistamine**– kui konfigureerite ei võrgu kaardistamine, pärast Tõrkesiirde ei ühtegi VM võrku ühendatud tiražeeritud virtuaalmasinates.

- **Azure'i virtuaalmasinates imitatsiooniga Hyper-V kohapealse VMM-i pilve**– Network vastenduse kaartide vahel VM VMM-i serverisse ja suunata Azure võrkude teha järgmist:
    - **Ühenduse loomine virtuaalmasinates pärast Tõrkesiirde**– millist Tõrkesiirde samas võrgus saate luua ühenduse üksteisest, olenemata sellest, millist taastamise kava need on kõik masinad.
    - **Võrgu lüüsi**– kui sihtkohas Azure võrk on häälestatud võrgu lüüsi, virtuaalmasinates saate luua ühenduse muude kohapealse virtuaalmasinates.
    - **Võrgu kaardistamine**– kui konfigureerite ei võrgu kaardistamine, ainult virtuaalmasinates selle Tõrkesiirde taastamine sama lepingu saab ühendada üksteise pärast fail üle Azure.


## <a name="network-mapping-example"></a>Võrgu vastenduse näide

Võrgu vastenduse saab konfigureerida VM VMM serverites või ühe VMM Server, kui kaks saitide haldab sama serveri vahel. Kui vastenduse on õigesti konfigureeritud ja kopeerimine on lubatud, virtuaalse masina esmane asukohas on ühendatud võrku ja selle koopia asukohas target ühendatakse selle võrgudraivi.

Kui võrkude seadistatud õigesti VMM, kui valite target VM võrgu network vastendamisel, allika VM võrgu kasutavate VMM allika pilved kuvatakse, koos saadaval target VM võrkude kaitse kasutatavate target pilved.

Siin on näide illustreerimiseks see süsteem. Vaatame kahes kohas New York ja Chicago organisatsiooni.

**Asukoht** | **VMM server** | **VM võrkude** | **Vastendatud**
---|---|---|---
New York | VMM-NewYork| VMNetwork1-NewYork | Vastendatud VMNetwork1-Chicago
 |  | VMNetwork2-NewYork | Pole vastendatud
Chicago | VMM-Chicago| VMNetwork1-Chicago | Vastendatud VMNetwork1-NewYork
 | | VMNetwork2-Chicago | Pole vastendatud

See näide:

- Iga virtual masina, mis on ühendatud VMNetwork1-NewYork koopia virtuaalse masina loomisel ühendatakse VMNetwork1-Chicago.
- VMNetwork2-NewYork või VMNetwork2-Chicago koopia virtuaalse masina loomisel ühendatakse see pole mis tahes võrk.

Siin on, kuidas VMM pilved on häälestatud meie näite ettevõttes ja loogilised võrgud, mis on õhupalli seotud.

### <a name="cloud-protection-settings"></a>Pilveteenuse sätted

**Kaitstud pilveteenuses** | **Pilveteenuse kaitsmine** | **Loogilise võrgu (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Loogika ja VM võrgu sätteid.

**Asukoht** | **Loogilise võrgu** | **Seotud VM võrgu**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-networks"></a>Target (sihtkoht) võrkude

Põhjal neid sätteid, kui valite target VM võrgu, järgmine tabel näitab Valikud, mis on saadaval.

**Valige** | **Kaitstud pilveteenuses** | **Pilveteenuse kaitsmine** | **Target võrgus saadaval**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Saadaval
 | GoldCloud1 | GoldCloud2 | Saadaval
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Pole saadaval
 | GoldCloud1 | GoldCloud2 | Saadaval



## <a name="multiple-subnets"></a>Mitu alamvõrku

Kui target võrgus on mitu alamvõrku ja üks nende alamvõrku on sama alamvõrgu, mis asub allika virtuaalse masina nimi, siis koopia virtuaalse masina ühendatakse selle target alamvõrgu Tõrkesiirde pärast. Kui pole target alamvõrgu kattuvad nimega, virtuaalse masina ühendatud esimese alamvõrgu võrgus.


### <a name="failback"></a>Failback

Kuvamiseks, mis juhtub puhul failback (pööratud dispersioonanalüüs) Oletame, et VMNetwork1-NewYork vastendatakse VMNetwork1 Chicago, järgmisi sätteid.


**Virtuaalse masina** | **VM võrku ühendatud**
---|---
VM1 | VMNetwork1-võrgus
VM2 (VM1 koopia) | VMNetwork1-Chicago

Nende sätetega vaatame, mis juhtub paar võimalust.

**Stsenaarium** | **Tulemus**
---|---
Pärast Tõrkesiirde VM 2 atribuudid ei muutu. | VM 1 jääb allika võrku ühendatud.
Võrgu atribuutide VM 2 muudetakse pärast Tõrkesiirde ja katkeb. | VM-1 on ühendus katkestatud.
Võrgu atribuutide VM 2 muudetakse pärast Tõrkesiirde ja on ühendatud VMNetwork2-Chicago. | Kui VMNetwork2-Chicago pole vastendatud, katkestatakse VM-1.
Võrgu kaardistamine VMNetwork1-Chicago muudetakse. | VM 1 nüüd vastendatud VMNetwork1-Chicago võrguga.


## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil on võrgu kaardistamine, [Alustamine saidi taastamine juurutamise](site-recovery-best-practices.md)paremini mõista.
