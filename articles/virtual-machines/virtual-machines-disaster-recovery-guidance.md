<properties
    pageTitle="Mida teha juhul on Azure teenuse häirete mõjutab Azure'i virtuaalmasinates | Microsoft Azure'i"
    description="Siit saate teada, mida teha juhul, kui mõni Azure teenuse häirete mõjutab Azure'i virtuaalmasinates."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Mida teha juhul on Azure teenuse häirete mõjutab Azure'i virtuaalmasinates

Microsofti töötame selle nimel, veenduge, et meie teenuste on alati saadaval, kui neid vajate. Sunnib väljaspool meie kontrolli mõnikord meile mõju viisil, mis põhjustavad planeerimata teenuse häirete.

Microsoft osutab selle teenuste teenindustaseme leping (SLA) kohustuse sees ja ühenduvuse. SLA Azure kohta leiate [Azure'i taseme lepingud](https://azure.microsoft.com/support/legal/sla/).

Azure'i on juba palju sisseehitatud platvormi funktsioone, mis toetavad tugevalt saadaolevad rakendused. Lisateavet lugege [kõrge-saadavus Azure rakenduste ja](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)teenuste kohta.

Selles artiklis antakse ülevaade true katastroof taastamise stsenaarium, kui kogu piirkonna kogemusi mõne katkestuste tõttu loodusõnnetus või levinud teenuse peatamine. Need on harv juhud, kuid peate ette võimalus on olemas ka katkestuste kogu piirkonna kohta. Kui kogu piirkonna teenuse häirete, kohalikult liigsete andmete eksemplarid ajutiselt oleks saadaval. Kui olete lubanud geo-dispersioonanalüüs, kolm täiendavad koopiat teie Azure Storage plekid ja tabelid on talletatud teises regioonis. Azure'i remaps korral täieliku piirkondliku katkestuste või mõne tõttu, esmane piirkond ei ole Taastatavad, kõik DNS-i kirjed geo kopeeritud alale.

>[AZURE.NOTE]Pange tähele, et teil on mis tahes juhtida seda toimingut ja see ainult esineda piirkond kogu teenuse häirete. Seetõttu tuleb ka toetuvad muude rakenduste kohased varukoopia strateegiad kättesaadavus kõrgeima taseme saavutamiseks. Lisateavet leiate jaotisest [andmete strateegiad katastroofiabi](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery)kohta.

Selleks et saaksite neid harvaesinevate juhud toime, pakume Azure'i virtuaalmasinates teenuse häirete ning kus Azure virtuaalse masina rakenduse juurutatakse järgmised juhised.

##<a name="option-1-wait-for-recovery"></a>Variant 1: Oodake, kuni taastamine
Sel juhul enam teie pole vaja midagi. Tea, et töötame hoolega taastamine teenuse kättesaadavus. Praeguse teenuse olek saate vaadata meie [Azure'i teenuste seisundi armatuurlaud](https://azure.microsoft.com/status/).

>[AZURE.NOTE]See on parim valik, kui te pole häälestanud Azure saidi taastamise, virtuaalse masina varundamise, lugemisõigus – geograafilise liigne salvestusruumi või geograafilise liigne salvestusruumi enne häireid. Kui olete loonud geograafilise liigne salvestusruumi või lugemisõigus – geograafilise liigne salvestusruumi salvestusruumi konto teie VM virtuaalne kõvaketas (VHDs) talletamiseks, saate vaadata, taastada pilti VHD ja proovige ettevalmistamise uute VM see. See ei ole eelistatud suvandi, sest pole sünkroonimise andmete garantiid, v.a juhul, kui kasutatakse Azure VM varundamine või Azure saidi taastamine. Seega see suvand on tagatud töö.

Klientidele, kes soovivad kohe juurde sellistele virtuaalmasinates, on saadaval järgmised kaks võimalust.  

>[AZURE.NOTE]Pange tähele, et mõlemad järgmistest suvanditest on võimalus teatud andmete kaotsimineku.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Variant 2: VM varukoopia põhjal taastamine
Klientidele, kes on konfigureeritud VM varukoopia, saate taastada selle varundamine ja taastamine punkti VM.

Uue VM Azure varukoopia põhjal taastamine, leiate [Azure'i virtuaalmasinates taastada](../backup/backup-azure-restore-vms.md).

Selleks et saaksite oma Azure'i virtuaalmasinates varukoopia taristu kavandamine, vaadake teemat [leping VM varukoopia taristu Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Võimalus 3: Algatada Tõrkesiirde Azure saidi taastamise abil
Kui olete konfigureerinud Azure saidi taastamise töötamiseks oma kannatavas Azure'i virtuaalmasinates, saate oma VMs taastamine nende koopiad. Need koopiad võivad asuda Azure või kohapealne. Sel juhul saate luua uue VM oma olemasolevat koopiat. On Azure saidi taastamise koopia oma VMs taastamiseks vaadake [migreerimine Azure'i IaaS virtuaalmasinates Azure saidi taastamine Azure'i piirkondade vahel](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Kuigi Azure virtuaalne arvuti operatsioonisüsteem ja andmete ketast kopeeritud teisene VHD, kui need on geograafilise liigne salvestusruumi või lugemisõigus – geograafilise liigne salvestusruumi konto, iga VHD kopeeritud sõltumatult. See tase kopeerimine ei garanteeri järjepidevuse kogu kopeeritud VHDs. Kui teie taotlus ja/või andmebaasidele, mis kasutavad ketast andmed on sõltuvused teineteisele, ei ole tagatud, et kõik VHDs ei kopeeritud ühe hetktõmmisena. Samuti ei ole tagatud, et VHD koopia geograafilise liigne salvestusruumi või lugemisõigus – geograafilise liigne salvestusruumi tulemuseks rakenduse ühtsete hetktõmmis VM käivitamiseks.

##<a name="next-steps"></a>Järgmised sammud
Kuidas rakendada katastroofiabi ja kõrge-saadavus strateegia kohta leiate lisateavet teemast [katastroofiabi ja Azure rakenduste kõrge kättesaadavus](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Arendamise üksikasjaliku tehnilise ülevaate pilve platvormi võimalusi, leiate [Azure'i paindlikkust tehnilised juhendid](../resiliency/resiliency-technical-guidance.md).

Varundamine VMs kohta leiate teavet leiate [Azure'i virtuaalmasinates varundamine](../backup/backup-azure-vms.md).

Saate teada, kuidas kasutada Azure saidi taastamise korraldab ja automatiseerida oma füüsilise (ja virtuaalse) Windows ja Linux masinad kaitse selle sõitma VMWare ja Hyper-V VMs leiate [Azure'i saidi taastamine](https://azure.microsoft.com/documentation/learning-paths/site-recovery/).

Kui juhiseid ei ole selge või kui soovite Microsoft teie nimel toimingute tegemiseks, pöörduge [Klienditoe poole](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
