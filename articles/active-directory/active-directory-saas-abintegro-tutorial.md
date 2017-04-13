<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Abintegro | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Abintegro abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-abintegro"></a>Õpetus: Azure'i Active Directory integreerimine Abintegro

Selle õpetuse eesmärk on integreerimine Azure ja Abintegro kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Abintegro ühekordse sisselogimise lubatud tellimuse

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Abintegro ühekordse sisselogimise rakendusse Abintegro ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Abintegro lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-abintegro-tutorial/IC790076.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-abintegro"></a>Rakenduste integreerimise jaoks Abintegro lubamine

Selle jaotise eesmärk on liigendamine Abintegro jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-abintegro-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Abintegro lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-abintegro-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-abintegro-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-abintegro-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-abintegro-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **abintegro**.

    ![Rakenduse Galerii] (./media/active-directory-saas-abintegro-tutorial/IC790077.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Abintegro**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Abintegro] (./media/active-directory-saas-abintegro-tutorial/IC790078.png "Abintegro")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Abintegro oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Abintegro** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-abintegro-tutorial/IC790079.png "Ühe logima konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Abintegro logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-abintegro-tutorial/IC790080.png "Ühe logima konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Abintegro Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Abintegro (nt: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-abintegro-tutorial/IC790081.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise Abintegro juures** **metaandmete allalaadimine**nuppu ja salvestage metaandmete faili oma arvutist.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-abintegro-tutorial/IC790082.png "Ühe logima konfigureerimine")

5.  Abintegro tugimeeskonnale saata selle metadatafile.

    >[AZURE.NOTE] Ühekordse sisselogimise konfigureerimine on vaja teha Abintegro tugimeeskonnalt. Saate teatise niipea, kui konfiguratsioon on lõpule viidud.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-abintegro-tutorial/IC790083.png "Ühe logima konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

On teil konfigureerida ettevalmistamine Abintegro kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja proovib Abintegro Accessi paneeli abil sisse logida, Abintegro kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud Abintegro.
##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-abintegro-perform-the-following-steps"></a>Abintegro kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Abintegro **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-abintegro-tutorial/IC790084.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-abintegro-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
