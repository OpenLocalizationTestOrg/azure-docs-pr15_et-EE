<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Kintone | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Kintone abil!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Õpetus: Azure'i Active Directory integreerimine Kintone
  
Selle õpetuse eesmärk on integreerimine Azure ja Kintone kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Kintone ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Kintone ühekordse sisselogimise rakendusse Kintone ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Kintone lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-kintone"></a>Rakenduste integreerimise jaoks Kintone lubamine
  
Selle jaotise eesmärk on liigendamine Kintone jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Kintone lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Kintone**.

    ![Rakenduse Galerii] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Kintone**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Kintone oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Kintone** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Kintone logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Kintone Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.kintone.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil Kintone** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil **Kintone** ettevõtte administraatorina.

6.  Klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Sätted")

7.  Klõpsake nuppu **kasutajad ja süsteemi haldamine**.

    ![Kasutajad ja süsteemi haldamine] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Kasutajad ja süsteemi haldamine")

8.  Klõpsake jaotises **süsteemi halduse \> turvalisus** klõpsake nuppu **Logi sisse**.

    ![Sisselogimine] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Sisselogimine")

9.  Klõpsake valikut **Luba SAML autentimine**.

    ![SAML autentimine] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML autentimine")

10. SAML autentimine jaotises tehke järgmist.

    ![SAML autentimine] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML autentimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Kintone** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Kintone** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    3.  Klõpsake nuppu **Sirvi** oma allalaaditud serdi üleslaadimine.
    4.  Klõpsake nuppu **Salvesta**.

11. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Kintone sisse logida, nad peavad olema ettevalmistatud Kintone.  
Puhul Kintone, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Kintone** ettevõtte saidi administraator.

2.  Klõpsake **sätet**.

    ![Sätted] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Sätted")

3.  Klõpsake nuppu **kasutajad ja süsteemi haldamine**.

    ![Kasutaja ja süsteemi haldamine] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Kasutaja ja süsteemi haldamine")

4.  Klõpsake jaotises **Kasutaja halduse** **osakondade ja kasutajad**.

    ![Osakonna ja kasutajad] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Osakonna ja kasutajad")

5.  Klõpsake nuppu **Uus kasutaja**.

    ![Uued kasutajad] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Uued kasutajad")

6.  Jaotises **Uus kasutaja** , tehke järgmist.

    ![Uued kasutajad] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Uued kasutajad")

    1.  Tippige **Kuvatav nimi**, **Kasutajanimi**, **Uus parool**, **Kinnitage parool**, **Meiliaadress** ja muud üksikasjad kehtiv AAD konto, mida soovite seotud texboxes säte.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Kintone kasutaja konto loomise tööriistade või API-de esitatud Kintone AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Kintone kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Kintone **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).