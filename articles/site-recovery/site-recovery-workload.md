<properties
    pageTitle="Millist töökoormus saate kaitsta Azure saidi taastamine?"
    description="Azure'i saidi taastamine kaitseb teie töökoormus ja rakenduste dispersioonanalüüs, Tõrkesiirde ja taastamise kohapealse virtuaalmasinates ja füüsilise serveri Azure või sekundaarne kohapealse saidile"
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
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Millist töökoormus saate kaitsta Azure saidi taastamine?


Selles artiklis kirjeldatakse töökoormus ja rakenduste saaksite Azure saidi taastamise teenuse korrata.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Ülevaade

Peavad organisatsioonid äri järjepidevus ja avariijärgne taastamine (BCDR) strateegia säilitada töökoormus ja andmete turvalised ja saadaval ajal plaanitud ja plaanimata tööseisakute ja tavalise tööpäeva tingimustele nii kiiresti kui võimalik taastada.

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine. Saidi taastamise abil saate juurutada rakenduse arvestada dispersioonanalüüs pilveteenusesse või saidi. Kas teie rakendused asuvad Windowsi või Linuxi, töötava füüsilise serverites VMware või Hyper-V abil saate saidi taastamine korraldab dispersioonanalüüs, teha katastroofi taastamine katsetamine, ja Käivita failovers ja failback.


Saidi taastamine integreerub Microsoft rakendusi, sh SharePointi, Exchange'i, Dynamics, SQL serveri ja Active Directory. Microsoft ka tihedat koostööd ees müüjad, sh Oracle, SAP-i, IBM ja punane rolli. Saate kohandada dispersioonanalüüs lahenduste rakendus – rakenduse alusel.

## <a name="why-use-site-recovery-for-application-replication"></a>Miks kasutada saidi taastamine rakenduse kopeerimise?

Saidi taastamine protsentuaalset Rakendusetaseme kaitse ja taastamiseks järgmiselt:

- Rakenduse diagnostika, pakkudes dispersioonanalüüs mis tahes toetatud arvutis töötab töökoormus.
- Lähedal sünkroonse kopeerimisest, RPOs kasvõi vajadustele kõige tähtsamate ärirakendusi 30 sekundit.
- Ühe või mitme tasandi rakenduste rakenduse ühtsete hetktõmmiseid.
- SQL serveri AlwaysOn ja muude Rakendusetaseme dispersioonanalüüs tehnoloogiale, sealhulgas AD kopeerimine, SQL-i AlwaysOn, Exchange'i andmebaasi kättesaadavuse rühmad (DAGs) ja Oracle'i andmete Guard koostöös integreerimine.
- Paindlik taastamine lepingud, mis võimaldavad taastada ka kogu taotluse virnas ühe klõpsuga ja lisada välise skripte ja käsitsi toimingud leping lisada.
- Täpsemad võrgu haldus saidi taastamine ja Azure lihtsustada rakenduse võrgu nõuded, sh võimalus reserveerida IP-aadressi konfigureerimine koormust tasakaalustavad ja integreerimine Azure liikluse haldur, madal RTO võrgu switchovers.
-  Rikkaliku automatiseerimise teeki, mis pakub tootmise valmis, rakenduse kohased skripte, et saate alla laadida ja taastamise integreeritud.



## <a name="workload-summary"></a>Töökoormus Kokkuvõte

Saidi taastamine saate kopeerida mõne rakenduse toetatud arvutis töötab. Lisaks on partneriks toote meeskondadel teha täiendavaid konkreetse rakenduse testimine.

**Töökoormus** | **Paljundada Hyper-V VMs saidi** | **Azure Hyper-V VMs paljundada** | **Paljundada VMware VMs saidi** | **Paljundada VMware VMs Azure'i**
---|---|---|---|---
Active Directory DNS-i | Y | Y | Y | Y
Veebirakenduste (IIS-i, SQL-i) | Y | Y | Y | Y
System Center Toiminguhalduri | Y | Y | Y | Y
SharePointi | Y | Y | Y | Y
SAP-I<br/><br/>SAP-i saidi paljundada Azure'i jaoks – kobar | Y (Microsofti testitud) | Y (Microsofti testitud) | Y (Microsofti testitud) | Y (Microsofti testitud)
Exchange (mitte DAG) | Y | Tulen varsti | Y | Y
Remote'i töölaua/VDI-Lisandmoodul | Y | Y | Y | N/A
Linux (operatsioonisüsteem ja rakendused) | Y (Microsofti testitud) | Y (Microsofti testitud) | Y (Microsofti testitud) | Y (Microsofti testitud)
Dynamics AXILE | Y | Y | Y | Y
Dynamics CRM-iga | Y | Tulen varsti | Y | Tulen varsti
Oracle'i | Y (Microsofti testitud) | Y (Microsofti testitud) | Y (Microsofti testitud) | Y (Microsofti testitud)
Windows faili Server | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Active Directory ja DNS-i ise

Mõne Active Directory ja DNS-i infrastruktuur on oluline, et enamik enterprise rakendused. Ajal avariitaastet, peate kaitsta ja taastada nende taristu komponendid, enne tagasi oma töökoormus ja rakendused.

Saate luua automatiseeritud katastroof taastamise kava Active Directory ja DNS-i saidi taastamine. Oletagem näiteks, et soovite nurjuda SharePointi ja SAP-i esmane, teisene saidile, saate häälestada taastamise leping, millele ei üle Active Directory esmalt ja seejärel täiendavad konkreetse rakenduse taastamise kava kasutada rakendused, mis sõltuvad Active Directory.

[Lisateavet leiate teemast](site-recovery-active-directory.md) Active Directory ja DNS-i kaitsmise kohta.

## <a name="protect-sql-server"></a>SQL serveri kaitsmine

SQL serveri andmeid teenuste foundation ette andmeteenuste palju äri rakendusi kohapealse data Centeri kaudu.  Saidi taastamine saab kasutada koos SQL serveri HA/DR tehnoloogiate mitmerajalisest enterprise rakendused, mis kasutavad SQL serveri kaitsta. Saidi taastamine pakub.

- Lihtne ja soodne katastroofi taastamise lahendus SQL serveri korral. Azure'i või saidi korrata mitme SQL serveri autonoomse serverid ja kogumite, väljaanded ja versioonid.  
- SQL-i Kättesaadavusrühmad, hallata Tõrkesiirde ja failback Azure saidi taastamine taastamise lepingutega integreerimine.
- Kõigi tasemete seas rakenduses, sh SQL serveri andmebaasi-lõpuni taastamise kavad.
- SQL serveri jaoks tippväärtus skaleerimist laadib saidi taastamine, järgi "lõhkemist" need üheks suuremad IaaS virtuaalse masina suurused Azure.
- Lihtne testimine SQL serveri katastroofiabi. Käivitage test failovers andmete analüüsimiseks ja käivitada ilma mõjutavad tootmiskeskkonnast vastavuse kontrollimise.

[Lisateavet leiate teemast](site-recovery-sql.md) SQL serveri kaitsmise kohta.

##<a name="protect-sharepoint"></a>SharePointi kaitsmine

Azure'i saidi taastamine aitab kaitsta SharePointi juurutustes, järgmiselt:

- Vajate ja seotud taristu kulud tõttu serveripargi tõrkejärgseks kaob. Saidi taastamise abil saate ise on kogu serveripargis (Web, rakenduse ja andmebaasi astme) Azure või saidi.
- Lihtsustab rakenduse juurutamine ja haldus. Värskenduste juurutatud esmane saidi paljundatakse automaatselt ja on seega pärast Tõrkesiirde ja pargis saidi taastamine saadaval. Samuti vähendab juhtimise keerukus ja seotud tõttu serveripargi ajakohasena hoidmine kulud.
- SharePointi rakenduste arendamise ja testimine tootmise nagu Kopeeri nõudmisel koopia keskkonna testimine ja silumine loomisega lihtsustab.
- Ülemineku pilveteenusele lihtsustab migreerimiseks SharePointi juurutustes Azure saidi taastamise abil.

[Lisateavet leiate teemast](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) SharePointi kaitsmise kohta.


## <a name="protect-dynamics-ax"></a>Dynamics AXI kaitsmine

Azure'i saidi taastamine aitab kaitsta teie Dynamics AXI ERP lahendus, mõõt.

- Orkesteroinnin kogu Dynamics AXI keskkonna (Web ja AOS astme, andmebaasi astme, SharePointi) Azure või saidi kopeerimine.
- Lihtsustamine migreerimine Dynamics AXI juurutuste pilveteenusesse (Azure).
- Dynamics AXI rakenduste arendamise lihtsustamine ja testimine on tootmise nagu Kopeeri nõudmisel, katsetamine ja silumine loomisega.

[Lisateavet leiate teemast](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) dünaamiline AXI kaitsmise kohta.

## <a name="protect-rds"></a>RDS kaitsmine

Kaugtöölaua teenused (RDS) võimaldab töölaua virtuaaltöölaua infrastruktuuri (VDI), seansi töölaudu ja rakendusi, mis võimaldab kasutajatel töötada igal pool. Azure'i saidi taastamine saate teha järgmist.

- Saidi, ja serveri rakendused ja seansid saidi või Azure paljundada hallatava või mittehallatava ühise virtuaalset töölauda.
- Siin on, mida saate ise.

**RDS** | **Paljundada Hyper-V VMs saidi** | **Azure Hyper-V VMs paljundada** | **Paljundada VMware VMs saidi** | **Paljundada VMware VMs Azure'i** | **Paljundada füüsilise serveri saidi** | **Azure'i füüsilise serveri paljundada**
---|---|---|---|---|---|---
**Ühise virtuaalse töölaua (majandamata)** | Jah | Ei | Jah | Ei | Jah | Ei
**Ühise virtuaalse töölaua (hallatavate ja ilma UPD)** | Jah | Ei | Jah | Ei | Jah | Ei
**Serveri rakendused ja seansside (ilma UPD)** | Jah | Jah | Jah | Jah | Jah | Jah


[Lisateavet leiate teemast](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) RDS'IST kaitsmise kohta


## <a name="protect-exchange"></a>Exchange'i kaitsmine

Saidi taastamine aitab kaitsta Exchange'i, järgmiselt:

- Väike Exchange'i juurutuste korral ühe või eraldi serverid, nt saidi taastamine ise ja ei õnnestu üle Azure'i või saidi.
- Suuremate juurutuste saidi taastamine integreerub Exchange'i DAGS.
- Exchange'i DAGs on soovitatav lahendus Exchange'i tõrkejärgseks ettevõttes.  Saidi taastamine taastamise lepingud võivad hõlmata DAGs korraldab DAG Tõrkesiirde saitidel.


[Lisateavet leiate teemast](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) Exchange kaitsmise kohta.

## <a name="protect-sap"></a>SAP-i kaitsmine

Saidi taastamise abil kaitsta SAP-i juurutamise järgmiselt:

- Lubamiseks kogu SAP-i kasutuselevõtu imitatsiooniga erinevate juurutamise kihid Azure'i või saidi.
- Lihtsustage pilveteenusesse migreerimiseks, juurutamise SAP-i migreerimiseks Azure saidi taastamise abil.
- SAP-i arengu lihtsustamiseks ja testimine, luues meeldimise tootmise kopeerida nõudmisel testimiseks ja silumine rakendusi.

[Lisateavet leiate teemast](http://aka.ms/asr-sap) SAP-i kaitsmise kohta.

## <a name="next-steps"></a>Järgmised sammud

[Saidi taastamine juurutamise ettevalmistamine](site-recovery-best-practices.md) 
