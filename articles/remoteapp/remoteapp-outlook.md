<properties
    pageTitle="Outlooki kasutamine Azure RemoteApp | Microsoft Azure'i" 
    description="Saate teada, kuidas konfigureerimine ja kasutamine Outlooki Azure RemoteApp | Microsoft Azure'i"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Microsoft Outlooki kasutamine Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp toetab Microsoft Outlooki O365. Lugege rohkem teavet [Office töötab Azure RemoteApp](remoteapp-officesubscription.md). On paar Soovitatavate sätete Outlooki Azure RemoteApp kasutamisel.

## <a name="cached-mode"></a>Vahemälurežiim
Vahemälurežiim on soovitatav konfiguratsioon, kui kasutate Outlooki Azure RemoteApp. Kui konfigureerite Exchange'i vahemälurežiimi kasutamiseks konto Outlook 2013, Outlook 2013 töötab kasutaja Microsoft Exchange'i postkasti ühenduseta andmefaili (OST-fail) koos selle ühenduseta aadressiraamatu (OAB) kasutaja arvutisse salvestatud kohaliku eksemplari. Vahemällu talletatud postkasti ja OAB uuendatakse perioodiliselt O365 teenus. Lugege lisateavet [vahemällu talletatud ja Online'i režiimi erinevuste](https://technet.microsoft.com/library/jj683103.aspx)kohta.

Kasutaja saab valida **Exchange'i vahemälurežiimi** või **Ühendusega režiimi** konto häälestamise ajal või muutke konto sätteid. Office'i kohandamise tööriista (okt) või rühmapoliitika abil saate juurutada režiimi ühe või teise.  

Lugege [juhiseid samm-sammult vahemälurežiim lubamine](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Otsing
Azure RemoteApp, kasutades Outlooki otsing on piiratud. Azure RemoteApp kasutab mahutamiseks kasutaja seansside ühise VMs. Otsingu indekseerimisest sõltub sellest, kas arvuti ID, mis on erinev eri VMs. On võimalik, et iga kord, kui kasutaja logib Azure RemoteApp, see on esitatud uue VM. Mida tähendab, kui kohalik otsing lubamiseks selle indekseerija käivitatakse iga kord, kui seadme ID muudab (kui kasutaja on erinevate VM). Sõltuvalt suurus soovitud. OST-fail, selle indekseerija võib võtta kaua aega ja ressursse, muude rakenduste häälestamine. Otsing ei oleks ainult aeglane, kuid võib tulemusi. Ühendusega režiimi konto profiili abil töötavad selle ümber, ent üldise jõudluse tekib tõttu kohaliku vahemälu (vaadake lisateavet vahemällu talletatud ja online režiimis vahe linki). Kahjuks ei saa keelata indekseeritud kohalik otsing ja veebiotsingu ei saa lubatud vaikimisi rakenduses Outlook 2013.
