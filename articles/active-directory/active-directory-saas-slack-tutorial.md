<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine vaikne | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory vaikne kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Õpetus: Azure'i Active Directory integreerimine vaikne
  
Selle õpetuse eesmärk on integreerimine Azure ja vaikne kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Vaikne ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud vaikne ühekordse sisselogimise taotluse saidile vaikne ettevõtte (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks vaikne lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-slack-tutorial/IC794980.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-slack"></a>Rakenduste integreerimise jaoks vaikne lubamine
  
Selle jaotise eesmärk on liigendamine jaoks vaikne rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Rakenduste integreerimise jaoks vaikne lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-slack-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-slack-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-slack-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **vaikne**.

    ![Rakenduse Galerii] (./media/active-directory-saas-slack-tutorial/IC794981.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **vaikne**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Stsenaarium] (./media/active-directory-saas-slack-tutorial/IC796925.png "Stsenaarium")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks vaikne oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **vaikne** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-slack-tutorial/IC794982.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas kas soovite kasutajad logida vaikne** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ning seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-slack-tutorial/IC794983.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Vaikne sisselogimise URL** väljale Tippige oma vaikne rentniku URL (nt: "*https://azuread.slack.com*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-slack-tutorial/IC794984.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise vaikne veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-slack-tutorial/IC794985.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil vaikne ettevõtte administraatorina.

6.  Minge **Microsoft Azure AD \> meeskonnatöö sätted**.

    ![Meeskonnatöö sätted] (./media/active-directory-saas-slack-tutorial/IC794986.png "Meeskonnatöö sätted")

7.  **Meeskonnatöö sätted** jaotises **autentimine** vahekaarti ning seejärel klõpsake käsku **Muuda sätteid**.

    ![Meeskonnatöö sätted] (./media/active-directory-saas-slack-tutorial/IC794987.png "Meeskonnatöö sätted")

8.  Klõpsake dialoogiboksis **SAML autentimissätted** tehke järgmist.

    ![SAML sätted] (./media/active-directory-saas-slack-tutorial/IC794988.png "SAML sätted")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil vaikne** dialoogiboksi kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **SAML 2.0 lõpp-punkti (HTTP)** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil vaikne** dialoogiboksi kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja väljaandja** tekstiväli.
    3.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.
    
        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **Avaliku serdi** tekstiväli.
    5.  Tühjendage ruut **Luba kasutajatel muuta oma meiliaadress**.
    6.  Valige **Luba kasutajatel valida oma kasutajanimi**.
    7.  **Autentimise teie meeskond peab kasutama**, valige **see pole kohustuslik**.
    8.  Klõpsake nuppu **Salvesta konfigureerimine**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-slack-tutorial/IC794989.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate vaikne sisse logida, nad peavad olema ettevalmistatud vaikne.
  
On teil konfigureerida kasutaja ettevalmistamine vaikne toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab vaikne sisse logida, luuakse automaatselt vaikne konto vajaduse korral.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Vaikne kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **vaikne **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-slack-tutorial/IC794990.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-slack-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).