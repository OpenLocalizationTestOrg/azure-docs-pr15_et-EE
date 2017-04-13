
<properties
    pageTitle="Azure Active Directory rentnikule Azure RemoteApp muutmine | Microsoft Azure'i"
    description="Saate teada, kuidas muuta Azure Active Directory rentniku seostatud Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Azure Active Directory rentniku Azure RemoteApp muutmine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kasutajate juurdepääsu Azure RemoteApp kasutab Azure Active Directory (Azure AD). Ainult Azure AD rentniku, mida saate kasutada Azure RemoteApp on Azure tellimusega seotud. Saate vaadata tellimusega seotud portaalis lehel **sätted** . Vaadake veergu **Directory** menüü **tellimused** .

> [AZURE.NOTE] Selle muudatuse õnnestub, esmalt olemasoleva Azure Active Directory rentniku: kõik Azure RemoteApp saidikogumid kõik kasutajad eemaldada. Selleks valige Azure portaali, avage vahekaart **Azure RemoteApp** ja Azure RemoteApp iga saidikogumi. Minge menüüsse **kasutajate** ja kasutajate, mis kuuluvad teie praegune Azure Active Directory rentniku eemaldamine. Korrake kõiki olemasolevaid Azure RemoteApp saidikogumid. Ei tee seda, mida ei saa luua või paik saidikogumid.

Kui soovite kasutada erinevaid rentniku, tehke järgmist, et teie tellimus:

1. Eemaldage portaalis Azure AD kasutajad, kellele olete andnud juurdepääsu Azure RemoteApp saidikogumid. (Vt märkus ülaltoodud juhised kohta, kuidas seda teha.)


2. Teenuse administraatori seatud Microsofti kontoga (varem Live ID). (Ei tea, kui olete juba teenuse administraator? Te saate teada, klõpsates **Sätted -> administraatorid**.) Nüüd siin on, kuidas muuta mis:
    1. Klõpsake paremas ülanurgas kasutaja ja klõpsake **arve kuvamine**.
    2. Klõpsake tellimust. Klõpsake lehel uus Kerige alla ja paremas **tellimuse üksikasjade redigeerimine** nuppu. (Sortimine teise eesnime alumise right, kui see aitab teil selle leida.)
    3. Tippige Microsofti konto kasutaja, mis peaks olema teenuse administraator.

3. Nüüd väljalogimine portaali ja seejärel logige uuesti sisse Microsofti kontoga, mis on määratud eelmises etapis.


4. Klõpsake **Uus rakendus -> teenuste -> aktiivne Directory -> Directory -> kohandatud loomine**.
5. Valige jaotises **kataloogi** **kasutamine olemasoleva kausta**. Meil on välja logida, saate portaali kohe, et valida **olen valmis nüüd välja logitud**.
6. Logige uuesti portaali sisse administraatorina globaalse kataloogi, mille soovite lisada. (Kui te ei ole juba üldadministraator, saab pärast ringi, logige sisse ja seejärel logige välja.)
7. Peate sisestama sisselogimisel kui soovite kasutada oma olemasoleva AD rentniku tellimus. Klõpsake nuppu **Jätka**ja seejärel klõpsake nuppu **Logi välja kohe**.
5. Uuesti ja minge tagasi **Sätted -> tellimused**. Valige oma tellimuse ja seejärel klõpsake nuppu **Redigeeri Directory**. Valige Azure AD rentniku, mida soovite kasutada.



Saate kohe kasutada uut Azure AD rentniku Azure tellimuse juurdepääsu piiramiseks ja konfigureerida kasutajajuurdepääsu Azure RemoteApp.
