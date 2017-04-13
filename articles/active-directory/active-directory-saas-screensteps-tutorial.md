<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ScreenSteps | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory ScreenSteps abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Õpetus: Azure'i Active Directory integreerimine ScreenSteps
  
Selle õpetuse eesmärk on integreerimine Azure ja ScreenSteps kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   ScreenSteps rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud ScreenSteps ühekordse sisselogimise rakendusse ScreenSteps ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks ScreenSteps lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-screensteps"></a>Rakenduste integreerimise jaoks ScreenSteps lubamine
  
Selle jaotise eesmärk on liigendamine ScreenSteps jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Rakenduste integreerimise jaoks ScreenSteps lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ScreenSteps**.

    ![Rakenduse Galerii] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ScreenSteps**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ScreenSteps oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **ScreenSteps** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate ScreenSteps logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ScreenSteps sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. ScreenSteps.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise ScreenSteps veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil ScreenSteps ettevõtte administraatorina.

6.  Valige **konto haldamine**.

    ![Konto haldamine] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Konto haldamine")

7.  Klõpsake valikut **Remote autentimine**.

    ![Serveri autentimine] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Serveri autentimine")

8.  Klõpsake nuppu **Loo autentimise lõpp-punkti**.

    ![Serveri autentimine] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Serveri autentimine")

9.  Jaotises **autentimine lõpp-punkti loomine** tehke järgmist.

    ![Loo lõpp autentimine] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Loo lõpp autentimine")

    1.  Tippige tekstiväljale **tiitel** tiitel.
    2.  Valige loendist **režiimi** **SAML**.
    3.  Klõpsake nuppu **Loo**.

10. Jaotises **Serveri autentimine lõpp-punkti** tehke järgmist.

    ![Serveri autentimine lõpp-punkti] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Serveri autentimine lõpp-punkti")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ScreenSteps** kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Remote sisselogimise URL-i** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ScreenSteps** **Remote välju URL-i** väärtus kopeerida ja seejärel kleepige **URL-i välja logida** tekstiväli.
    3.  Klõpsake käsku **Vali fail**ja laadige alla serdi.
    4.  Klõpsake nuppu **Värskenda**.

11. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate **ScreenSteps**sisse logida, nad peavad olema ettevalmistatud **ScreenSteps**.  
Puhul **ScreenSteps**, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Ettevalmistamise ScreenSteps kasutajakonto, tehke järgmist.

1.  Logige sisse oma **ScreenSteps** rentniku.

2.  Valige **konto haldamine**.

    ![Konto haldamine] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Konto haldamine")

3.  Klõpsake nuppu **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Kasutajatele")

4.  Klõpsake nuppu **Loo kasutaja**.

    ![Kõik kasutajad] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Kõik kasutajad")

5.  Valige loendist **Kasutajaroll** oma kasutaja rollid.

6.  Kasutajaroll, tippige jaotises**"eesnimi**, **perekonnanimi**, **e-posti**, **Login**, **parool** ja **Parooli kinnitus"**kehtiv AAD konto soovite seotud tekstiväljad säte.

    ![Uus kasutaja] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Uus kasutaja")

7.  Jaotises rühmad valige **Autentimise rühma (SAML)**ja seejärel klõpsake nuppu **Loo kasutaja**.

    ![Rühmad] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Rühmad")

>[AZURE.NOTE]Saate kasutada mis tahes muud ScreenSteps kasutaja konto loomise tööriistade või API-de esitatud ScreenSteps AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>ScreenSteps kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **ScreenSteps **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Kasutajate määramine] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Kasutajate määramine")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).