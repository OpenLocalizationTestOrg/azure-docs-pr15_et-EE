<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine kohandatava komplekti | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory kohandatava komplekti kasutamine!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Õpetus: Azure'i Active Directory integreerimine kohandatava komplekti

Selle õpetuse eesmärk on integreerimine Azure ja kohandatava komplekti kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Kohandatavad komplekti rentnikku

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud kohandatava komplektile ühekordse sisselogimise rakendusse kohandatava komplekti ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise kohandatava tarkvarakomplekti lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Rakenduste integreerimise kohandatava tarkvarakomplekti lubamine

Selle jaotise eesmärk on liigendamine kohandatava tarkvarakomplekti rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Rakenduste integreerimise kohandatava tarkvarakomplekti lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Kohandatava komplekti**.

    ![Rakenduse Galerii] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Kohandatava komplekti**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Kohandatavad komplekti] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Kohandatavad komplekti")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks kohandatava komplektile oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise kohandatava tarkvarakomplekti konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **Kohandatava komplekti** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate kohandatava komplekti logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse sätete konfigureerimine** tekstiväljale **Vastus URL-i** , tippige oma URL-i abil järgmise mustriga "*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*", ja seejärel klõpsake nuppu **edasi**.

    >[AZURE.NOTE] Saate selle väärtuse kohandatava komplekti **SAML SSO sätete** lehe kaudu.

    ![Rakenduse sätete konfigureerimine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Rakenduse sätete konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise kohandatava komplekti veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**. salvestage serdi fail teie arvutile

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil kohandatava komplekti ettevõtte administraatorina.

6.  Valige **administraator**.

    ![Administraator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administraator")

7.  Klõpsake jaotises **kasutajate ja rollide** **Haldamine SAML SSO sätted**.

    ![SAML SSO sätete haldamine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "SAML SSO sätete haldamine")

8.  Lehel **SAML SSO sätted** tehke järgmist.

    ![SAML SSO sätted] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO sätted")

    1.  **Identiteedi pakkuja nimi** väljale Tippige oma konfiguratsioon.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil kohandatava komplekti** dialoogiboksi **Üksuse ID** väärtus kopeerida ja seejärel kleepige see **identiteedipakkuja üksuse ID** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil kohandatava komplekti** dialoogiboksi kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige see **identiteedipakkuja SSO URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil kohandatava komplekti** dialoogiboksi kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **URL-i kohandatud välju** tekstiväli.
    5.  Laadige oma allalaaditud serdi, klõpsake **faili valimine**.
    6.  **SAML kasutaja ID-d**, valige **kasutaja kohandatava ülevaateid kasutaja nimi**.
    7.  **SAML kasutaja id asukoht**, valige **teemareal NameID kasutaja ID-d**.
    8.  **SAML NameID vorming**, valige **meiliaadress**.
    9.  Nimega **SAML lubamine**, valige **Luba SAML SSO ja otsese kohandatava ülevaateid Logi sisse**.
    10. Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate kohandatava komplekti sisse logida, nad peavad olema ettevalmistatud kohandatava komplekti.  
Puhul kohandatava tarkvarakomplekt ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **Kohandatava komplekti** ettevõtte administraatorina.

2.  Valige **administraator**.

    ![Administraator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administraator")

3.  **Kasutajate ja rollide** jaotises nuppu **Lisa kasutaja**.

    ![Kasutaja lisamine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Kasutaja lisamine")

4.  Jaotises **Uus kasutaja** , tehke järgmist.

    ![Vormiandmete edastamine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Vormiandmete edastamine")

    1.  Tippige **nimi**, **Login**, **e-posti**, soovite seotud tekstiväljad säte kehtiv Azure Active Directory kasutaja **parooli** .
    2.  Valige **roll**.
    3.  Klõpsake **esitada**.

>[AZURE.NOTE] Saate kasutada mis tahes muud kohandatava komplekti kasutaja konto loomise tööriistade või API-de osutavad kohandatava komplekti kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Kohandatava komplekti kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake **Kohandatava komplekti **rakenduse integreerimise lehel **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
