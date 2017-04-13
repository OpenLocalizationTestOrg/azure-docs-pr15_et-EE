<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Panorama9 | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Panorama9 abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Õpetus: Azure'i Active Directory integreerimine Panorama9
  
Selle õpetuse eesmärk on integreerimine Azure ja Panorama9 kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Panorama9 ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Panorama9 ühekordse sisselogimise rakendusse Panorama9 ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Panorama9 lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-panorama9"></a>Rakenduste integreerimise jaoks Panorama9 lubamine
  
Selle jaotise eesmärk on liigendamine Panorama9 jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Panorama9 lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Panorama9**.

    ![Rakenduse Galerii] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Panorama9**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Panorama9 oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Panorama9 konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Panorama9** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Panorama9 logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Panorama9 Logi sisse URL** väljale Tippige oma URL-i kasutavad kasutajad sisse logida Panorama9 (nt: "*https://dashboard.panorama9.com/saml/access/3262*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Panorama9 veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage see kohalikult teie arvutis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Panorama9 ettevõtte administraatorina.

6.  Klõpsake tööriistaribal nuppu **Halda**ja klõpsake **laiendid**.

    ![Laiendid] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Laiendid")

7.  Klõpsake dialoogiboksis **laiendid** **Ühekordse sisselogimise**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Ühekordne sisselogimine")

8.  Tehke jaotises **sätted** järgmist.

    ![Sätted] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Sätted")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Panorama9** dialoogiboksi **Ühekordse sisselogimise teenuse URL-i** väärtus kopeerida ja seejärel kleepige **URL-i identiteedi pakkuja** tekstiväli.
    2.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **serdi sõrmejälje** tekstiväli.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    3.  Klõpsake nuppu **Salvesta**.

9.  Azure AD klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja **Complete** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks klõpsake.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Panorama9 sisse logida, ta peab olema ettevalmistatud Panorama9.  
Puhul Panorama9, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Panorama9** ettevõtte saidi administraator.

2.  Menüü peal, klõpsake käsku **Halda**ja klõpsake **kasutajaid**.

    ![Kasutajatele] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Kasutajatele")

3.  Klõpsake nuppu **+**.

4.  Jaotises kasutaja tehke järgmist.

    ![Kasutajatele] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Kasutajatele")

    1.  Tippige tekstiväljale **e-posti** meiliaadress lubatud Azure Active Directory kasutaja soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE]Saate kasutada mis tahes muud Panorama9 kasutaja konto loomise tööriistade või API-de esitatud Panorama9 AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Panorama9 kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Panorama9** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).