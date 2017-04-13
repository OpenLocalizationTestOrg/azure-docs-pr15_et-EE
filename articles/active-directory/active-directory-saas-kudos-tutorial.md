<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine maine | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory maine kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Õpetus: Azure'i Active Directory integreerimine maine
  
Selle õpetuse eesmärk on integreerimine Azure ja maine kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Maine rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud maine ühekordse sisselogimise rakendusse maine ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Maine jaoks rakenduse integreerimise lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-kudos"></a>Maine jaoks rakenduse integreerimise lubamine
  
Selle jaotise eesmärk on liigendamine maine jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Rakenduste integreerimise jaoks maine lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **maine**.

    ![Rakenduse Galerii] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **maine**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Maine] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Maine")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks maine oma kontoga Azure AD abil SAML protokolli federation lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **maine** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida maine** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Maine Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.kudosnow.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise maine juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil maine ettevõtte administraatorina.

6.  Menüü peal, klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Sätted")

7.  Klõpsake **integratsioon \> SSO**.

8.  Jaotises **SSO** tehke järgmist.

    ![SSO-d] (./media/active-directory-saas-kudos-tutorial/IC787807.png "SSO-d")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil maine** dialoogiboksi **Ühekordse sisselogimise teenuse URL-i** väärtus kopeerida ja seejärel kleepige **URL-i sisse logida** tekstiväli.
    2.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP]
        Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see siis tekstivälja **x.509 vastav sert**
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil maine** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **Välju URL-i** tekstiväli.
    5.  Tippige tekstiväljale **Oma maine URL-i** oma ettevõtte nime.
    6.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate maine sisse logida, nad peavad olema ettevalmistatud maine.  
Maine, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **maine** ettevõtte administraatorina.

2.  Menüü peal, klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Sätted")

3.  Valige **kasutaja administraator**.

4.  Klõpsake vahekaarti **Kasutajad** ja seejärel klõpsake nuppu **Lisa kasutaja**.

    ![Kasutaja administraator] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Kasutaja administraator")

5.  Tehke jaotises **Lisa kasutaja** , toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Kasutaja lisamine")

    1.  Tippige **eesnimi**, **Perekonnanimi**, **e-posti** ja muud üksikasjad lubatud Azure Active Directory konto soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Loo kasutaja**.

>[AZURE.NOTE]Saate kasutada mis tahes muud maine kasutaja konto loomise tööriistade või API-de osutavad maine kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Maine kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **maine **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).