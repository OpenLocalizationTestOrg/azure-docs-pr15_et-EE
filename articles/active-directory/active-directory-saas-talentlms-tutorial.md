<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine TalentLMS | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory TalentLMS abil!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Õpetus: Azure'i Active Directory integreerimine TalentLMS
  
Selle õpetuse eesmärk on integreerimine Azure ja TalentLMS kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   TalentLMS rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud TalentLMS ühekordse sisselogimise rakendusse TalentLMS ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks TalentLMS lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-talentlms"></a>Rakenduste integreerimise jaoks TalentLMS lubamine
  
Selle jaotise eesmärk on liigendamine TalentLMS jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Rakenduste integreerimise jaoks TalentLMS lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **TalentLMS**.

    ![Rakenduse Galerii] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **TalentLMS**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks TalentLMS oma konto abil SAML protokolli federation Azure AD lubamise kohta. .  
Ühekordse sisselogimise jaoks TalentLMS konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **TalentLMS** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate TalentLMS logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **TalentLMS sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. TalentLMSapp.com*", ja seejärel klõpsake nuppu **edasi**.

    ![URL-i sisse logida] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "URL-i sisse logida")

4.  Lehel **Konfigureeri ühekordse sisselogimise TalentLMS veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\TalentLMS.cer**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil TalentLMS ettevõtte administraatorina.

6.  Jaotises **konto ja sätted** vahekaarti **Kasutajad** .

    ![Konto ja sätted] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Konto ja sätted")

7.  **Ühekordne sisselogimine (SSO)**, klõpsake nuppu

8.  Ühekordse sisselogimise jaotises tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Ühekordne sisselogimine")

    1.  Valige loendist **Tüüp SSO integreerimine** **SAML 2.0**.
    2.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil TalentLMS** dialoogiboksi **Identiteedi pakkuja ID** väärtus kopeerida ja seejärel kleepige see **identiteedipakkuja (IdP)** tekstiväli.
    3.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Serdi sõrmejälje** tekstiväli.

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil TalentLMS** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **serveri URL** tekstiväli.
    5.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil TalentLMS** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **Remote väljunud URL-i** tekstiväli.
    6.  Tippige tekstiväljale **TargetedID** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  Tippige tekstiväljale **eesnimi** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  Tippige tekstiväljale **perekonnanimi** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  Tippige tekstiväljale **e-posti** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate TalentLMS sisse logida, nad peavad olema ettevalmistatud TalentLMS.  
Puhul TalentLMS, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **TalentLMS** rentniku.

2.  Klõpsake nuppu **Kasutajad**ja seejärel klõpsake nuppu **Lisa kasutaja**.

3.  Klõpsake lehel **Lisa kasutaja** dialoogiboksis tehke järgmist:

    ![Kasutaja lisamine] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Kasutaja lisamine")

    1.  Tippige seotud atribuudiväärtused Azure AD kasutajakonto järgmised tekstiväljad: **eesnimi**, **perekonnanimi**, **e-posti aadress**.
    2.  Klõpsake nuppu **Lisa kasutaja**.

>[AZURE.NOTE] Saate kasutada mis tahes muud TalentLMS kasutaja konto loomise tööriistade või API-de esitatud TalentLMS AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>TalentLMS kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **TalentLMS **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).