<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Sciforma | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Sciforma abil!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-ad-integration-with-sciforma"></a>Õpetus: Azure'i AD Sciforma integreerimine
  
Selle õpetuse eesmärk on integreerimine Azure ja Sciforma kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Sciforma rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Sciforma ühekordse sisselogimise rakendusse Sciforma ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Sciforma lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-sciforma-tutorial/IC777369.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-sciforma"></a>Rakenduste integreerimise jaoks Sciforma lubamine
  
Selle jaotise eesmärk on liigendamine Sciforma jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-sciforma-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Sciforma lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sciforma-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-sciforma-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-sciforma-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-sciforma-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Sciforma**.

    ![Rakenduse Galerii] (./media/active-directory-saas-sciforma-tutorial/IC777370.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Sciforma**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Sciforma] (./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Sciforma oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Sciforma** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sciforma-tutorial/IC777372.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Sciforma logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sciforma-tutorial/IC777373.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Sciforma sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Sciforma.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-sciforma-tutorial/IC777374.png "Rakenduse URL-i konfigureerimine")

4.  Klõpsake soovitud **konfigureerimine ühekordse sisselogimise veebisaidil Sciforma** lehe allalaadimiseks oma metaandmete **metaandmete alla laadida**ja seejärel andmed nime esitusviis kohalikult **c:\\SciformaMetaData.xml**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sciforma-tutorial/IC777375.png "Konfigureerimine Ühekordne sisselogimine")

5.  Edasi Sciforma faili metaandmete tugimeeskonnale. Ühekordse sisselogimise jaoks konfigureerib toe meeskond.

6.  Valige kinnituse ühekordse sisselogimise konfigureerimine, ja klõpsake nuppu **täielik** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sciforma-tutorial/IC777376.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida ettevalmistamine Sciforma kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja proovib Sciforma Accessi paneeli abil sisse logida, Sciforma kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud Sciforma.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-sciforma-perform-the-following-steps"></a>Sciforma kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Sciforma **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-sciforma-tutorial/IC777377.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-sciforma-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).