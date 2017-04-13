<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Picturepark | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Picturepark abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Õpetus: Azure'i Active Directory integreerimine Picturepark
  
Selle õpetuse eesmärk on integreerimine Azure ja Picturepark kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Picturepark rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Picturepark ühekordse sisselogimise rakendusse Picturepark ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Picturepark lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-picturepark"></a>Rakenduste integreerimise jaoks Picturepark lubamine
  
Selle jaotise eesmärk on liigendamine Picturepark jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Picturepark lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Picturepark**.

    ![Rakenduse Galerii] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Picturepark**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Picturepark oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Picturepark konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Picturepark** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Picturepark logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Picturepark Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.picturepark.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Picturepark veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Picturepark ettevõtte administraatorina.

6.  Klõpsake tööriistaribal nuppu **Haldustööriistad**ja klõpsake **Halduskonsooli**.

    ![Halduskonsooli] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Halduskonsooli")

7.  Klõpsake **autentimist**ja seejärel klõpsake nuppu **identiteedipakkujad**.

    ![Autentimine] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Autentimine")

8.  **Identiteedi pakkuja konfigureerimine** jaotises tehke järgmist.

    ![Identiteedi pakkuja konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identiteedi pakkuja konfigureerimine")

    1.  Klõpsake nuppu **Lisa**.
    2.  Tippige oma konfiguratsioon.
    3.  Valige **Määra vaikesätteks**.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Picturepark** dialoogiboksi kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **Väljaandja URI** tekstiväli.
    5.  Eksporditud serdi **sõrmejälje** väärtus kopeerida, ja seejärel kleepige see **Usaldusväärne väljaandja pöial Prindi** tekstiväli.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    6.  Klõpsake **JoinDefaultUsersGroup**.
    7.  Atribuut **Emailaddress** tekstiväljale **taotluste** määramiseks tippige **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Konfigureerimine")
    8.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Picturepark sisse logida, nad peavad olema ettevalmistatud Picturepark.  
Puhul Picturepark, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Picturepark** rentniku.

2.  Ülaosas tööriistaribal klõpsake **Haldustööriistad**ja klõpsake käsku **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Kasutajatele")

3.  Vahekaart **Ülevaade kasutajad** , klõpsake nuppu **Uus**.

    ![Kasutajate haldamine] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Kasutajate haldamine")

4.  Klõpsake dialoogiboksis **Kasutaja loomiseks** tehke järgmist.

    ![Kasutaja loomine] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Kasutaja loomine")

    1.  Tippige soovitud: **e-posti aadress**, **parool**, **kinnitage parool**, **eesnimi**, **perekonnanimi**, **ettevõtte**, **riigi**, **ZIP**, **linna** kehtiv Azure Active Directory kasutaja soovite seotud tekstiväljad ot säte.
    2.  Valige soovitud **keel**.
    3.  Klõpsake nuppu **Loo**.

>[AZURE.NOTE]Saate kasutada mis tahes muud Picturepark kasutaja konto loomise tööriistade või API-de esitatud Picturepark AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Picturepark kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Picturepark **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).