<properties
    pageTitle="Katse ja ulatuse tulemusi kohapealse kohapealse Hyper-V kopeerimine saidi taastamine | Microsoft Azure'i"
    description="Sellest artiklist leiate teavet jõudluse testimine kohapealse abil kohapealse kopeerimise abil Azure saidi taastamise kohta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Jõudlust testida ja ulatuse tulemusi kohapealse abil kohapealse Hyper-V kopeerimine saidi taastamine

Saate Microsoft Azure saidi taastamise korraldab ja hallata dispersioonanalüüs virtuaalmasinates ja füüsilise serveri Azure'i või sekundaarne andmekeskuse. Selles artiklis antakse jõudluse testimine me ei, kui imitatsiooniga Hyper-V virtuaalmasinates kahe kohapealse andmekeskuste tulemused.



## <a name="overview"></a>Ülevaade

Testimise eesmärk oli uurida, kuidas Azure saidi taastamise sooritab ühtlane olekus kopeerimise ajal. Ühtlane olekus dispersioonanalüüs juhul, kui virtuaalmasinates on lõpetatud algse kopeerimise ja delta muudatuste sünkroonimine. See on oluline abil ühtlane olek, kuna tegemist on riik, kus enamik virtuaalmasinates jäävad kui ootamatute katkestuste tekkida jõudluse mõõtmiseks.


Juurutamise test oli kahe kohapealse saitide VMM Server iga ala. Selle testi juurutamise on tüüpiline pea office/haru Office'i juurutamise, mille peakontor esmane saidi ja kontoris, teisese või taastamine saidi nimega.

### <a name="what-we-did"></a>Me ei

Siin on, mida me ei testi edasi.

1. Loodud virtuaalmasinates VMM mallide kasutamine.

1. Alustamise virtuaalmasinates ja jäädvustada võrdlusalus jõudluse mõõdikute 12 tunni jooksul.

1. Loodud parkimise esmane ja taastamise VMM serverites.

1. Azure'i saidi taastamise, sh vastendamise lähte- ja taastamise pilvede konfigureeritud pilvepõhise kaitse.

1. Lubatud virtuaalmasinates kaitse ja lastakse esialgse täieliku kopeerimine.

1. Oodanud paari tunni süsteemi stabiliseerumiseks.

1. Jäädvustatud jõudluse mõõdikute 12 tunni jooksul tagada kõigi virtuaalmasinates jäi oodatud dispersioonanalüüs olekus need 12 tundi.

1. Mõõtke delta jõudluse võrdlusalus mõõdikud ja dispersioonanalüüs jõudluse mõõdikute vahel.

## <a name="test-deployment-results"></a>Juurutamise tulemused

### <a name="primary-server-performance"></a>Esmane server jõudlus

- Hyper-V koopia jälitab muudatused asünkroonselt esmane server logifaili koos minimaalne salvestusruumi pea kohal.

- Hyper-V koopia kasutab omas säilitada vahemälu minimeerimiseks IOPS kohal jälitus. See talletab kirjutab VHDX mälu ja tühjendatakse need üheks logifaili enne Logi saadetud taastamine saidi. Flush kettal juhtub ka siis, kui selle kirjutab tabas lülitame limiit.

- Joonis näitab kopeerimise pea ühtlane olekus IOPS kohal. Me näeme, et IOPS kohal tõttu dispersioonanalüüs on umbes 5%, mis on üsna madal.

![Esmane tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V koopia kasutab mälu optimeerida jõudluse esmane serveris. Nagu on näidatud järgmises graafik, on marginaalne mälu kohal esmane kobar kõigis serverites. Mälu kohal kuvatud on kasutatud võrreldes kokku installitud mälu Hyper-V server kopeerimise mälu protsent.

![Esmane tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V koopia on minimaalne CPU pea kohal. Nagu joonisel näha, on vahemikus 2 – 3% dispersioonanalüüs pea kohal.

![Esmane tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Teisese (taastamine) serveri jõudluse

Hyper-V koopia kasutab väike mälu taastamine serveris optimeerida salvestusruumi toimingute arv. Graafik Kokkuvõte mälukasutust taastamine serveris. Mälu kohal kuvatud on kasutatud võrreldes kokku installitud mälu Hyper-V server kopeerimise mälu protsent.

![Teisene tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Suuruse taastamine saidi/v-toimingud on selle funktsiooni kirjutada toiminguid Esmane saidil arvu. Vaatame kokku/v-toimingud taastamine saidi võrreldes kokku/v-toimingud ja kirjutage esmane saidil toiminguid. Graafikud näitavad, et kogusumma IOPS saidil taastamine

- Umbes 1,5 korda kirjutada IOPS esmase.

- Umbes 37% kokku IOPS esmane saidil.

![Teisene tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Teisene tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Mõju kopeerimise võrgu kasutamine

Keskmiselt 275 MB sekundis võrgu läbilaskevõime kasutati suhtes olemasoleva läbilaskevõime 5 GB sekundis esmane ja taastamise sõlme (koos tihendamine lubatud) vahel.

![Tulemite võrgu kasutamine](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Mõju kopeerimise virtuaalse masina jõudlus

Oluline on kopeerimise tootmise töökoormus töötab soovitud virtuaalmasinates mõju. Kui esmane saidi on piisavalt ette valmistatud kopeerimise, ei tohiks klõpsake soovitud teenustest olla mingit mõju. Hyper-V koopia kerge jälgimine süsteemi tagab töötab soovitud virtuaalmasinates töökoormus pole mõjutab ühtlane olekuga kopeerimise ajal. See on kujutatud järgmises diagrammid.

See graafik esitab IOPS virtuaalmasinates töötab muus töökoormus enne ja pärast kopeerimine on lubatud. Saate jälgida, on kahe vahel.

![Koopia efekti tulemused](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Järgneval joonisel on läbilaskevõime virtuaalmasinates töötab muus töökoormus enne ja pärast kopeerimine on lubatud. Saate jälgida, et dispersioonanalüüs ei ole olulist mõju.

![Tulemuste koopia efektid](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Kokkuvõte

Tulemused näitavad selgelt, et Azure saidi taastamise, koos Hyper-V koopia, skaala ka koos miinimum kohal suur kobar.  Azure'i saidi taastamine pakub lihtsa juurutus, kopeerimine, juhtimiseks ja jälgimiseks. Hyper-V koopia pakub taristut eduka dispersioonanalüüs skaleerimist. Kavandamise on optimaalse juurutamise soovitame laadite [Hyper-V koopia võimsus Plaanur](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Keskkonna üksikasjad

### <a name="primary-site"></a>Esmane saidi

- Esmane saidil on viis Hyper-V serverites 470 virtuaalmasinates sisaldav klaster.

- Funktsiooni virtuaalmasinates Käivita erinevate töökoormus ja kõik on lubatud Azure saidi taastamise kaitse.

- Salvestusruumi kobar sõlme pakub serveri SAN. Mudel – Hitachi HUS130.

- Iga kobar server on üks Gbit neli võrgu kaarti (NICs).

- Kahe võrgu kaardid on ühendatud mõne iSCSI privaatvõrgu ja kahe välise ettevõtte võrku ühendatud. Üks väliseid võrgustikke on mõeldud ainult kobar suhtlus.

![Esmane riistvaranõuded](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Server|RAM-I|Mudel|Protsessor|Protsessorite arvu|NIC|Tarkvara|
|---|---|---|---|---|---|---|
|Hyper-V kobar serverites: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25|128ESTLAB-HOST25 on 256|Dell™ PowerEdge™ R820|Intel Xeon(R) CPU E5-4620 0 @ 2.20GHz|4|Ma Gbit x 4|Windows Server andmekeskuse 2012 R2 (x64) + Hyper-V roll|
|VMM Server|2|||2|1 Gbit|Windows Server 2012 andmebaasi R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Teisese (taastamine) sait

- Teisene saidil on kuus sõlme tõrkesiirdeklastrite.

- Salvestusruumi kobar sõlme pakub serveri SAN. Mudel – Hitachi HUS130.

![Esmane riistvara määratlus](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Server|RAM-I|Mudel|Protsessor|Protsessorite arvu|NIC|Tarkvara|
|---|---|---|---|---|---|---|
|Hyper-V kobar serverites: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10|96|Dell™ PowerEdge™ R720|Intel Xeon(R) CPU E5-2630 0 @ 2.30GHz|2|Ma Gbit x 4|Windows Server andmekeskuse 2012 R2 (x64) + Hyper-V roll|
|ESTLAB-HOST17|128|Dell™ PowerEdge™ R820|Intel Xeon(R) CPU E5-4620 0 @ 2.20GHz|4||Windows Server andmekeskuse 2012 R2 (x64) + Hyper-V roll|
|ESTLAB-HOST24|256|Dell™ PowerEdge™ R820|Intel Xeon(R) CPU E5-4620 0 @ 2.20GHz|2||Windows Server andmekeskuse 2012 R2 (x64) + Hyper-V roll|
|VMM Server|2|||2|1 Gbit|Windows Server 2012 andmebaasi R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Serveri töökoormus

- Katsetamiseks valitud enterprise kliendi stsenaariumid levinud töökoormus.

- Kasutame [IOMeter](http://www.iometer.org) töökoormus omaduste alusel valitud tabeli modelleerimiseks summeeritud.

- Kõik IOMeter profiilid määratakse kirjutamiseks juhusliku baiti simuleerida halvima kirjutamine mustrite töökoormus.

|Töökoormus|I/O suurus (KB)|% Juurdepääs|% Lugemine|Tasumata väljundtoimingute|I/O muster|
|---|---|---|---|---|---|
|Faili Server|48163264|60 20 %5 %5% 10%|80% 80% 80% 80% 80%|88888|100% juhusliku|
|SQL serveri (käibe-1) SQL serveri (käibe-2)|864|100% 100%|70 %0%|88|100% random100% järjestikune|
|Exchange'i|32|100%|67%|8|100% juhusliku|
|Töökoha/VDI-Lisandmoodul|464|66% 34%|70% 95%|11|Mõlemad 100% juhusliku|
|Faili veebiserverisse|4864|33 34% 33%|95% 95% 95%|888|75% juhusliku|

### <a name="virtual-machine-configuration"></a>Virtuaalse masina konfigureerimine

- 470 virtuaalmasinates esmane kobar.

- Kõik virtuaalmasinates VHDX ketas.

- Virtuaalmasinates töötab summeeritud tabelis töökoormus. Kõik loodi VMM mallid.

|Töökoormus|# VMs|Minimaalne RAM (GB)|Suurim lubatud RAM (GB)|Loogilise ketta suurus (GB) VM kohta.|Suurim lubatud IOPS|
|---|---|---|---|---|---|
|SQL serveri|51|1|4|167|10|
|Exchange Serveri|71|1|4|552|10|
|Faili Server|50|1|2|552|22|
|VDI-LISANDMOODUL|149|.5|1|80|6|
|Veebiserver|149|.5|1|80|6|
|KOGUSUMMA|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure'i saidi taastamine sätted

- Kohapealse kohapealse kaitse konfigureeritud Azure saidi taastamine

- VMM server on konfigureeritud, neli pilved Hyper-V kobar serverite ja nende virtuaalmasinates sisaldavad.

|Esmane VMM pilveteenuses|Kaitstud virtuaalmasinates pilveteenuses|Sagedus dispersioonanalüüs|Täiendavad taastamise punkte|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 minutit|Ükski|
|PrimaryCloudRpo30s|47|30 sekundit|Ükski|
|PrimaryCloudRpo30sArp1|47|30 sekundit|1|
|PrimaryCloudRpo5m|235|5 min|Ükski|

### <a name="performance-metrics"></a>Jõudluse mõõdikud

Tabelis on kokku võetud jõudluse mõõdikute ja väidab, et olid mõõdetud juurutamise.

|Meetermõõdustik|Counter|
|---|---|
|PROTSESSOR|\Processor(_Total)\% protsessori aeg|
|Saadaoleva mäluga|\Memory\Available MB|
|IOPS|\PhysicalDisk (_Total) \Disk edastamine sekundis|
|VM lugeda (IOPS) sekundis|\Hyper-V virtuaalse mäluseadmesse (<VHD>) \Read sekundis|
|VM kirjutamine (IOPS) sekundis|\Hyper-V virtuaalse mäluseadmesse (<VHD>) \Write toimingute/S|
|Lugege läbilaskevõime VM|\Hyper-V virtuaalse mäluseadmesse (<VHD>) \Read baiti sekundis|
|Läbilaskevõime VM kirjutamine|\Hyper-V virtuaalse mäluseadmesse (<VHD>) \Write baiti sekundis|


## <a name="next-steps"></a>Järgmised sammud

- [Vahel kahe kohapealse VMM saitide häälestamine](site-recovery-vmm-to-vmm.md)
