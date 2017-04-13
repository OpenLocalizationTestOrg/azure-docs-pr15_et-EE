<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Clarizen | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Clarizen abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Õpetus: Azure'i Active Directory integreerimine Clarizen

Selle õpetuse eesmärk on integreerimine Azure ja Clarizen kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Clarizen ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Clarizen ühekordse sisselogimise rakendusse Clarizen ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Clarizen lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-clarizen"></a>Rakenduste integreerimise jaoks Clarizen lubamine

Selle jaotise eesmärk on liigendamine Clarizen jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Clarizen lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Clarizen**.

    ![Rakenduse Galerii] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Clarizen**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Clarizen oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Clarizen** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Clarizen** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Konfigureeri ühekordse sisselogimise Clarizen veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Ühekordse sisselogimise konfigureerimine")

4.  Muu veebibrauseri akna, logige saidile **Clarizen** ettevõtte administraator (nt: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Klõpsake oma kasutajanimi ja seejärel klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Sätted")

6.  Klõpsake vahekaarti **Globaalsätete** ja seejärel klõpsake **Ühendatud autentimist**kõrval nuppu **Redigeeri**.

    ![Globaalsätete] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globaalsätete")

7.  Klõpsake dialoogiboksis **Ühendatud autentimise** tehke järgmist.

    ![Ühendatud autentimine] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Ühendatud autentimine")

    1.  Klõpsake **üles** laadida oma allalaaditud serdi.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Clarizen** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige see **sisselogimise URL-i** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **konfigureerimine ühe väljunud veebisaidil Clarizen** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **Sign-out URL** tekstiväli.
    4.  Valige suvand **Kasuta postitus**.
    5.  Klõpsake nuppu **Salvesta**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Clarizen sisse logida, nad peavad olema ettevalmistatud Clarizen.  
Puhul Clarizen, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Clarizen** ettevõtte saidi administraator.

2.  Klõpsake valikut **inimesed**.

    ![Inimesed] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Inimesed")

3.  Klõpsake nuppu **Kutsu kasutaja**.

    ![Kasutajate kutsumine] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Kasutajate kutsumine")

4.  Klõpsake lehel Kutsu inimesi dialoogiboksi tehke järgmist.

    ![Inimeste kutsumine] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Inimeste kutsumine")

    1.  Tippige tekstiväljale **e-posti** e-posti aadressi kehtiv Azure Active Directory konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Kutsu**.

    >[AZURE.NOTE] Azure Active Directory konto omanik saadetakse teile e-posti ja järgige oma konto kinnitada, kuni see muutub aktiivne link.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Clarizen kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Clarizen **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
