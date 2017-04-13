<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Kevad CM | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Kevad CM Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Õpetus: Azure'i Active Directory integreerimine Kevad CM
  
Selle õpetuse eesmärk on näidata, kuidas häälestada ühekordse sisselogimise Azure Active Directory ja SpringCM vahel.
  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   SpringCM ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, olete määranud SpringCM Azure Active Directory kasutajad saab ühekordse sisselogimise AAD Accessi juhtpaneeli kaudu.

1.  Rakenduste integreerimise jaoks SpringCM lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-springcm"></a>Rakenduste integreerimise jaoks SpringCM lubamine
  
Selle jaotise eesmärk on liigendamine SpringCM jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Rakenduste integreerimise jaoks SpringCM lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **SpringCM**.

    ![Rakenduse Galerii] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **SpringCM**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selles jaotises kirjeldatakse, kuidas kasutajad saavad SpringCM autentimiseks ja Azure Active Directory federation SAML protokolli abil oma konto.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **SpringCM** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate SpringCM logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **SpringCM Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad SpringCM rakenduse logida ja seejärel klõpsake nuppu **edasi**. 

    Rakenduse URL on SpringCM rentniku URL-i (nt: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise SpringCM veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage fail serdi kohalikku arvutisse.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Ühe logima konfigureerimine")

5.  Erinevate web brauseriaknas sisse logida saidile **SpringCM** ettevõtte administraatorina.

6.  Menüü peal, käsku **Mine**, klõpsake käsku **eelistused**ja klõpsake jaotises **Kontoeelistused** **SAML SSO-d**.

    ![SAML SSO-d] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO-d")

7.  Identiteedi pakkuja konfigureerimine jaotises tehke järgmist.

    ![Identiteedi pakkuja konfigureerimine] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identiteedi pakkuja konfigureerimine")

    1.  Klõpsake oma Azure Active Directory allalaaditud serdi üleslaadimine **Serdi väljaandja valimine** või **Muutmine väljaandja sert**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil SpringCM** kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **väljaandja** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil SpringCM** kopeerige **Singel sisselogimise URL-i** väärtus ning kleepige need **Teenuse pakkuja (SP) algatatud lõpp-punkti** tekstiväli.
    4.  Kui **SAML lubatud**, valige **Luba**.
    5.  Klõpsake nuppu **Salvesta**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Ühe logima konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure Active Directory kasutajate SpringCM sisse logida, nad peavad olema ettevalmistatud SpringCM.  
Puhul SpringCM, ettevalmistamise on käsitsi ülesanne.

>[AZURE.NOTE] Lisateavet leiate teemast [loomine ja redigeerimine SpringCM kasutaja](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Ettevalmistamise SpringCM kasutajakonto, tehke järgmist.

1.  Logige saidile **SpringCM** ettevõtte administraatorina.

2.  Klõpsake nuppu **Mine**ja seejärel klõpsake nuppu **Aadressiraamat**.

    ![Kasutaja loomine] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Kasutaja loomine")

3.  Klõpsake nuppu **Loo kasutaja**.

4.  Valige **kasutaja roll**.

5.  Valige **aktiveerimise meilisõnumeid saata**.

6.  Tippige eesnimi, perekonnanimi ja e-posti aadressi soovite seotud tekstiväljad säte kehtib Azure Active Directory kasutajakonto.

7.  **Turberühma**kasutaja lisamiseks.

8.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud SpringCM kasutaja konto loomise tööriistade või API-de esitatud SpringCM AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>SpringCM kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **SpringCM** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).




