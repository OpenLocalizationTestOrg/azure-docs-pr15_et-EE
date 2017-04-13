<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Evidence.com | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Evidence.com vahel."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Õpetus: Azure'i Active Directory integreerimine Evidence.com

Selle õpetuse eesmärk on näidata, kuidas häälestada ühekordse sisselogimise Azure Active Directory (AAD) ja Evidence.com vahel. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:
    
* Microsoft Azure'i kehtiv tellimus
* Tellimuse Evidence.com koos ühekordse sisselogimise lubatud (e-posti earlyaccess@evidence.com kui SAML-põhiste ühekordse sisselogimise pole lubatud)

Pärast selle õpetuse, saab AAD kasutajad, kellele olete määranud Evidence.com Accessi ühekordse sisselogimise AAD Accessi paneeli abil rakendusse.

## <a name="add-evidencecom-to-your-directory"></a>Kataloogi Evidence.com lisamine

Selles jaotises kirjeldatakse, kuidas lisada Evidence.com Azure Active Directory integreeritud rakendusena.

**Rakenduste integreerimise tõendusmaterjalide kogumiseks lubamiseks tehke järgmist.**

1.  [Azure'i klassikaline portaalis](https://manage.windowsazure.com), klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

4.  Rakenduse Galerii avamiseks klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Lisa rakendus galeriist**.

5.  Tippige otsinguväljale **Evidence.com**.

6.  Tulemuste paanil valige **Evidence.com**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .


## <a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selles jaotises kirjeldatakse, kuidas kasutajad saavad Evidence.com autentimiseks ja Azure Active Directory federation SAML protokolli abil oma konto.

**Ühekordse sisselogimise konfigureerimiseks tehke järgmist.**

1.  Pärast lisamist Evidence.com Azure klassikaline portaalis, klõpsake **Konfigureerimine ühekordse sisselogimise**. 
 
2.  Järgmisel kuval Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

3.  Rakenduse URL-i konfigureerimine ekraanil, sisestage URL, kus kasutajad saavad Evidence.com rentniku URL-i sisse logida (näide: https://yourtenant.evidence.com ja seejärel klõpsake nuppu **edasi**. 

4.  **Laadige alla serdi** linki ja salvestage see arvuti kohalikule kettale. Häälestada SSO Evidence.com saidil kasutatakse seda serti ja metaandmete URL-id (üksuse ID, SSO sisselogimise URL ja Logi välja URL-i). 

5.  Web eraldi brauseriaknas sisselogimiseks oma Evidence.com rentniku administraatorina ja liikuge menüü **administraator**
      
6.  Klõpsake **asutuse ühekordse sisselogimise** kohta
 
7.  Valige **SAML põhjal ühekordse sisselogimise**
 
8.  Kopeerige **Väljaandja URL-i**Azure klassikaline portaali ja vastava välja Evidence.com **Ühekordse sisselogimise** ja **Ühe Logi välja** väärtused.

9.  Avatud serdi allalaaditud etapis 4 tekstiredaktoris, nt Notepad.exe, ning kopeerige ja kleepige väljale **Turbeserdiga** sisu. 

10. Salvestage konfiguratsiooni Evidence.com.
 
11. Azure'i klassikaline portaalis, märkige ruut **Kinnita, mille olete konfigureerinud ühekordse sisselogimise eespool kirjeldatud**. Selle ruudu võimaldab praeguse serdi alustada tööd selle rakenduse jaoks.
 
12. Ühekordse sisselogimise kinnitamise lehel nuppu **valmis**.  


## <a name="creating-an-evidencecom-test-user"></a>Loomise rakendust Evidence.com test

Azure AD kasutajad saaksid sisse logida, peab ta ettevalmistatud juurdepääsu Evidence.com rakenduse sees. Selles jaotises kirjeldatakse, kuidas sees Evidence.com Azure AD kasutajakontode loomine

**Ettevalmistamise kasutaja konto Evidence.com:**

1.  Logige veebibrauseri akna, saidile Evidence.com ettevõtte administraatorina.

2.  Avage menüü **administraator** .

3.  Klõpsake **Lisa kasutaja**.

4.  Klõpsake nuppu **Lisa** .

5.  **E-posti aadress** lisatud kasutaja peab vastama kasutajate kasutajanimi Azure AD, kellele soovite anda juurdepääsu. Kui kasutajanime ja meiliaadressi pole teie asutuse sama väärtuse, saate kasutada funktsiooni **Evidence.com > Atribuudid > ühekordse sisselogimise** Azure klassikaline portaali nameidenitifer, mis on saadetud Evidence.com e-posti aadressi muutmiseks.


## <a name="assigning-users-to-evidencecom"></a>Kasutajate määramine Evidence.com

Ettevalmistatud AAD kasutajad saaksid näha oma Accessi paneeli Evidence.com, ta peab olema määratud juurdepääsu Azure klassikaline portaali sees.

**Evidence.com kasutajate määramine**

1.  Klõpsake Lühijuhend leht Evidence.com Azure klassikaline portaalis, **määrata kasutajatele Evidence.com**.
 
2.  **Kuvatakse** menüü, valige, kas soovite määrata Evidence.com kasutaja või rühm ja klõpsake nuppu märge.
 
3.  Valige loendis **Kasutajad** kasutajate rühma nimi, kellele soovite määrata Evidence.com.
 
4.  Lehe jalus nuppu **Määra** .

