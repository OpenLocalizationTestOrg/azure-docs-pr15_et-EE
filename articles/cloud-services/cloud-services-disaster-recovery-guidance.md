<properties
    pageTitle="Mida korral on Azure teenuse häirete, mis mõjutab Azure'i pilveteenustega | Microsoft Azure'i"
    description="Vaadake, mida teha korral Azure teenuse häirete, mis mõjutab Azure'i pilveteenustega."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Mida teha korral on Azure teenuse häirete, mis mõjutab Azure'i pilveteenustega

Microsofti töötame selle nimel, veenduge, et meie teenuste on alati saadaval, kui neid vajate. Sunnib väljaspool meie kontrolli mõnikord meile mõju viisil, mis põhjustavad planeerimata teenuse häirete.

Microsoft osutab selle teenuste teenindustaseme leping (SLA) kohustuse sees ja ühenduvuse. SLA Azure kohta leiate [Azure'i taseme lepingud](https://azure.microsoft.com/support/legal/sla/).

Azure'i on juba palju sisseehitatud platvormi funktsioone, mis toetavad tugevalt saadaolevad rakendused. Lisateavet lugege [kõrge-saadavus Azure rakenduste ja](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)teenuste kohta.

Selles artiklis antakse ülevaade true katastroof taastamise stsenaarium, kui kogu piirkonna kogemusi mõne katkestuste tõttu loodusõnnetus või levinud teenuse peatamine. Need on harv juhud, kuid peate ette võimalus on olemas ka katkestuste kogu piirkonna kohta. Kui kogu piirkonna teenuse häirete, kohalikult liigsete andmete eksemplarid ajutiselt oleks saadaval. Kui olete lubanud geo-dispersioonanalüüs, kolm täiendavad koopiat teie Azure Storage plekid ja tabelid on talletatud teises regioonis. Azure'i remaps korral täieliku piirkondliku katkestuste või mõne tõttu, esmane piirkond ei ole Taastatavad, kõik DNS-i kirjed geo kopeeritud alale.

>[AZURE.NOTE]Pange tähele, et teil on mis tahes juhtida seda toimingut ja see ainult esineda andmekeskuse kogu teenuse häirete. Seetõttu tuleb ka toetuvad muude rakenduste kohased varukoopia strateegiad kättesaadavus kõrgeima taseme saavutamiseks. Lisateavet leiate jaotisest [andmete strateegiad katastroofiabi](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR)kohta. Kui soovite võib mõjutada oma Tõrkesiirde, mida soovite [lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), mis loob andmete kirjutuskaitstud koopia teises regioonis kasutamise.

Aitavad toime nende harvaesinevate juhud, pakume järgmised juhised Azure'i virtuaalmasinates (VM) teenuse häirete ning kus juurutatakse Azure VM rakenduse.

##<a name="option-1-wait-for-recovery"></a>Variant 1: Ootamine taastamine
Sel juhul enam teie pole vaja midagi. Leida Azure meeskondadel hoolega töötamise teenuse kättesaadavus taastamiseks. Praeguse teenuse olek saate vaadata meie [Azure'i teenuste seisundi armatuurlaud](https://azure.microsoft.com/status/).

>[AZURE.NOTE]See on parim valik, kui kliendi pole häälestanud Azure saidi taastamise või on sekundaarne juurutamise teises regioonis.

Klientidele, kes soovivad kohe juurdepääsu oma juurutatud pilveteenustega, on saadaval järgmised suvandid.

>[AZURE.NOTE]Pange tähele, et need suvandid on võimalus teatud andmete kaotsimineku.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Variant 2: Uuesti juurutada uue regioon oma pilvepõhise teenuse konfigureerimine

Kui teil on oma algse koodi, saate lihtsalt ümberkorraldamine taotlus, seotud konfiguratsiooni ja seotud ressursid uue piirkonnas uue pilveteenusesse.  

Täpsemat teavet selle kohta, kuidas luua ja juurutada cloud teenuserakenduse, vaadake, [Kuidas luua ja juurutada mõnda pilveteenusesse](./cloud-services-how-to-create-deploy-portal.md).

Sõltuvalt rakenduse andmeallikate peate rakenduse andmeallika taastamine toiminguid kontrollida.
  * Azure Storage andmeallikad, leiate [Azure'i salvestusruumi dispersioonanalüüs](../storage/storage-redundancy.md#read-access-geo-redundant-storage) kontrollida saadaolevad suvandid valige dispersioonanalüüs vormi rakenduse.
  * SQL-andmebaasi allikatest, lugege [Ülevaade: Cloud business järjepidevus ja andmebaasi Õnnetusejärgne taastamine SQL-andmebaasi](../sql-database/sql-database-business-continuity.md) kontrollida vastavalt valitud dispersioonanalüüs mudel rakenduse suvandid, mis on saadaval.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Võimalus 3: Kasutada varukoopia juurutamise kaudu Azure'i liikluse haldur
See suvand eeldab, et teil on juba loodud teie rakenduse lahendus piirkondliku avariitaastet meeles. Kui teil on juba teisene cloud services rakenduse juurutamine, mis teises regioonis töötab ja liikluse haldur kanali kaudu ühendatud, saate selle suvandi. Sel juhul sekundaarse juurutamise seisundi kontrollimiseks. Kui see on terve, saate suunata liiklus selle kaudu Azure'i liikluse haldur. Selle strateegia abil saate ära ning liikluse marsruutimise meetod Tõrkesiirde tellimuse konfiguratsioone Azure'i liikluse halduris. Lisateavet leiate teemast [liikluse haldur sätete konfigureerimine](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Azure'i pilveteenustega tasakaalustamiseks piirkondade Azure'i liikluse haldur](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Järgmised sammud

Kuidas rakendada katastroofiabi ja kõrge-saadavus strateegia kohta leiate lisateavet teemast [katastroofiabi ja Azure rakenduste kõrge kättesaadavus](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Arendamise üksikasjaliku tehnilise ülevaate pilve platvormi võimalusi, leiate [Azure'i paindlikkust tehnilised juhendid](../resiliency/resiliency-technical-guidance.md).

Kui juhiseid ei ole selge, või kui soovite Microsoft teie nimel toimingute tegemiseks pöörduge [Klienditoe poole](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
