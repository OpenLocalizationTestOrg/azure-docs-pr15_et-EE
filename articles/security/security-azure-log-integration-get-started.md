<properties
   pageTitle="Alustamine integreerimine Azure log | Microsoft Azure'i"
   description="Saate teada, kuidas installida Azure log integreerimine teenuse ja integreerida logid Azure storage, Azure'i auditilogid ja Azure turbekeskus teatised."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Alustamine integreerimine Azure log (eelvaade)

Azure'i log integreerimine võimaldab teil integreerida kohapealse Turbeteave ja sündmuse Management (SIEM) süsteemid töötlemata logid oma Azure ressurssidest. Selle integreerimine pakub ühendatud armatuurlaua oma varasid, kohapealse või pilves, nii, et saate liitmine, oleksid, analüüsida ja Teavita seostatud rakenduste turvalisus sündmuste jaoks.

Selles õpetuses juhendab teid kuidas installida Azure log integreerimine ja integreerida logid Azure storage, Azure'i auditilogid ja Azure turbekeskus teatised. Hinnanguline aega selles õpetuses on üks tund.

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse lõpuleviimiseks peab olema järgmised:

- Masina (kohapealse või pilves) integreerimine Azure Logi teenuse installimiseks. Selles seadmes peab töötama 64-bitise Windowsi Opsüsteemi .net installitud 4.5.1. Selles arvutis nimetatakse **Azlog integraator**.
- Azure'i tellimus. Kui te ei ole, mida saate registreeruda [tasuta konto](https://azure.microsoft.com/free/).
- Azure'i diagnostika lubatud Azure'i virtuaalmasinates (VMs). Pilveteenustega diagnostika lubamiseks leiate [Azure'i diagnostika lubada Azure pilveteenustega](../cloud-services/cloud-services-dotnet-diagnostics.md). Diagnostika Azure VM, mis töötab Windowsi jaoks, vaadake [Lubamiseks Azure diagnostika virtuaalse masina opsüsteemi Windows PowerShelli kasutamine](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Ühenduvuse Azlog integraatori Azure salvestusruumi ja autentimiseks ja lubada Azure tellimusse.
- Azure'i VM logide, SIEM agent (näiteks Splunk ühes kohas ekspediitor, HP ArcSight Windowsi sündmuste koguja agent või IBM QRadari WinCollect) peab olema installitud Azlog integraator.

## <a name="deployment-considerations"></a>Juurutamise kaalutlused

Mitme eksemplari Azlog integraator saate käivitada, kui sündmus helitugevus on kõrge. Koormusetasakaalustuseks *(WAD)* Windows Azure'i diagnostika salvestusruumi kontod ja anda linnanimede tellimuste arv põhinema oma võimsus.

8-protsessor (tuum) arvutis saab Azlog integraator ühekordsest protsessi umbes 24 miljonit sündmuste päevas (~1M/hour).

4-protsessor (tuum) arvutisse, saab Azlog integraator ühekordsest protsessi umbes 1,5 miljonit sündmuste päevas (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Installige Azure'i log integreerimine

Laadige [Azure'i logige integreerimine](https://www.microsoft.com/download/details.aspx?id=53324).

Integreerimine Azure Logi teenuse kogub telemeetria andmeid arvutist, kuhu on installitud.  Kogutud telemeetria andmed on:

- Erandid, mis ilmnevad integreerimine Azure log täitmine
- Mõõdikute kohta päringute ja töödeldud sündmuste arv
- Statistika kohta, milliseid Azlog.exe kasutatakse Käsurea võtmed

> [AZURE.NOTE] See suvand, eemaldades saate väljalülitamine telemeetria andmete kogumine.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Sujuv Azure VM logid kontodelt Azure'i diagnostika salvestusruum

1. Kontrollige, et eeltingimused tagada konto WAD salvestusruumi kogub logid enne jätkamist oma integreerimine Azure log eelnimetatud. Tehke järgmist, kui teie WAD salvestusruumi konto pole kogub logid.

2. Avage käsuviip ja **CD-le** üheks **c:\Program Files\Microsoft Azure'i Log integreerimine**.

3. Käivitage käsk

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Asendage StorageAccountName konfigureeritud diagnostika sündmuste saadud oma VM Azure storage konto nimi.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Kui soovite näidata üles, kui XML-i tellimuse id, lisa sõbralik nimi Tellimuse ID:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Oodake 30 – 60 minutit (see võib kuluda kuni tund) ja seejärel vaadata sündmusi, mis tõmmatakse salvestusruumi konto. Vaatamiseks avage **Sündmusevaatur > Windowsi logid > edastatud sündmuste** Azlog integraator kohta.

5. Veenduge, et teie standard SIEM konnektor arvutisse installitud konfigureeritud Valige kaustast **Edastatud sündmuste** sündmused ja toru neid oma SIEM eksemplari. Vaadake üle ja vaadata logid integreerimise konfigureerimiseks SIEM konfiguratsiooni.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Mida teha, kui andmed ei kuvata edastatud sündmuste kausta?

Kui tunni pärast andmete ei kuvata **Edastatud sündmuste** kausta, siis:

1. Kontrollige seadme ja veenduge, et see on juurdepääs Azure'i. Ühenduse testimiseks proovige avada [Azure portaali](http://portal.azure.com) brauseri kaudu.

2. Veenduge, et kasutaja konto **azlog** on kirjutamisõigust, klõpsake kausta **users\azlog**.

3. Veenduge, et lisatud **azlog allika lisamine** käsku salvestusruumi konto on loetletud käsk **azlog lähteloendis**käivitamisel.
4. Minge **Sündmusevaatur > Windowsi logid > rakenduse** integreerimine Azure log teatatud kuvamiseks, kui seal on vigu.

Kui te ikka ei näe sündmusi, siis:

1. Laadige [Microsoft Azure'i salvestusruumi Explorer](http://storageexplorer.com/).

2. Ühenduse loomine salvestusruumi kontole lisatud käsu **azlog allika lisada**.

3. Microsoft Azure'i salvestusruumi Exploreris, liikuge tabeli **WADWindowsEventLogsTable** , et kontrollida, kas kõik andmed. Kui ei, siis diagnostika VM pole õigesti konfigureeritud.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Sujuv Azure auditilogid ja turbekeskuse teatised

1. Avage käsuviip ja **CD-le** üheks **c:\Program Files\Microsoft Azure'i Log integreerimine**.

2. Käivitage käsk

        azlog createazureid

      See käsk palub teil Azure sisselogimiseks. Käsk loob siis [Azure Active Directory teenuse põhilise](../active-directory/active-directory-application-objects.md) Azure AD rentnikud majutavad sisselogitud kasutaja on administraator, koostöö administraator või omaniku Azure'i tellimused. Käsk nurjub, kui sisselogitud kasutaja on külalisena kasutaja Azure AD rentniku. Azure'i autentimine on lõpule jõudnud – Azure Active Directory (AD).  Teenuse põhilise Azlog integreerimiseks loomise loob Azure AD identiteedi, mis antakse juurdepääs lugemise Azure'i tellimused.

3. Käivitage käsk

        azlog authorize <SubscriptionID>

      Seda määrab lugeja Accessi põhisumma teenusega tellimusel loodud etappi 2. Kui te ei määra on SubscriptionID, siis avaldab kõik tellimused, millele teil on juurdepääs mis tahes teenuse põhisumma lugeja rolli määramine.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Hoiatused võidakse kuvada, kui käivitate käsu **lubada** kohe pärast **createazureid** käsu. On mõned latentsus Azure AD konto loomise ja kui konto on kasutamiseks saadaval. Kui te oodake umbes 10 sekundit pärast **createazureid** käsu käivitada käsu **autoriseerida** , siis pole näed need hoiatused.

4. Märkige ruut kinnitamiseks auditilogi logifailide JSON on järgmised kaustad:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Kontrollige kinnitada, et need olemas turbekeskus teatiste järgmistest kaustadest.

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Osutage standard SIEM faili ekspediitor konnektor kausta toru SIEM eksemplari andmed. Peate võib-olla mõni väli vastendused põhjal SIEM toote te kasutate.

Kui teil on küsimusi integreerimine Azure Log, saatke meile e-posti [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses te teada, kuidas installida Azure log integreerimine ja integreerida logid Azure salvestusruumist. Lisateabe saamiseks vaadake järgmist:

- [Microsoft Azure'i Log integreerimine Azure logide (eelvaade)](https://www.microsoft.com/download/details.aspx?id=53324) – allalaadimiskeskus üksikasjad, süsteeminõuded ja installi juhised integreerimine Azure Logi sisse.
- [Azure'i log integreerimine tutvustus](security-azure-log-integration-overview.md) – selle dokumendi tutvustab teile integreerimine Azure Logi selle võtme võimaluste ja kuidas see toimib.
- [Partneri konfigureerimistoimingute](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – sellest ajaveebipostitusest näitab, kuidas konfigureerida Azure log integreerimine partnerite lahenduste Splunk, HP ArcSight ja IBM QRadari töötamiseks.
- [Azure'i log integreerimine korduma kippuvad küsimused (KKK)](security-azure-log-integration-faq.md) – see KKK vastuseid küsimustele integreerimine Azure log.
- [Integrated Security Center teatiste Azure logige integreerimine](../security-center/security-center-integrating-alerts-with-log-integration.md) – selle dokumendi näitab, kuidas sünkroonida turbekeskus teatised, virtuaalse masina turvalisus sündmuste logi analytics või SIEM lahenduse Azure'i diagnostika- ja Azure auditilogid, kogutud koos.
- [Uued funktsioonid Azure diagnostika- ja Azure auditilogid](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – sellest ajaveebipostitusest tutvustab teile Azure'i auditilogid ja muud funktsioonid, mis aitavad teil saada toimingute Azure ressursside ülevaate.
