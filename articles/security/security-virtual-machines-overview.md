<properties
   pageTitle="Azure'i Virtuaalmasinates turvalisuse ülevaade | Microsoft Azure'i"
   description=" Azure'i Virtuaalmasinates annab teile paindlikkuse Virtualization ilma osta ja säilitada füüsilise riistvara, mis töötab virtuaalse masina.  Selles artiklis antakse ülevaade tuum Azure turbefunktsioonid, mida saab kasutada Azure'i Virtuaalmasinates. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Azure'i Virtuaalmasinates turvalisuse ülevaade

Azure'i Virtuaalmasinates abil saate juurutada mitmesuguseid arvutuste lahenduste dünaamilised viisil. Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP-i ja Azure BizTalki teenuste tugi, saate juurutada, mis tahes töökoormus ja mis tahes keeleks, peaaegu ühtegi opsüsteemi.

Azure'i virtuaalarvuti kaudu saate Virtualization ilma osta ja säilitada füüsilise riistvara, mis töötab virtuaalse masina.  Saate luua ja juurutada rakenduste kinnituse, et teie andmed on kaitstud ja turvalise meie turvaline andmekeskuste.

Azure'i, saate koostada täiustatud turvalisusega, nõuetele vastavuse lahenduste mis:

- Kaitsta oma virtuaalmasinates viiruste ja ründevara
- Tundliku loomuga andmete krüptimine
- Turvaline võrguliiklust.
- Tuvastamine ja ohtude tuvastamine
- Nõuetele vastavus

Käesoleva artikli eesmärk on ülevaate tuum Azure turbefunktsioonid, mida saab kasutada koos virtuaalmasinates. Pakume ka linke artiklitele, mis nii, et saate lisateavet funktsioonide üksikasjad.  

Põhilised Azure virtuaalse masina turvalisus funktsioonid alla selle artikli teemad:

- Ründevaratõrje
- Riistvara turvalisus mooduli
- Virtuaalse masina ketta krüptimine
- Virtuaalse masina varundamine
- Azure'i saidi taastamine
- Virtuaalne võrgunduse
- Turvalisus rühmapoliitika haldus ja aruandlus
- Nõuetele vastavus

## <a name="antimalware"></a>Ründevaratõrje

Azure, saate ründevaratõrje tarkvara turvalisus müüjad, nagu Microsoft, Symantec, Trend Micro, McAfee ja Kaspersky oma virtuaalmasinates pahatahtlike faile, nuhkvara ja muude ohtude eest kaitsta. Vt lisateavet allpool artiklid partnerite lahenduste leidmiseks.

Microsoft Antimalware Azure'i pilveteenustega ja Virtuaalmasinates on reaalajas kaitse võimalus, mis aitab tuvastada ja eemaldada viiruste, nuhkvara ja muu ründetarkvara eest.  Microsoft Antimalware pakub konfigureerida teatiste kui teada pahatahtlik või soovimatu tarkvara proovib ennast installida või käivitage operatsioonisüsteemides oma Azure.

Microsoft Antimalware on ühe-agent lahenduse rakenduste ja rentniku keskkondade mõeldud inimsekkumist taustal. Saate juurutada kaitse vastavalt oma rakenduse töökoormus, kas tavaline secure-vaikimisi koos vajadustele või täpsemad kohandatud konfiguratsiooni, sh ründevaratõrje jälgimine.

Kui juurutada ja lubada Microsoft Antimalware, core järgmised funktsioonid on saadaval.

- Reaalajas kaitse - kuvari tegevuse pilveteenustega ja Virtuaalmasinates tuvastada ja blokeerida ründevara täitmine.
- Ajastatud skannimine - perioodiliselt teostab suunatud skannimine tuvastamiseks ründevara, sh aktiivselt töötavad programmid.
- Malware parandamise - sisestatakse toimingu tuvastatud ründevara, nt kustutada või karantiini pahatahtlike faile ja puhastamisel pahatahtlik registrikirjed.
- Allkirja värskendused – automaatselt installe uusima kaitse allkirjad (viiruse mõisted) kaitse tagamiseks on ajakohane määratud sagedusega.
- Ründevaratõrje Engine värskendused – automaatselt värskendab Microsoft Antimalware engine.
- Ründevaratõrje platvormi värskendused – automaatselt värskendab Microsoft Antimalware platvormi.
- Aktiivse kaitse - aruannete kohta tuvastatud ohud ja kahtlaste ressursid, et tagada kiire reageerimine Azure telemeetria metaandmete ja võimaldab reaalajas sünkroonse allkirja kohaletoimetamise kaudu Microsofti aktiivne kaitse süsteemi (kaardid).
- Näidised, aruandlus – pakub ja teenuse Microsoft Antimalware teenuse viimistlemine ja luba tõrkeotsingu hõlbustamiseks näidised aruandeid.
- Välistamised – võimaldab rakenduse ja teenuse administraatorid konfigureerida teatud faile protsessid, ja viib välistamine kaitse ja skannimine jõudlus ja muudel põhjustel.
- Ründevaratõrje sündmuse saidikogumi - kirjete ründevaratõrje teenuse seisund, kahtlaste tegevuste ja parandamise toimingut, operatsioonisüsteemi sündmuste logi ja kogub neid kliendi Azure Storage kontole.

Lisateave: ründevaratõrje tarkvara kaitsta oma virtuaalmasinates kohta leiate lisateavet teemast:

- [Microsofti ründevaratõrje Azure pilveteenustega ja Virtuaalmasinates](../security/azure-security-antimalware.md)
- [Ründevaratõrje lahendusi Azure'i Virtuaalmasinates juurutamine](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Kuidas installida ja konfigureerida teenust Windows VM Trend Micro sügav turvalisus](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Kuidas installida ja konfigureerida Windows VM Symantec Endpoint Protection](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Uue ründevaratõrje suvandite Azure'i Virtuaalmasinates – McAfee Endpoint Protection kaitsmine](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Azure'i turuplatsil lahendustele](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Riistvara turvalisus mooduli

Krüptimise ja autentimise parandada turvalisust, kui klahvidega ise on kaitstud. Salvestades Azure'i klahvi Vault saate lihtsustada oma kriitilised saladusi ja võtmed ja haldamiseks. Klahv Vault pakub võimalust talletada oma klahvid riistvara turvalisus moodulid (HSMs) sertifitseeritud FIPS 140-2 tase 2 standarditele. Varundamise või [läbipaistvaid andmete krüptimine](https://msdn.microsoft.com/library/bb934049.aspx) SQL serveri muutmiseks saate kõik salvestatakse võti Vault klahvid või rakenduste saladusi. Õigused ja juurdepääs kaitstud nendeks hallatakse [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)kaudu.

Lisateave:

- [Mis on Azure klahvi Vault?](../key-vault/key-vault-whatis.md)
- [Azure'i klahvi Vault kasutamise alustamine](../key-vault/key-vault-get-started.md)
- [Azure'i klahvi Vault ajaveeb](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuaalse masina ketta krüptimine

Azure'i ketta krüptimine on uue võimaluse, mis võimaldab teil oma Windowsi ja Linux Azure virtuaalse masina ketast krüptida. Azure'i ketta krüptimise kasutab valdkonna standard [BitLockeri](https://technet.microsoft.com/library/cc732774.aspx) funktsioon Windowsi ja Linux funktsiooni [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) OS ja andmete ketast krüptimine.

Lahendus on integreeritud Azure klahvi Vault aitab teil määrata, ja ketta krüptimise võtmed ja saladusi tellimuse võtme vault, tagades, et virtuaalse masina ketta kõik andmed on krüptitud korral Azure salvestusruumi haldamine.

Lisateave:

- [Windowsi ja Linux IaaS VMs Azure'i ketta krüptimine](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure'i ketta krüptimise Linux ja Windows Virtuaalmasinates](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Virtuaalse masina krüptimine](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Virtuaalse masina varundamine

Azure'i varukoopiad on skaleeritav lahendus, mida kaitseb rakenduse andmete null investeering ja minimaalsete kulud. Saate rakenduse tõrked rikutud andmete ja inimeste tõrgete saate lisada rakenduste vead. Azure'i varundamise, on kaitstud oma virtuaalmasinates, kus töötab Windows ja Linux.

Lisateave:

- [Mis on Azure varukoopia?](../backup/backup-introduction-to-azure-backup.md)
- [Azure varukoopia Õppeteema](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure varukoopia teenuse – KKK](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure'i saidi taastamine

Ettevõtte BCDR strateegia oluline osa on aru saada, kuidas hoida ettevõtte töökoormus ja rakendused üles ja töötab plaanitud ja plaanimata katkestuste korral. Azure'i saidi taastamine aitab korraldab dispersioonanalüüs, Tõrkesiirde ja teenustest ja rakendused, et need oleksid saadaval teisene asukohast kui teie peamine asukoht läheb alla.

Saidi taastamine:

- **Simplifies BCDR strateegia kavandamine** – saidi taastamine hõlbustab dispersioonanalüüs, Tõrkesiirde ja taastamise mitme business töökoormus ja rakendused ühest kohast. Saidi taastamine orchestrates kopeerimise ja Tõrkesiirde, kuid ei intercept rakenduse andmete või olla mis tahes teavet.
- **Annab juurdepääsu paindlikele dispersioonanalüüs** – saidi taastamise abil saate ise töötab Hyper-V virtuaalmasinates, VMware virtuaalmasinates ja Windows/Linux füüsilise serveri töökoormus.
- **Toetab Tõrkesiirde ja taastamine** – pakub saidi taastamine testi failovers toetama katastroofi taastamine Trelle tootmise keskkondades mõjutamata. Minimaalne andmekao (olenevalt dispersioonanalüüs sagedus) katastroofideks ootamatut abil saate käivitada ka kavandatud failovers koos oodatud katkestuste null andmekao või planeerimata failovers. Pärast Tõrkesiirde, saate failback esmane saitidele. Saidi taastamine pakub taastamine lepingud, mis võivad hõlmata skripte ja Azure automatiseerimine töövihikuid nii, et saate kohandada Tõrkesiirde ja mitmekihilise rakenduste taastamine.
- **Eliminates teisene andmekeskuse** – saate korrata, teisene kohapealse saidi või Azure. Azure'i sihtkohana kasutamise avariitaastet kõrvaldab maksumuse ja keerukus saidi haldamine. Kopeeritud andmed on salvestatud Azure Storage.
- **Ühendab olemasoleva BCDR tehnoloogiatega** – saidi taastamine partnerite rakenduse BCDR muude funktsioonidega. Näiteks saate saidi taastamise kaitsta SQL serveri tagasi lõpuks ettevõtte töökoormus. See hõlmab kohalikke tugi SQL serveri AlwaysOn kättesaadavus rühma Tõrkesiirde haldamiseks.

Lisateave:

- [Mis on Azure saidi taastamise?](../site-recovery/site-recovery-overview.md)
- [Azure'i saidi taastamine tööpõhimõtted](../site-recovery/site-recovery-components.md)
- [Mida töökoormus on kaitstud Azure saidi taastamine?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuaalne võrgunduse

Virtuaalmasinates vaja võrguühendus. See tingimus toetamiseks nõuab Azure'i virtuaalmasinates Azure virtuaalse võrku ühendatud. Muu Azure virtuaalse kaudu on loogiline ehitada, ehitatud füüsilise Azure võrgu struktuuri. Iga loogiline Azure virtuaalse võrgu on eraldatud kõik muud Azure virtuaalse võrgu. Selle eraldamise aitab tagada, et teie juurutuste võrguliiklust ei ole puuetega inimestele juurdepääsetavate teistele Microsoft Azure'i klientidele.

Lisateave:

- [Azure'i võrgu turvalisuse ülevaade](security-network-overview.md)
- [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)
- [Funktsioonide võrgunduse ja partnerlus Enterprise stsenaariumid](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Turvalisus rühmapoliitika haldus ja aruandlus

Azure'i turbekeskus aitab vältida, avastada ja ohtude vastamine ja pakub saate suurendada nähtavus, ja juhtida, oma Azure ressursse turvalisus. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

Azure'i turbekeskus aitab teil optimeerida ja jälgida, virtuaalse masina turvalisus:

- Virtuaalse masina [Turvalisus soovitused](../security-center/security-center-recommendations.md) näiteks süsteemi värskendused, konfigureerimine ACL lõpp-punktid, lubada ründevaratõrje, lubada võrgu turberühmad ja rakendada ketta krüptimine.
- Teie virtuaalmasinates seisundi kontrollimine

Lisateave:

- [Azure'i turbekeskus tutvustus](../security-center/security-center-intro.md)
- [Azure'i turbekeskus korduma kippuvad küsimused](../security-center/security-center-faq.md)
- [Azure'i turbekeskus kavandamise ja toimingud](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Nõuetele vastavus

Azure'i Virtuaalmasinates on serditud FISMA, FedRAMP, HIPAA, PCI DSS tase 1 ja muudes programmides põhilistest nõuetele vastavus. Kinnitusega lihtsustab oma Azure rakenduste nõuetele vastavus ja teie ettevõtte käsitlema mitmesuguseid sise- ja regulatiivsetele nõuetele.

Lisateave:

- [Microsoft Usalduskeskus: nõuetele vastavus](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Usaldusväärsete Cloud: Microsoft Azure turvalisus, privaatsus ja vastavus](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
