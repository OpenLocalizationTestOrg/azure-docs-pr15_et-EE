<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine e-Builder | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada e-Builder Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Õpetus: E-Koosturi Azure Active Directory integreerimine
  
Selle õpetuse eesmärk on integreerimine Azure ja e-Builder kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   E-Builder rentnikku
  
Pärast selle õpetuse, saab Azure AD kasutajate e-Builder määratud ühekordse sisselogimise rakendusse e-Builder ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks e-Builder lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-e-builder"></a>Rakenduste integreerimise jaoks e-Builder lubamine
  
Selle jaotise eesmärk on liigendamine jaoks e-Builder rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Rakenduste integreerimise jaoks e-Builder lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsing**, **E-Koostur**.

    ![Rakenduse Galerii] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Rakenduse Galerii")

7.  Tulemuste paanil **E-Koosturi**valimine ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![E-Koosturi] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "E-Koosturi")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate e-Builder oma konto abil SAML protokolli federation Azure AD autentimiseks lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **E-Koosturi** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate e-Builder logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** tekstiväljale **URL-i sisse logida e-Builder** tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>.e-Builder.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Rakenduse URL-i konfigureerimine")

4.  Klõpsake soovitud **konfigureerimine ühekordse sisselogimise veebisaidil e-Koosturi** lehe allalaadimiseks oma metaandmete **metaandmete alla laadida**ja seejärel andmed nime esitusviis kohalikult **c:\\e-BuilderMetaData.xml**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Konfigureerimine Ühekordne sisselogimine")

5.  Edasi e-Builder faili metaandmete tugimeeskonnale. Ühekordse sisselogimise jaoks konfigureerib toe meeskond.

6.  Valige kinnituse ühekordse sisselogimise konfigureerimine, ja klõpsake nuppu **täielik** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida ettevalmistamine e-Builder kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab e-Builder Accessi paneeli abil sisse logida, e-Builder kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud e-Koostur.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Kasutajate e-Builder määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **e-Builder **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
