<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine saadiku | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory saadiku kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Õpetus: Azure'i Active Directory integreerimine saadiku
  
Selle õpetuse eesmärk on integreerimine Azure ja saadiku kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Saadiku rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud saadiku ühekordse sisselogimise rakendusse saadiku ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks saadiku lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-envoy"></a>Rakenduste integreerimise jaoks saadiku lubamine
  
Selle jaotise eesmärk on liigendamine saadiku jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Rakenduste integreerimise jaoks saadiku lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **saadiku**.

    ![Rakenduse Galerii] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **saadiku**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Saadiku] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Saadiku")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks saadiku oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks saadiku konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **saadiku** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Ühekordse sisselogimise lubamiseks")

2.  Lehel **Kuidas soovite kasutajad logida saadiku** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Saadiku sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Envoy.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise saadiku veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\Envoy.cer**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil saadiku ettevõtte administraatorina.

6.  Klõpsake tööriistaribal nuppu **sätted**.

    ![Saadiku] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Saadiku")

7.  Klõpsake **ettevõtte**.

    ![Ettevõtte] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Ettevõtte")

8.  Klõpsake **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  **SAML autentimise** konfigureerimine jaotises tehke järgmist.

    ![SAML autentimine] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML autentimine")

    >[AZURE.NOTE] Rakenduse genereeritud automaatne HQ asukoht ID väärtus.

    1.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **sõrmejälje** tekstiväli.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil saadiku** dialoogiboksi kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja HTTP SAML URL-i** tekstiväli.
    3.  Klõpsake nuppu **Salvesta muudatused**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida ettevalmistamine saadiku kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab saadiku Accessi paneeli abil sisse logida, saadiku kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud saadiku.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Saadiku kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **saadiku **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).