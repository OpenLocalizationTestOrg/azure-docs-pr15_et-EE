<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Thirdlight | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Thirdlight abil!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Õpetus: Azure'i Active Directory integreerimine Thirdlight
  
Selle õpetuse eesmärk on integreerimine Azure ja Thirdlight kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Thirdlight ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Thirdlight ühekordse sisselogimise rakendusse Thirdlight ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Thirdlight lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Rakenduste integreerimise jaoks Thirdlight lubamine
  
Selle jaotise eesmärk on liigendamine Thirdlight jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Thirdlight lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Thirdlight**.

    ![Rakenduse Galerii] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Thirdlight**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Thirdlight oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Thirdlight konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Thirdlight** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Thirdlight logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Thirdlight sisselogimise URL** väljale Tippige oma URL-i kasutavad kasutajad logida Thirdlight rakenduse (nt: "*http://azuresso2.thirdlight.com/*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Thirdlight veebisaidil** oma metaandmete allalaadimiseks klõpsake nuppu **allalaadimine metaandmete**seejärel salvestage fail metaandmete kohapeal arvutisse

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Thirdlight ettevõtte administraatorina.

6.  Minge **konfiguratsiooni \> süsteemi halduse**, ja seejärel klõpsake käsku **SAML2**.

    ![Süsteemi haldamine] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Süsteemi haldamine")

7.  SAML2 konfigureerimine jaotises tehke järgmist.

    ![SAML Ühekordne sisselogimine] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML Ühekordne sisselogimine")

    1.  Valige **SAML2 ühekordse sisselogimise lubamiseks**.
    2.  **Andmeallika IdP metaandmete jaoks**, valige **Laadi IdP metaandmete XML-ist**.
    3.  Metaandmete allalaaditud faili avamiseks, kopeerige sisu ja seejärel kleepige **IdP metaandmete XML-i** tekstiväli.
    4.  Klõpsake nuppu **Salvesta SAML2 sätted**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Thirdlight sisse logida, ta peab olema ettevalmistatud Thirdlight.  
Puhul Thirdlight, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Thirdlight** ettevõtte saidi administraator.

2.  Avage menüü **Kasutajad** .

3.  Valige **kasutajad ja rühmad**.

4.  Klõpsake nuppu **Lisa uus kasutaja** .

5.  Sisestage **kasutajanimi, nimi või kirjeldus, e-posti, valige valmissäte või rühma uute liikmete** kehtiv AAD konto, mida soovite näha.

6.  Klõpsake nuppu **Loo**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Thirdlight kasutaja konto loomise tööriistade või API-de esitatud Thirdlight AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Thirdlight kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Thirdlight **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).