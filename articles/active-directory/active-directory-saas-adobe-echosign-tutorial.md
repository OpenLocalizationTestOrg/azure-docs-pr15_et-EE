<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Adobe EchoSign | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Adobe EchoSign Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Õpetus: Azure'i Active Directory integreerimine Adobe EchoSign

Selle õpetuse eesmärk on integreerimine Azure ja Adobe EchoSign kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Adobe EchoSign, mis on ühe logige lubatud tellimuse

Pärast selle õpetuse, olete määranud Adobe EchoSign Azure AD kasutajate saab ühe rakenduse Adobe EchoSign ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu sisse logida.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Adobe EchoSign lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Rakenduste integreerimise jaoks Adobe EchoSign lubamine

Selle jaotise eesmärk on liigendamine Adobe EchoSign jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Adobe EchoSign lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsing**, **Adobe EchoSign**.

    ![Rakenduse Galerii] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Adobe EchoSign**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Adobe EchoSign oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Adobe EchoSign** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Adobe EchoSign logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Adobe EchoSign Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.echosign.com/*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Adobe EchoSign juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse Adobe EchoSign ettevõtte saidi administraator.

6.  Menüü peal, klõpsake nuppu **konto**ja surra Vasakpoolsel navigeerimispaanil, valige **SAML sätted** jaotises **Konto sätted**.

    ![Konto] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Konto")

7.  SAML sätted jaotises tehke järgmist.

    ![SAML sätted] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML sätted")

    1.  **SAML režiim**, valige **SAML kohustuslik**.
    2.  Valige **Luba EchoSign konto administraatorid volituste EchoSign sisse logima**.
    3.  **Kasutaja loomine**, valige **Automaatne lisamine kaudu SAML autenditud kasutajad**.

8.  Teisaldada, toimige järgmiselt.

    ![SAML sätted] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML sätted")

    1.  Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Adobe EchoSign veebisaidil** lehel **Üksuse ID** väärtus kopeerida ja seejärel kleepige **IdP üksuse ID** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Adobe EchoSign** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ning kleepige need **IdP sisselogimise URL-i** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Adobe EchoSign** dialoogiboksi **Remote välju URL-i** väärtus kopeerida ja seejärel kleepige **URL-i IdP välju** tekstiväli.
    4.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **IdP serdi** tekstivälja 6.  Klõpsake nuppu **Salvesta muudatused**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Adobe EchoSign sisse logida, nad peab olema ettevalmistatud Adobe EchoSign.  
Adobe EchoSign, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **Adobe EchoSign** ettevõtte administraatorina.

2.  Menüü peal, klõpsake valikut **konto**ja surra Vasakpoolsel navigeerimispaanil, valige **kasutajad ja rühmad**, ja seejärel klõpsake nuppu **Loo uus kasutaja**.

    ![Konto] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Konto")

3.  Jaotises **Uue kasutaja loomiseks** tehke järgmist:

    ![Kasutaja loomine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Kasutaja loomine")

    1.  Tippige **Meiliaadress**, **eesnimi** ja **Perekonnanimi** kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Loo kasutaja**.

        >[AZURE.NOTE] Azure Active Directory konto omanik saavad meili, mis sisaldab linki konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud Adobe EchoSign kasutaja konto loomise tööriistade või API-de osutavad Adobe EchoSign AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Adobe EchoSign kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Adobe EchoSign **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
