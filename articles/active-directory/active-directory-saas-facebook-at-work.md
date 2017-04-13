<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Facebooki tööl | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida tööl ühekordse sisselogimise Azure Active Directory ja Facebooki vahel."
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
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Õpetus: Azure'i Active Directory tööl Facebooki integreerimine

Selle õpetuse eesmärk näitavad, kuidas tööl Facebooki integreerimine Azure Active Directory (Azure AD).

Tööl Facebooki integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs Facebooki tööl 
- Te saate automaatselt ette kasutajatele, kellel on juurdepääs antud tööl Facebooki konto
- Saate lubada kasutajatele automaatselt saada allkirjastatud-on Facebooki tööl (ühekordse sisselogimise) koos oma Azure AD kontod
- Saate hallata oma kontosid ühes keskses kohas 

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Eeltingimused 

Azure AD integreerimise konfigureerimiseks CS tärni vajate järgmist:

- Tellimuse Azure AD
- Facebooki tööl koos ühekordse sisselogimise lubatud

Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Lisada Facebooki tööl galeriist
Facebooki integreerimine Azure AD üheks tööl konfigureerimiseks tuleb kõigepealt lisada Facebooki tööl galeriist hallatavate SaaS rakenduste loendisse.

**Lisada Facebooki galeriist tööl, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
    
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Facebooki tööl**.

7. Tulemuste paanil valige **Facebooki tööl**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .


### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises kirjeldatakse, kuidas kasutajad saavad autentimiseks Facebooki tööl oma konto Azure Active Directory federation SAML protokolli abil.

**Konfigureerida Azure AD ühekordse sisselogimise Facebook tööl, tehke järgmist.**

1.  Pärast lisamist Facebooki Azure klassikaline portaalis tööl, klõpsake **Konfigureerimine ühekordse sisselogimise**.

2.  Sisestage URL **Rakenduse URL-i konfigureerimine** ekraanil, kui kasutajad on töö rakendamisel Facebooki sisse logida. See on teie Facebooki töö rentniku URL (näide: https://example.facebook.com/). Kui olete lõpetanud, klõpsake nuppu **edasi**.

3.  Erinevate web brauseriaknas, logige sisse oma Facebooki töö ettevõtte saidil administraatorina.

4. Järgige juhiseid konfigureerimiseks Facebooki tööl kasutada Azure AD identiteedi pakkuja järgmine URL: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Täidetud naasmiseks brauseris windows Azure'i klassikaline portaali nähtaval, ruut, et kinnitada, et olete täitnud juhiseid ja seejärel klõpsake nuppu **edasi** ja **valmis**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automaatselt ettevalmistamise kasutajad Facebooki tööl

Azure AD toetab automaatselt sünkroonib võimalus määratud kasutajate töö Facebooki konto üksikasjad. See automaatne sychronization võimaldab Facebooki tööl saamiseks tuleb lubada kasutajate juurdepääsu enne nende esimest korda sisselogimine katsel andmed. See ka tühistage sätted kasutajad Facebooki tööl, kui juurdepääsu on tühistatud Azure AD.

Automaatse ettevalmistamise saate häälestada, klõpsates nuppu **Konfigureeri konto ettevalmistamise** klassikaline Azure portaali aknas.

Automaatse ettevalmistamise konfigureerimise kohta lisateabe saamiseks vt [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Facebooki tööl kasutajate määramine

Ettevalmistatud AAD kasutajad saaksid neid vaadata Facebooki tööl oma Accessi paneeli, ta peab olema määratud juurdepääsu Azure klassikaline portaali sees.

**Facebooki tööl kasutajate määramine**

1.  Klõpsake avalehe Facebooki tööl Azure klassikaline portaalis, **määrata kontod**.

2.  Menüü **kuvamiseks** valige, kas soovite määrata tööl Facebooki kasutaja või rühma ja klõpsake nuppu märge.

3.  Valige loend, kasutaja või rühma nimi, kellele soovite määrata Facebooki tööl.

4.  Lehe jalus nuppu **Määra** .


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




