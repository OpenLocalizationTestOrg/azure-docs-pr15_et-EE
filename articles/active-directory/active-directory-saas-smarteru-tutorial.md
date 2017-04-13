<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SmarterU | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory SmarterU abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Õpetus: Azure'i Active Directory integreerimine SmarterU
  
Selle õpetuse eesmärk on integreerimine Azure ja SmarterU kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   SmarterU rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud SmarterU ühekordse sisselogimise rakendusse SmarterU ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks SmarterU lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-smarteru"></a>Rakenduste integreerimise jaoks SmarterU lubamine
  
Selle jaotise eesmärk on liigendamine SmarterU jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Rakenduste integreerimise jaoks SmarterU lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **SmarterU**.

    ![Rakenduse fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Rakenduse fallery")

7.  Tulemuste paanil valige **SmarterU**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks SmarterU oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **SmarterU** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate SmarterU logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Ühekordse sisselogimise konfigureerimine")

3.  Klõpsake soovitud **konfigureerimine ühekordse sisselogimise veebisaidil SmarterU** lehe allalaadimiseks oma metaandmete **metaandmete alla laadida**ja seejärel andmed nime esitusviis kohalikult **c:\\SmarterUMetaData.cer**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Ühekordse sisselogimise konfigureerimine")

4.  Erinevate web brauseriaknas, logige sisse saidil SmarterU ettevõtte administraatorina.

5.  Klõpsake tööriistaribal käsku **Konto sätted**.

    ![Konto sätted] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Konto sätted")

6.  Lehel konto konfigureerimine tehke järgmist.

    ![Välise autoriseerimine] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Välise autoriseerimine")

    1.  Valige **Luba välise autoriseerimine**.
    2.  Valige jaotises **Juhtslaidi Login juhtelemendi** **SmarterU** vahekaart.
    3.  Valige jaotises **Kasutaja vaikimisi sisselogimine** **SmarterU** vahekaart.
    4.  Valige **Luba Okta**.
    5.  Metaandmete allalaaditud faili sisu ja seejärel kleepige **Okta metaandmete** tekstiväli.
    6.  Klõpsake nuppu **Salvesta**.

7.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate SmarterU sisse logida, nad peavad olema ettevalmistatud SmarterU.  
Puhul SmarterU, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **SmarterU** rentniku.

2.  Avage **Kasutajad**.

3.  Valige jaotises kasutaja tehke järgmist.

    ![Uus kasutaja] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Uus kasutaja")

    1.  Klõpsake nuppu **+ kasutaja**.
    2.  Tippige seotud atribuudiväärtused Azure AD kasutajakonto järgmised tekstiväljad: **esmane e-posti**, **Töötaja ID**, **parool**, **kinnitage parool**, **eesnimi**, **perekonnanimi**.
    3.  Klõpsake nuppu **aktiivne**.
    4.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud SmarterU kasutaja konto loomise tööriistade või API-de esitatud SmarterU AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>SmarterU kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **SmarterU **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).