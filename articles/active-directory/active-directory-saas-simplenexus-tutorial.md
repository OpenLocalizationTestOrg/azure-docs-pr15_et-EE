<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SimpleNexus | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory SimpleNexus abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Õpetus: Azure'i Active Directory integreerimine SimpleNexus
  
Selle õpetuse eesmärk on integreerimine Azure ja SimpleNexus kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   SimpleNexus ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud SimpleNexus ühekordse sisselogimise rakendusse SimpleNexus ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks SimpleNexus lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Rakenduste integreerimise jaoks SimpleNexus lubamine
  
Selle jaotise eesmärk on liigendamine SimpleNexus jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Rakenduste integreerimise jaoks SimpleNexus lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **lihtne seos**.

    ![Rakenduse Galerii] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **SimpleNexus**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Lihtne seos] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Lihtne seos")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks SimpleNexus oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **SimpleNexus** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate SimpleNexus logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **SimpleNexus sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://simplenexus.com/CompanyName\_login*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise SimpleNexus veebisaidil** klõpsake **metaandmete allalaadimine**ja seejärel edasi metaandmete tugimeeskonnale SimpleNexus faili.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Ühekordse sisselogimise konfigureerimine")

    >[AZURE.NOTE] Ühekordse sisselogimise peab olema lubatud SimpleNexus tugimeeskonnalt.

5.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate SimpleNexus sisse logida, nad peavad olema ettevalmistatud SimpleNexus.  
SimpleNexus, ettevalmistamise on käsitsi tööülesande läbi rentnikuadministraator.

>[AZURE.NOTE] Saate kasutada mis tahes muud SimpleNexus kasutaja konto loomise tööriistade või API-de esitatud SimpleNexus AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>SimpleNexus kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **SimpleNexus **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).