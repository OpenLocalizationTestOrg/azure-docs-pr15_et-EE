<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine uue reliikvia | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada uut reliikvia Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Õpetus: Azure'i Active Directory integreerimine uue reliikvia
  
Selle õpetuse eesmärk on näidata, kuidas häälestada ühekordse sisselogimise Azure Active Directory ja uus reliikvia vahel.
  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Uue reliikvia ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, olete määranud uue reliikvia Azure Active Directory kasutajad saab ühekordse sisselogimise AAD Accessi juhtpaneeli kaudu.

1.  Rakenduste integreerimise jaoks uus reliikvia lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-new-relic"></a>Rakenduste integreerimise jaoks uus reliikvia lubamine
  
Selle jaotise eesmärk on liigendamine jaoks uus reliikvia rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Rakenduste integreerimise jaoks uus reliikvia lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Uut reliikvia**.

    ![Rakenduse Galerii] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Uus reliikvia**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Uue reliikvia] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Uue reliikvia")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selles jaotises kirjeldatakse, kuidas kasutajad saavad oma kontoga sisse Azure Active Directory federation SAML protokolli abil uus reliikvia autentimiseks.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **Uus reliikvia** rakenduse integreerimine Azure klassikaline portaalis.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida uue reliikvia** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** tekstiväljale **Uus reliikvia Logi sisse URL** tippige URL, mida kasutatakse teie kasutajatele uue reliikvia rakenduse logida ja seejärel klõpsake nuppu **edasi**. 

    Rakenduse URL on uus reliikvia rentniku URL-i (nt: *https://rpm.newrelic.com*):

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil uue reliikvia** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage fail serdi kohalikku arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas sisse logida saidile **Uue reliikvia** ettevõtte administraatorina.

6.  Klõpsake menüü ülaosas käsku **Konto sätted**.

    ![Konto sätted] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Konto sätted")

7.  Klõpsake vahekaarti **Turvalisus ja autentimine** , ja seejärel klõpsake vahekaarti **ühekordse sisselogimise** .

    ![Ühekordne sisselogimine] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Ühekordne sisselogimine")

8.  Klõpsake lehel SAML dialoogiboksi tehke järgmist:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Klõpsake oma allalaaditud serdi Azure Active Directory üleslaaditava **Faili valimine** .
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil uue reliikvia** kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Remote sisselogimise URL-i** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil uue reliikvia** kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL-i kuvataks välju** tekstiväli.
    4.  Klõpsake nuppu **Salvesta muudatused**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure Active Directory kasutajad uue reliikvia sisse logida, ta peab olema ette valmistatud uue reliikvia sisse.  
Uue reliikvia, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Uue reliikvia kasutajakonto sätete, tehke järgmist.

1.  Logige saidile **Uue reliikvia** ettevõtte administraatorina.

2.  Klõpsake menüü ülaosas käsku **Konto sätted**.

    ![Konto sätted] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Konto sätted")

3.  Vasakul paanil **konto** **Kokkuvõte**nuppu ja seejärel klõpsake nuppu **Lisa kasutaja**.

    ![Konto sätted] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Konto sätted")

4.  Klõpsake dialoogiboksis **aktiivsed kasutajad** teha järgmist:

    ![Aktiivsed kasutajad] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktiivsed kasutajad")

    1.  Tippige tekstiväljale **e-posti** meiliaadress lubatud Azure Active Directory kasutaja soovite ette valmistada.
    2.  Valige **rolliga** **kasutaja**.
    3.  Klõpsake nuppu **Lisa kasutaja**.

>[AZURE.NOTE]Saate kasutada mis tahes muud uue reliikvia kasutaja konto loomise tööriistade või API-de osutavad uue reliikvia kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Uue reliikvia kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Uus reliikvia** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).




