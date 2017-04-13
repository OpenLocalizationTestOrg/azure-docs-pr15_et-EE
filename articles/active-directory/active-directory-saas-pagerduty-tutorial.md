<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Pagerduty | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Pagerduty abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Õpetus: Azure'i Active Directory integreerimine Pagerduty
  
Selle õpetuse eesmärk on integreerimine Azure ja Pagerduty kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Pagerduty rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Pagerduty ühekordse sisselogimise rakendusse Pagerduty ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Pagerduty lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Rakenduste integreerimise jaoks Pagerduty lubamine
  
Selle jaotise eesmärk on liigendamine Pagerduty jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Pagerduty lubamiseks tehke järgmist.

1.  Klõpsake vasakpoolsel navigeerimispaanil Azure'i haldusportaal **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Pagerduty**.

    ![Rakenduse Galerii] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Pagerduty**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Pagerduty oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Pagerduty** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Pagerduty logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Pagerduty sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Pagerduty.com*", ja seejärel klõpsake nuppu **edasi**.

    ![URL-i konfigureerimine rakendus] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "URL-i konfigureerimine rakendus")

4.  Lehel **Konfigureeri ühekordse sisselogimise Pagerduty juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Pagerduty ettevõtte administraatorina.

6.  Klõpsake menüü ülaosas käsku **Konto sätted**.

    ![Konto sätted] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Konto sätted")

7.  Klõpsake **ühekordse sisselogimise**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Ühekordne sisselogimine")

8.  Klõpsake lehel lubada ühekordse sisselogimise (SSO) tehke järgmist.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Ühekordse sisselogimise lubamiseks")

    1.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    2.  Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **x.509 vastav sert** tekstiväli
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Pagerduty** dialoogi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Pagerduty** dialoogi, kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    5.  Valige **ühekordse sisselogimise sisselülitamine**.
    6.  Klõpsake nuppu **Salvesta muudatused**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Pagerduty sisse logida, nad peavad olema ettevalmistatud Pagerduty.  
Puhul Pagerduty, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Pagerduty** rentniku.

2.  Klõpsake menüü peal, **Kasutajad**.

3.  Klõpsake **kasutaja lisamine**.

    ![Kasutajate lisamine] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Kasutajate lisamine")

4.  Tippige dialoogiboksi **kutsuda oma meeskonna** **esimese ja viimase nimi** ja **meiliaadress, kelle soovite ettevalmistamise, klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Saada kutsub**Azure AD kasutaja** .

    ![Kutsuda oma meeskonna] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Kutsuda oma meeskonna")

    >[AZURE.NOTE] Kõik lisatud kasutajad saavad kutse PagerDuty konto loomiseks.

>[AZURE.NOTE] Saate kasutada mis tahes muud Pagerduty kasutaja konto loomise tööriistade või API-de esitatud Pagerduty AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Pagerduty kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Pagerduty **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).