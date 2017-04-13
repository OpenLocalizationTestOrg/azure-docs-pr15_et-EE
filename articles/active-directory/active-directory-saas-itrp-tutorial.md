<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ITRP | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory ITRP abil!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Õpetus: Azure'i Active Directory integreerimine ITRP
  
Selle õpetuse eesmärk on integreerimine Azure ja ITRP kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   ITRP rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud ITRP ühekordse sisselogimise rakendusse ITRP ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks ITRP lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-itrp"></a>Rakenduste integreerimise jaoks ITRP lubamine
  
Selle jaotise eesmärk on liigendamine ITRP jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Rakenduste integreerimise jaoks ITRP lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ITRP**.

    ![Rakenduse Galerii] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ITRP**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ITRP oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks ITRP konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **ITRP** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajad logida ITRP** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ITRP sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. ITRP.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise ITRP veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\ITRP.cer**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil ITRP ettevõtte administraatorina.

6.  Klõpsake tööriistaribal nuppu **sätted**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Valige vasakpoolsel navigeerimispaanil **Ühekordse sisselogimise**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Ühekordne sisselogimine")

8.  Ühekordse sisselogimise konfigureerimine jaotises tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Ühekordne sisselogimine")

    ![Ühekordne sisselogimine] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Ühekordne sisselogimine")

    1.  Klõpsake nuppu **Luba**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ITRP** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **Remote välju URL-i** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ITRP** dialoogiboksi **SAML SSO URL-i** väärtus kopeerida ja seejärel kleepige **URL-i SAML SSO** tekstiväli.
    4.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Serdi sõrmejälje** tekstiväli.
        
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    5.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate ITRP sisse logida, nad peavad olema ettevalmistatud ITRP.  
Puhul ITRP, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **ITRP** rentniku.

2.  Klõpsake tööriistaribal ülaosas **kirjed**.

    ![Administraator] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Administraator")

3.  Valige hüpikmenüüst, **inimesed**.

    ![Inimesed] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Inimesed")

4.  Klõpsake nuppu **Lisa uus isiku** ("+").

    ![Administraator] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Administraator")

5.  Klõpsake dialoogiboksis uue isiku lisamiseks tehke järgmist.

    ![Kasutaja] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Kasutaja")

    1.  Tippige **nimi**, **e-posti** kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud ITRP kasutaja konto loomise tööriistade või API-de esitatud ITRP AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>ITRP kasutajate määramiseks tehke järgmist.

1.  Azure AD portaalis test konto loomine.

2.  Klõpsake lehel **ITRP **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).