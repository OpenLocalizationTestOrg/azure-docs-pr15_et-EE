<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Zendesk | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Zendesk abil!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Õpetus: Azure'i Active Directory integreerimine Zendesk
  
Selle õpetuse eesmärk on integreerimine Azure ja Zendesk kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Zendesk rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Zendesk ühekordse sisselogimise rakendusse Zendesk ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Zendesk lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-zendesk"></a>Rakenduste integreerimise jaoks Zendesk lubamine
  
Selle jaotise eesmärk on liigendamine Zendesk jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Zendesk lubamiseks tehke järgmist.

1.  Klõpsake vasakpoolsel navigeerimispaanil Azure'i haldusportaal **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Zendesk**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Zendesk**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Zendesk oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Zendesk konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Lehel **Zendesk** rakenduse integreerimine Azure AD portaalis nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Zendesk logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Konfigureerimine Ühekordne sisselogimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Rakenduse URL-i konfigureerimine")
  
    lisamine. Tippige tekstiväljale **Zendesk sisselogimise URL** oma URL, kasutades järgmist mustrit:`https://<tenant-name>.zendesk.com`

    b. Klõpsake nuppu **edasi**.



4.  Lehel **Konfigureeri ühekordse sisselogimise Zendesk juures** nuppu **Laadi alla serdi**ja salvestage serdi faili kohalikult oma compiter.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Zendesk ettevõtte administraatorina.

6.  Klõpsake linki **administraator**.

7.  Klõpsake vasakpoolsel navigeerimispaanil nuppu **sätted**ja seejärel vahekaarti **Turvalisus**.

    ![Turvalisus] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Turvalisus")

8.  **Lehel,** klõpsake vahekaarti **haldus ja agentide** .

9.  Valige **ühekordse sisselogimise (SSO) ja SAML**, ja seejärel valige **SAML**.

10. Azure AD portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Zendesk** **SAML SSO URL-i** väärtus kopeerida ja seejärel kleepige **URL-i SAML SSO** tekstiväli.

11. Azure AD portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Zendesk** kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **Remote välju URL-i** tekstiväli.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Ühekordne sisselogimine")

12. Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Serdi sõrmejälje** tekstiväli.

    >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

13. Klõpsake nuppu **Salvesta**.

14. Azure AD portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate **Zendesk**sisse logida, nad peavad olema ettevalmistatud **Zendesk**.  
Puhul **Zendesk**ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Ettevalmistamise Zendesk kasutajakonto, tehke järgmist.

1.  Logige sisse oma **Zendesk** rentniku.

2.  Valige vahekaart **Klientide loendi** .

3.  Valige vahekaart **kasutaja** ja klõpsake nuppu **Lisa**.

    ![Kasutaja lisamine] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Kasutaja lisamine")

4.  Tippige meiliaadress, olemasoleva Azure AD konto soovite näha, ja klõpsake siis nuppu **Salvesta**.

    ![Uus kasutaja] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Uus kasutaja")

>[AZURE.NOTE] Saate kasutada mis tahes muud Zendesk kasutaja konto loomise tööriistade või API-de esitatud Zendesk AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Zendesk kasutajate määramiseks tehke järgmist.

1.  Azure AD portaalis test konto loomine.

2.  Klõpsake lehel **Zendesk **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
