<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine lõuend LMS | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada lõuend LMS Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Õpetus: Azure'i Active Directory integreerimine lõuendile LMS

Selle õpetuse eesmärk on kuvamiseks integreerimine Azure ja lõuend.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Lõuend rentniku jaoks

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud lõuend ühekordse sisselogimise rakendusse lõuend ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise lõuend lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-canvas"></a>Rakenduste integreerimise lõuend lubamine

Selle jaotise eesmärk on liigendamine lõuend rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Rakenduste integreerimise lõuend lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **lõuend**.

    ![Rakenduse Galerii] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **lõuend**, ja klõpsake **lõpuleviimine** rakenduse lisamiseks.

    ![Lõuend] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Lõuend")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks lõuend oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise lõuend konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **lõuend** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajad logida lõuendile** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Lõuend sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga `https://<tenant-name>.instructure.com`, ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise lõuend veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas logige saidile lõuend ettevõtte administraatorina.

6.  Minge **kursusi \> hallatavate kontode \> Microsoft**.

    ![Lõuend] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Lõuend")

7.  Klõpsake vasakpoolsel navigeerimispaanil, valige **autentimist**ja seejärel klõpsake nuppu **Lisa uus SAML Config**.

    ![Autentimine] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Autentimine")

8.  Praeguse integreerimine lehel tehke järgmist.

    ![Praeguse integreerimine] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Praeguse integreerimine")

    1.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil lõuend** dialoogiboksi **Üksuse ID** väärtus kopeerida ja seejärel kleepige **IdP üksuse ID** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil lõuendile** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ning kleepige need **Logi sisse URL** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil lõuend** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Logi välja URL** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil lõuend** dialoogiboksi kopeerige **Muuda parooli URL-i** väärtus ja seejärel kleepige see tekstiväli **Linki Muuda parooli** .
    5.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Serdi sõrmejälje** tekstiväli.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    6.  Valige loendist **Login atribuut** **NameID**.
    7.  Valige loendist **Identifikaator vorming** **emailAddress**.
    8.  Klõpsake nuppu **Salvesta autentimissätted**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate lõuend sisse logida, ta peab olema ettevalmistatud lõuend.  
Lõuend, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma rentniku **lõuend** .

2.  Minge **kursusi \> hallatavate kontode \> Microsoft**.

    ![Lõuend] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Lõuend")

3.  Klõpsake nuppu **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Kasutajatele")

4.  Klõpsake nuppu **Lisa uus kasutaja**.

    ![Kasutajatele] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Kasutajatele")

5.  Uus kasutaja dialoogiboksi lehe lisamine, tehke järgmist.

    ![Kasutaja lisamine] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Kasutaja lisamine")

    1.  Tippige väljale **Täisnimi** kasutaja nimi.
    2.  Tippige tekstiväljale **e-posti** kasutaja meiliaadress.
    3.  Tippige väljale **Login** kasutaja Azure AD meiliaadress.
    4.  Valige **e-posti kasutaja selle konto loomise kohta**.
    5.  Klõpsake nuppu **Lisa kasutaja**.

>[AZURE.NOTE] Saate kasutada mis tahes muud lõuend kasutaja konto loomise tööriistade või API-de esitatud AAD kasutajakontode lõuend.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Lõuend kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **lõuend **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
