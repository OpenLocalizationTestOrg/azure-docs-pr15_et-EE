<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine TeamSeer | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory TeamSeer abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Õpetus: Azure'i Active Directory integreerimine TeamSeer
  
Selle õpetuse eesmärk on integreerimine Azure ja TeamSeer kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   TeamSeer rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud TeamSeer ühekordse sisselogimise rakendusse TeamSeer ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks TeamSeer lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-teamseer"></a>Rakenduste integreerimise jaoks TeamSeer lubamine
  
Selle jaotise eesmärk on liigendamine TeamSeer jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Rakenduste integreerimise jaoks TeamSeer lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **TeamSeer**.

    ![Rakenduse Galerii] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **TeamSeer**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks TeamSeer oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **TeamSeer** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate TeamSeer logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **TeamSeer sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://www.teamseer.com/companyid*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise TeamSeer veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil TeamSeer ettevõtte administraatorina.

6.  Valige **HR administraator**.

    ![HR administraator] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR administraator")

7.  Klõpsake nuppu **häälestus**.

    ![Häälestamine] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Häälestamine")

8.  Klõpsake **häälestamine SAML pakkuja üksikasjad**.

    ![SAML sätted] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML sätted")

9.  SAML pakkuja üksikasjad jaotises tehke järgmist.

    ![SAML sätted] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML sätted")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil TeamSeer** dialoogiboksi **Ühekordse sisselogimise teenuse URL-i** väärtus kopeerida ja seejärel kleepige **URL-i** tekstiväli.
    2.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see siis **IdP avaliku serdi** tekstiväli.

10. SAML pakkuja konfigureerimise lõpuleviimiseks tehke järgmist.

    ![SAML sätted] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML sätted")

    1.  **Testige e-posti aadressid**, tippige testkasutaja meiliaadress.
    2.  Tippige tekstiväljale **väljaandja** väljaandja URL-i teenusepakkuja.
    3.  Klõpsake nuppu **Salvesta**.

11. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate TeamSeer sisse logida, nad peavad olema ettevalmistatud ShiftPlanning.  
Puhul TeamSeer, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **TeamSeer** ettevõtte saidi administraator.

2.  Tehke järgmist.

    ![HR administraator] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR administraator")

    1.  Minge **HR administraator \> kasutajate**.
    2.  Klõpsake nuppu **Uus kasutaja viisardit**.

3.  **Kasutaja üksikasjad** jaotises tehke järgmist.

    ![Kasutaja üksikasjad] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Kasutaja üksikasjad")

    1.  Tippige **eesnimi**, **perekonnanimi**, kehtiv AAD konto, mida soovite säte seotud tekstiväljad **kasutajanimi (e-posti aadress)** .
    2.  Klõpsake nuppu **edasi**.

4.  Järgige sees Kuva uus kasutaja lisamise ja klõpsake nuppu **valmis**.

>[AZURE.NOTE] Saate kasutada mis tahes muud TeamSeer kasutaja konto loomise tööriistade või API-de esitatud TeamSeer ettevalmistamise Azure AD Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>TeamSeer kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **TeamSeer **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).