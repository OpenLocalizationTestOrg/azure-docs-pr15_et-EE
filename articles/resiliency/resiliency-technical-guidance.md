<properties
   pageTitle="Paindlikkust tehnilised juhendid index | Microsoft Azure'i"
   description="Tehniline mõisteid mõistmine ja kujundamisel olles, mis on väga saadaval, tõrketaluvusega rakendused, samuti katastroofi taastamine ja business järjepidevuse kavandamine"
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

#<a name="azure-resiliency-technical-guidance"></a>Azure'i paindlikkust tehnilise abi saada?

##<a name="introduction"></a>Sissejuhatus

Koosoleku kõrge-saadavus ja katastroofi taastamise nõuded nõuab kahte tüüpi teadmisi tehke järgmist.

- Üksikasjalik tehniline mõistmine pilve platvormi võimalused
- Kuidas arhitekt õigesti jaotatud teenus

See sari artikleid hõlmab endise: võimaluste ja piirangute Azure'i platvormi suhtes paindlikkust (mõnikord nimetatakse järjepidevuse). Kui olete huvitatud viimase, leiate artiklist sarja [avariitaastet](https://aka.ms/drtechguide)ja kõrge-saadavus Azure rakenduste. Kuigi see artikkel sari puudutab arhitektuur ja kujundus, mis ei ole sarja liikumine. Kujundus juhiseid, võite tutvuda materjali jaotises [Täiendavad ressursid](#additional-resources) .

Teave on korraldatud järgmistest artiklitest:

- [Kohalikud tõrked taastamine](resiliency-technical-guidance-recovery-local-failures.md).
Füüsilise riistvara (nt draivid, serverite ja võrguseadmete) võib nurjuda. Kui laadi naelu saate lõpe ressursid. Selles artiklis kirjeldatakse võimalusi säilitada kõrge-saadavus järgmistest tingimustest leiate Azure'i.

- [Mõne Azure piirkond kogu teenuse häirete taastamine](resiliency-technical-guidance-recovery-loss-azure-region.md).
Levinud tõrkeid on harva, kuid need on toonerikassettide. Kogu piirkondade võib muutuda isoleeritakse võrgu tõrgete tõttu või need võivad füüsilise katki loodusõnnetuste eest. Selles artiklis selgitatakse, kuidas luua rakendusi geograafiliselt erinevate piirkondade ulatuvad Azure'i abil.

- [Azure'i asutusesisesest taastamine](resiliency-technical-guidance-recovery-on-premises-azure.md).
Pilveteenuses muudab märgatavalt avariitaastet, mis võimaldab asutustel Azure'i abil saate luua teise saidi taastamise ökonoomika. Saate seda teha koostamise ja säilitades teisene andmekeskuse murdosa. Selles artiklis selgitatakse võimaluste pikendamiseks on kohapealse andmekeskuse pilveteenusesse leiate Azure'i.

- [Taastamine andmed on rikutud või juhuslikku kustutamist](resiliency-technical-guidance-recovery-data-corruption.md).
Rakenduste võib olla vigu rikutud andmeid. Tehtemärkide valesti kustutada olulisi andmeid. Selles artiklis selgitatakse, mis Azure pakub andmete varundamine ja taastamine eelmises punktis see aeg.

##<a name="additional-resources"></a>Lisaressursid

- [Katastroofiabi ja Microsoft Azure loodud kõrge kättesaadavus](resiliency-disaster-recovery-high-availability-azure-applications.md).
See artikkel kehtib saadavaloleku ja avariitaastet üksikasjaliku ülevaate. See hõlmab viite ja kandeandmete käsitsi kopeerimine ülesanne. Lõplik jaotistes kokkuvõtted eri tüüpi katastroofi taastamine topoloogiatest Azure piirkondade kättesaadavus kõrgeima taseme ulatuvad.

- [Kõrge-saadavus kontroll-loend](resiliency-high-availability-checklist.md).
Selles artiklis on funktsioonide teenused ja kujundusi, mis aitavad teil suurendada paindlikkust ja rakenduse kättesaadavus.

- [Microsoft Azure'i teenust paindlikkust juhiseid](resiliency-service-guidance-index.md).
Selles artiklis on Azure teenuste registri ja lingid nii katastroofi taastamise juhised ja kujundus juhiseid.

- [Ülevaade: Cloud business järjepidevus ja andmebaasi Õnnetusejärgne taastamine SQL-andmebaasi](../sql-database/sql-database-business-continuity.md).
Sellest artiklist leiate Azure'i SQL-andmebaasi tehnika kättesaadavus. Eelkõige keskendub varundus ja taaste strateegiad. Kui kasutate oma pilveteenuses Azure'i SQL-andmebaasi, peaksite see raamat ja selle seotud ressursid.

- [Kõrge-saadavus ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
Selles artiklis käsitletakse kättesaadavus suvandid, mida saate uurida, kui kasutate teenust (IaaS) majutada oma andmebaasi teenuste taristu. Arutatakse Kättesaadavusrühmad, andmebaasi peegeldus, Logi saatmine ja varundus ja taaste. Mitme õpetused näitab, kuidas neid kasutada.

- [Suuremahuliste teenust Azure pilveteenustega kujundamise head tavad](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
See artikkel keskendub arendamise väga paindlik cloud struktuur. Paljud meetodid skaleeritavus parandamiseks kasutavad ka parandada kättesaadavus. Lisaks, kui teie rakendus ei saa mastaapimiseks suurema koormuse, skaleeritavus probleemiks kättesaadavus.

- [Varundus ja taaste SQL Server Azure'i Virtuaalmasinates](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
See artikkel annab tehnilised juhendid varundamine ja taastamine Microsoft SQL Server Azure'i Virtuaalmasinates töötab.

- [Failsafe: juhised olles cloud struktuur](https://channel9.msdn.com/Series/FailSafe).
Sellest artiklist leiate juhised olles cloud struktuur, koostamise juhised nende arhitektuurides rakendamiseks Microsofti tehnoloogiate ja nende arhitektuurides jaoks teatud stsenaariumide rakendamiseks retseptid.

- [Tehniline juhtumianalüüsi: abil pilv tehnoloogiate avariitaastet parandamiseks](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
See juhtumianalüüsi näitab, kuidas kasutada Microsoft IT Azure'i avariitaastet parandamiseks.

##<a name="next-steps"></a>Järgmised sammud

See artikkel on osa sarjast värskendustest tehnilised juhendid Azure paindlikkust. Kui olete huvitatud selle sarja teiste artiklite lugemine, saate alustada [taastamise Kohalikud tõrked](resiliency-technical-guidance-recovery-local-failures.md).
