<properties
   pageTitle="Tehnilised juhendid: Azure'i asutusesisesest taastamine | Microsoft Azure'i"
   description="Artikli kohta, kuidas kujundamisel taastamine süsteemide kohapealse taristu Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Azure'i paindlikkust tehnilised juhendid: Azure'i asutusesisesest taastamine

Azure'i pakub teenuste laiend on kohapealse andmekeskuse Azure kõrge kättesaadavus ja katastroofi taastamise eesmärgil, mis võimaldab täielik kogum.

* __Networking__: koos virtuaalse privaatvõrgu, saate turvaliseks laiendamine kohapealse võrgu pilveteenusesse.
* __Arvutage__: klientidele, kes kasutavad kohapealse Hyper-V "tõstab ja shift" olemasoleva virtuaalmasinates (VM) Azure.
* __Salvestusruumi__: StorSimple laieneb Azure Storage failisüsteemis. Azure'i varukoopia teenus pakub varundamise failid ja SQL andmebaase Azure Storage.
* __Andmebaasi tiražeerimine__: SQL serveri 2014 (või uuem versioon) kättesaadavus rühmad, saate rakendada kõrge kättesaadavus ja Avariijärgne taaste kohapealse andmete jaoks.

##<a name="networking"></a>Võrgunduse

Azure'i virtuaalse võrgu abil saate loogiliselt eraldatud jaotise Azure loomine ja turvaliselt ühendada oma kohapealse andmekeskuse või ühe kliendi seadme mõne ühenduse abil. Virtuaalse võrguga, saate ära scalable, nõudmisel infrastruktuuri Azure tagades andmete ja rakenduste asutusesiseselt, sh opsüsteemi Windows Server, mainframes ja UNIX Ühenduvus. Lisateavet leiate [Azure'i networking dokumentatsiooni](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Arvuta

Kui kasutate kohapealse Hyper-V, saate "tõstke ja shift" olemasoleva Azure'i virtuaalmasinates ja teenuse pakkujad opsüsteemi Windows Server 2012 (või uuem versioon), pole VM muuta või ümber VM vormingud. Lisateabe saamiseks lugege [ketast ja VHDs Azure'i virtuaalmasinates jaoks](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Azure'i saidi taastamine

Kui soovite avariitaastet teenust (DRaaS), pakub Azure'i [Azure saidi taastamine](https://azure.microsoft.com/services/site-recovery/). Azure'i saidi taastamine pakub põhjalik kaitse VMware, Hyper-V ja füüsilise serveritega. Azure'i saidi taastamine, saate kasutada mõne muu kohapealses serveris või Azure saidi taastamine. Azure'i saidi taastamise kohta leiate lisateavet teemast [Azure saidi taastamise dokumentatsiooni](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Salvestusruumi

On mitu võimalust, kasutades Azure varukoopia saitide kohapealsed andmed.

###<a name="storsimple"></a>StorSimple

StorSimple turvaliselt ja läbipaistev ühendab salvestusruumi kohapealse rakendusi. Saadaval on ka ühe seadme, mis pakub suure jõudlusega astmeline kohalik ja salvestusruumi, reaalajas arhiivimine, pilvepõhist andmekaitse ja katastroofiabi. Lisateabe saamiseks vaadake [StorSimple toote lehele](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure'i varukoopiad

Azure'i varukoopiad võimaldab pilve varukoopia abil varukoopia tuttavaid tööriistu, Windows Server 2012 (või uuem versioon), Windows Server 2012 Essentials (või uuem versioon) ja System Center 2012 andmete kaitse Manager (või uuem versioon). Nende tööriistade ette töövoo varukoopia juhtimine, mis ei sõltu talletuskoht varukoopiaid, kas Azure Storage või kohalikule kettale. Pärast andmete varundamise pilveteenusesse, volitatud kasutajad saavad hõlpsalt tagasi varukoopiate serveritega.

Varundamiseks, kus on ainult failide muudatused üle pilveteenusesse. See aitab tõhus kasutamine salvestusruumi, vähendada läbilaskevõime kulu ja toetavad mitme versiooni andmete taastamine punkti õigel ajal. Saate kasutada täiendavaid funktsioone, nagu andmete säilituspoliitikate, andmete tihendamise ja ahendamise andmeedastus. Kasutades Azure varundusasukohta eelis selge varukoopiaid on automaatselt "välisest allikast". See kaob Turve ja kaitse kohapeal varukoopia meediumi eest nõuetele.

Lisateavet leiate teemast [mis on Azure varukoopia?](../backup/backup-introduction-to-azure-backup.md) ja [Konfigureerida Azure varukoopia DPM andmed](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Andmebaasi

Saate oma SQL serveri andmebaasi hübriid-IT-keskkonnas katastroofi taastamise lahendus on Kättesaadavusrühmad, andmebaasi peegeldus, Logi saatmine ja varundamise abil ja Azure'i bloobimälu taastada. Kõik need lahendused kasutage SQL Server Azure'i Virtuaalmasinates töötab.

Kättesaadavusrühmad saab kasutada, kui on olemas andmebaasi koopiad nii asutusesiseses keskkonnas hübriid-IT ja pilveteenuses. See on näidatud järgmisel joonisel.

![SQL serveri Kättesaadavusrühmad hübriid pilve arhitektuuri](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Andmebaasi peegeldus võib paikneda kohapealsed serverid ja serdi-põhise installiprogrammi pilve. Järgmine diagramm näitab selle kontseptsiooni.

![SQL serveri andmebaasi peegeldus hübriid pilve arhitektuur](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Logi saatmine saab sünkroonida ja kohapealse andmebaasi SQL Serveri andmebaas on Azure virtuaalne kohapeal.

![SQL serveri logifailide hübriid pilve arhitektuuri saatmine](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Lisaks saate varundada ja kohapealse andmebaasi otse Azure'i bloobimälu.

![Azure'i bloobimälu hübriid pilve arhitektuuri SQL serveri varundamine](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Lisateabe saamiseks vt [SQL Server Azure'i virtuaalmasinates kõrge kättesaadavus ja Avariijärgne taaste](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) ja [varundus ja taaste SQL Server Azure'i virtuaalmasinates](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Kohapealse taastamine Microsoft Azure kontroll-loendid

###<a name="networking"></a>Võrgunduse

  1. Vaadake üle võrgunduse jaotises selle dokumendi.
  2. Kohapealse turvaliselt ühendada pilveteenustega virtuaalse võrgu abil.

###<a name="compute"></a>Arvuta

  1. Vaadake üle Arvuta selle dokumendi osa.
  2. Paigutada VMs Hyper-V ja Azure vahel.

###<a name="storage"></a>Salvestusruumi

  1. Vaadake üle selle dokumendi jaotise salvestusruumi.
  2. StorSimple teenuste salvestusruumi kasutamise eeliseid.
  3. Azure'i varundus teenust kasutada.

###<a name="database"></a>Andmebaasi

  1. Vaadake üle selle dokumendi jaotist andmebaasi.
  2. Kaaluge SQL Server Azure'i VMs kohta nimega varukoopia.
  3. Häälestage Kättesaadavusrühmad.
  4. Konfigureerige serdi-põhise andmebaasi peegeldus.
  5. Kasutage Logi saatmine.
  6. Kohapealse andmebaasi varundada Azure'i bloobimälu.

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast värskendustest [Azure paindlikkust tehnilised juhendid](./resiliency-technical-guidance.md). Järgmise artiklis toodud selle sarja on [taastamise andmed on rikutud või juhuslikku kustutamist](./resiliency-technical-guidance-recovery-data-corruption.md).