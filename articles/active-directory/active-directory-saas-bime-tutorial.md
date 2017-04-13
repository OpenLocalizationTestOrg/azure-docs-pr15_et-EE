<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Bime | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Bime abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Õpetus: Azure'i Active Directory integreerimine Bime

Selle õpetuse eesmärk on integreerimine Azure ja Bime kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Bime rentniku jaoks

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Bime ühekordse sisselogimise rakendusse Bime ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Bime lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-bime-tutorial/IC775552.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-bime"></a>Rakenduste integreerimise jaoks Bime lubamine

Selle jaotise eesmärk on liigendamine Bime jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Bime lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-bime-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-bime-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-bime-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Bime**.

    ![Rakenduse Galerii] (./media/active-directory-saas-bime-tutorial/IC775553.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Bime**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Bime oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Bime konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Bime** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bime-tutorial/IC771709.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Bime logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bime-tutorial/IC775555.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Bime sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Bimeapp.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-bime-tutorial/IC775556.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Bime veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\Bime.cer**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bime-tutorial/IC775557.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Bime ettevõtte administraatorina.

6.  Klõpsake tööriistaribal **administraator**ja seejärel **kontot**.

    ![Administraator] (./media/active-directory-saas-bime-tutorial/IC775558.png "Administraator")

7.  Lehel konto konfigureerimine tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bime-tutorial/IC775559.png "Ühekordse sisselogimise konfigureerimine")

    1.  Valige **Luba SAML autentimine**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Bime** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Remote sisselogimise URL-i** tekstiväli.
    3.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Serdi sõrmejälje** tekstiväli.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    4.  Klõpsake nuppu **Salvesta**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bime-tutorial/IC775560.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Bime sisse logida, ta peab olema ettevalmistatud Bime.  
Puhul Bime, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Bime** rentniku.

2.  Klõpsake tööriistaribal **administraator**ja seejärel **Kasutajad**.

    ![Administraator] (./media/active-directory-saas-bime-tutorial/IC775561.png "Administraator")

3.  **Kasutajate loend**, klõpsake käsku **Lisa uus kasutaja** ("+").

    ![Kasutajatele] (./media/active-directory-saas-bime-tutorial/IC775562.png "Kasutajatele")

4.  Dialoogiboksi **Kasutaja üksikasjade** lehel tehke järgmist.

    ![Kasutaja üksikasjad] (./media/active-directory-saas-bime-tutorial/IC775563.png "Kasutaja üksikasjad")

    1.  Sisestage eesnimi, perekonnanimi, Login, e-posti kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu Salvesta.

>[AZURE.NOTE] Saate kasutada mis tahes muud Bime kasutaja konto loomise tööriistu või API-de esitatud Bime AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Bime kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Bime **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-bime-tutorial/IC775564.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-bime-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
