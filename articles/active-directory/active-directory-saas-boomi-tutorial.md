<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Boomi | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Boomi kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Õpetus: Azure'i Active Directory integreerimine Boomi

Selle õpetuse eesmärk on integreerimine Azure ja Boomi kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Boomi ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Boomi ühekordse sisselogimise rakendusse Boomi ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Boomi lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-boomi"></a>Rakenduste integreerimise jaoks Boomi lubamine

Selle jaotise eesmärk on liigendamine Boomi jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Boomi lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Boomi**.

    ![Rakenduse Galerii] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Boomi**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Boomi oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Boomi** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida Boomi** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Boomi vasta URL** väljale Tippige oma **Boomi AtomSphere sisselogimise URL-i** (nt: "*https://platform.boomi.com/sso/AccountName/saml*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Boomi veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Boomi ettevõtte administraatorina.

6.  Klõpsake tööriistaribal ülaosas oma ettevõtte nime ja seejärel **häälestus**.

    ![Häälestamine] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Häälestamine")

7.  Klõpsake **SSO suvandid**.

    ![SSO suvandid] (./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO suvandid")

8.  **Ühekordse sisselogimise suvandid** jaotises tehke järgmist.

    ![Ühekordse sisselogimise suvandid] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Ühekordse sisselogimise suvandid")

    1.  Valige **SAML ühekordse sisselogimise lubamiseks**.
    2.  Klõpsake nuppu **Import**üles laadida allalaaditud serdi.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Boomi** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli.
    4.  Kui **Domeeniliit Id asukoht**, valige **Federation Id on teema NameID elemendi**.
    5.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Boomi sisse logida, nad peavad olema ettevalmistatud Boomi.  
Puhul Boomi, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **Boomi** ettevõtte administraatorina.

2.  Minge **kasutajahalduse \> kasutajate**.

    ![Kasutajatele] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Kasutajatele")

3.  Klõpsake nuppu **Lisa kasutaja**.

    ![Kasutaja lisamine] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Kasutaja lisamine")

4.  Lehel **Lisa kasutaja rollid** dialoogiboksis tehke järgmist.

    ![Kasutaja lisamine] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Kasutaja lisamine")

    1.  Tippige **eesnimi**, **Perekonnanimi** ja **e-posti** kehtiv AAD konto soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu OK.

>[AZURE.NOTE] Saate kasutada mis tahes muud Boomi kasutaja konto loomise tööriistu või API-de osutavad Boomi kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Boomi kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Boomi **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
