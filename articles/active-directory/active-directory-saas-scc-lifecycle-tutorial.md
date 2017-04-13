<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SCC elutsükli | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory SCC elutsükli kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Õpetus: Azure'i Active Directory integreerimine SCC elutsükkel
  
Selle õpetuse eesmärk on integreerimine Azure ja SCC elutsükli kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   SCC elutsükli ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud SCC elutsükli ühekordse sisselogimise rakendusse SCC elutsükli ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks SCC elutsükli lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Rakenduste integreerimise jaoks SCC elutsükli lubamine
  
Selle jaotise eesmärk on liigendamine SCC elutsükli jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Rakenduste integreerimise jaoks SCC elutsükli lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **SCC elutsükli**.

    ![Rakenduse Galerii] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **SCC elutsükli**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![SCC elutsükkel] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC elutsükkel")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks SCC elutsükkel oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **SCC elutsükli** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida SCC elutsükli** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad SCC elutsükli rakenduse abil järgmise mustriga "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" logida ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise SCC elutsükli juures** **metaandmete allalaadimine**nuppu, ja seejärel salvestage metaandmete fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Ühekordse sisselogimise konfigureerimine")

5.  Metaandmete tugimeeskonnale SCC elutsükli faili edastamine.

    >[AZURE.NOTE]Ühekordse sisselogimise peab olema lubatud SCC elutsükli tugimeeskonnalt.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate SCC elutsükli sisse logida, nad peavad olema ettevalmistatud SCC elutsükli.
  
On teil konfigureerida kasutaja ettevalmistamise SCC elutsükkel toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab SCC elutsükli sisse logida, luuakse automaatselt konto SCC elutsükli vajaduse korral.

>[AZURE.NOTE]Saate kasutada mis tahes muud SCC elutsükli kasutaja konto loomise tööriistade või API-de osutavad SCC elutsükli kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>SCC elutsükli kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **SCC elutsükli **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).