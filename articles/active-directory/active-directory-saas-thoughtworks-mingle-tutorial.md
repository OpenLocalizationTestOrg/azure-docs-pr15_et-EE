<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Thoughtworks segunema | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory Thoughtworks segunema kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Õpetus: Azure'i Active Directory integreerimine Thoughtworks segunema
  
Selle õpetuse eesmärk on integreerimine Azure ja Thoughtworks segunema kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Thoughtworks segunema rentniku jaoks
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Thoughtworks segunema lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Rakenduste integreerimise jaoks Thoughtworks segunema lubamine
  
Selle jaotise eesmärk on liigendamine Thoughtworks segunema jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Thoughtworks segunema lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **thoughtworks minna**.

    ![Rakenduse Galerii] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Thoughtworks segunema**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Thoughtworks minna] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks minna")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Thoughtworks segunema oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus on vaja serdi üleslaadimine Thoughtworks segunema.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Thoughtworks segunema **rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida Thoughtworks segunema** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Thoughtworks minna rentniku URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.mingle.thoughtworks.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise Thoughtworks segunema veebisaidil** klõpsake nuppu Laadi alla metaandmete ja seejärel salvestage see oma arvutisse.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Konfigureerimine Ühekordne sisselogimine")

5.  Logige saidile **Thoughtworks segunema** ettevõtte administraatorina.

6.  Klõpsake **vahekaarti** ja klõpsake **SSO Config**.

    ![SSO Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO Config")

7.  Jaotises **SSO Config** , tehke järgmist.

    ![SSO Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO Config")

    1.  Metaandmete faili üleslaadimiseks klõpsake nuppu **Vali fail**.
    2.  Klõpsake nuppu **Salvesta muudatused**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
AAD kasutajad saaksid sisse logida, ta peab olema ette valmistatud Thoughtworks segunema rakendusse kasutajanime Azure Active Directory abil.  
Puhul Thoughtworks segunema, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile Thoughtworks segunema ettevõtte administraatorina.

2.  Klõpsake nuppu **profiili**.

    ![Esimese projekti] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Esimese projekti")

3.  Klõpsake **vahekaarti** ja seejärel klõpsake nuppu **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Kasutajatele")

4.  Klõpsake nuppu **Uus kasutaja**.

    ![Uus kasutaja] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Uus kasutaja")

5.  Klõpsake lehel **Uus kasutaja** dialoogiboksis tehke järgmist:

    ![Uus kasutaja] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Uus kasutaja")

    1.  Tippige **sisselogimisnimi**, **kuvatav nimi**, **Valige parooli**, soovite seotud tekstiväljad säte kehtib AAD konto **parooli kinnitus** .
    2.  **Kasutaja tüüp**, valige **täielik kasutaja**.
    3.  Klõpsake **selle profiili loomine**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Thoughtworks segunema kasutaja konto loomise tööriistade või API-de esitatud Thoughtworks segunema kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Thoughtworks segunema kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Thoughtworks segunema** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
