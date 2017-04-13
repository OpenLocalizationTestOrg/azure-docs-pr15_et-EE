<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine FM: süsteemid | Microsoft Azure'i" 
    description="Siit saate teada, kuidas kasutada FM: Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud süsteemid!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Õpetus: Azure'i Active Directory integreerimine FM: süsteemid
  
Selle õpetuse eesmärk on integreerimine Azure ja FM:Systems kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   FM:Systems ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud FM:Systems ühekordse sisselogimise rakendusse FM:Systems ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks FM:Systems lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Rakenduste integreerimise jaoks FM:Systems lubamine
  
Selle jaotise eesmärk on liigendamine FM:Systems jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Rakenduste integreerimise jaoks FM:Systems lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **FM:Systems**.

    ![Rakenduse Galerii] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **FM:Systems**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![FM: süsteemide] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: süsteemide")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks FM:Systems oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **FM:Systems** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida FM:Systems** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Rakenduse URL-i konfigureerimine")

    1.  Tippige tekstiväljale **URL FM:Systems Logi sisse** oma FM:Systems **Vastus URL-i** (nt: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Saate selle väärtuse oma FM: süsteemide tugitöötajate poole.

    2.  Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil FM:Systems** oma metaandmete allalaadimiseks klõpsake **metaandmete alla laadida**, ja seejärel salvestage metaandmeid teie arvutis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Ühekordse sisselogimise konfigureerimine")

5.  Esitage oma FM metaandmete allalaaditud faili: süsteemide tugitöötajate poole.

    >[AZURE.NOTE] Teie FM: Süsteemide tugimeeskonna on tegelik SSO konfigureerimine.
Saate teatise, kui tellimuse jaoks on lubatud SSO-d.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate FM:Systems sisse logida, nad peavad olema ettevalmistatud FM:Systems.  
Puhul FM:Systems, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige veebibrauseri akna, saidile FM:Systems ettevõtte administraatorina.

2.  Minge **süsteemi halduse \> haldamine turvalisus \> kasutajate \> kasutajate loendit**.

    ![Süsteemi haldamine] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Süsteemi haldamine")

3.  Klõpsake nuppu **Loo uus kasutaja**.

    ![Uue kasutaja loomine] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Uue kasutaja loomine")

4.  Jaotises **Kasutaja loomiseks** tehke järgmist.

    ![Kasutaja loomine] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Kasutaja loomine")

    1.  Sisestage kasutajanimi, parool ja kinnituse, e-posti aadress ja kehtiv Azure Active Directory kontot soovite säte seotud tekstiväljad töötaja ID-d.
    2.  Klõpsake nuppu **edasi**.

>[AZURE.NOTE] Saate kasutada mis tahes muud FM:Systems kasutaja konto loomise tööriistu või API-de esitatud FM:Systems AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>FM:Systems kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **FM:Systems **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).