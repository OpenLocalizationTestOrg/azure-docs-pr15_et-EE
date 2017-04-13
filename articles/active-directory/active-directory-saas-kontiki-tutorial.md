<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Kontiki | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Kontiki kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Õpetus: Azure'i Active Directory integreerimine Kontiki
  
Selle õpetuse eesmärk on integreerimine Azure ja Kontiki kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Kontiki ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Kontiki ühekordse sisselogimise rakendusse Kontiki ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Kontiki lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-kontiki"></a>Rakenduste integreerimise jaoks Kontiki lubamine
  
Selle jaotise eesmärk on liigendamine Kontiki jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Kontiki lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Kontiki**.

    ![Rakenduse Galerii] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Kontiki**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Kontiki oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Kontiki** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Ühe logima konfigureerimine")

2.  **Kuidas soovite kasutajad logida Kontiki** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Ühe logima konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Kontiki Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Kontiki (nt: "*https://company.mc.eval.kontiki.com/*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise Kontiki juures** **metaandmete allalaadimine**nuppu ja salvestage metaandmete faili oma arvutist.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Ühe logima konfigureerimine")

5.  Kontiki tugimeeskonnale saata selle metadatafile.

    >[AZURE.NOTE] Ühekordse sisselogimise konfigureerimine on vaja teha Kontiki tugimeeskonnalt. Saate teatise niipea, kui konfiguratsioon on lõpule viidud.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Ühe logima konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida ettevalmistamine Kontiki kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab Kontiki Accessi paneeli abil sisse logida, Kontiki kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud Kontiki.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Kontiki kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Kontiki **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).