<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Lucidchart | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Lucidchart abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Õpetus: Azure'i Active Directory integreerimine Lucidchart
  
Selle õpetuse eesmärk on integreerimine Azure ja Lucidchart kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Lucidchart ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Lucidchart ühekordse sisselogimise rakendusse Lucidchart ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Lucidchart lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Rakenduste integreerimise jaoks Lucidchart lubamine
  
Selle jaotise eesmärk on liigendamine Lucidchart jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Lucidchart lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Lucidchart**.

    ![Rakenduse Galerii] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Lucidchart**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Lucidchart oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Lucidchart** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Lucidchart logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Lucidchart Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Lucidchart rakenduse (nt: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Lucidchart veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete alla laadida**, ja salvestage andmefaili kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Lucidchart ettevõtte administraatorina.

6.  Klõpsake menüü peal, **meeskonnatöö**.

    ![Meeskonnatöö] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Meeskonnatöö")

7.  Klõpsake **rakenduse \> haldamine SAML**.

    ![SAML haldamine] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "SAML haldamine")

8.  Klõpsake lehel **SAML autentimissätted** dialoogiboksi tehke järgmist:

    1.  Valige **Luba SAML autentimine**ja seejärel klõpsake nuppu **Valikuline**.
        ![SAML autentimissätted] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML autentimissätted")
    2.  **Domeeni** tekstivälja, tippige oma domeeni ja seejärel nuppu **Muuda sert**.
        ![Serdi muutmine] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Serdi muutmine")
    3.  Metaandmete allalaaditud faili avamiseks, kopeerige sisu ja seejärel kleepige see **Üles metaandmete** tekstiväli.
        ![Metaandmete üleslaadimine] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Metaandmete üleslaadimine")
    4.  Valige **meeskond automaatselt Lisa uus kasutaja**ja klõpsake nuppu **Salvesta muudatused**.
        ![Muudatuste salvestamine] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Muudatuste salvestamine")

9.  Valige kinnituse ühekordse sisselogimise konfigureerimine, ja klõpsake nuppu **täielik** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida ettevalmistamine Lucidchart kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja proovib Lucidchart Accessi paneeli abil sisse logida, Lucidchart kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud Lucidchart.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Lucidchart kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Lucidchart **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).