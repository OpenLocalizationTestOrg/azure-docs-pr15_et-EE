<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Wikispaces | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory Wikispaces kasutamine!." 
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

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Õpetus: Azure'i Active Directory integreerimine Wikispaces
  
Selle õpetuse eesmärk on integreerimine Azure ja Wikispaces kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Wikispaces ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Wikispaces ühekordse sisselogimise rakendusse Wikispaces ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Wikispaces lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Sceanrio] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##<a name="enabling-the-application-integration-for-wikispaces"></a>Rakenduste integreerimise jaoks Wikispaces lubamine
  
Selle jaotise eesmärk on liigendamine Wikispaces jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Wikispaces lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Wikispaces**.

    ![Rakenduse Galerii] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Wikispaces**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Wikispaces oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Wikispaces** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida Wikispaces** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Wikispaces Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.wikispaces.net*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise Wikispaces juures** **metaandmete allalaadimine**nuppu ja salvestage metaandmete faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Ühekordse sisselogimise konfigureerimine")

5.  Wikispaces tugimeeskonnale saata selle metadatafile.

    >[AZURE.NOTE] Ühekordse sisselogimise konfigureerimine on vaja teha Wikispaces tugimeeskonnalt. Saate teatise niipea, kui konfiguratsioon on lõpule viidud.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Wikispaces sisse logida, nad peab olema ettevalmistatud Wikispaces.  
Wikispaces, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **Wikispaces** ettevõtte administraatorina.

2.  Minge **liikmed**.

    ![Liikmed] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Liikmed")

3.  Klõpsake **käsku Kutsu inimesi**.

    ![Inimeste kutsumine] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Inimeste kutsumine")

4.  Jaotises **Kutsu inimesi** , tehke järgmist.

    ![Inimeste kutsumine] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Inimeste kutsumine")

    1.  Sisestage **kasutajanimed või e-posti aadressi** soovite seotud tekstiväljad säte kehtib AAD konto.
    2.  Klõpsake nuppu **saada**.  

        >[AZURE.NOTE] Azure Active Directory konto omanik saab e-posti, sh link konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud Wikispaces kasutaja konto loomise tööriistade või API-de osutavad Wikispaces kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Wikispaces kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Wikispaces **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).