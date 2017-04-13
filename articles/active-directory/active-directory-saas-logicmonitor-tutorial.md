<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine LogicMonitor | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory LogicMonitor abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Õpetus: Azure'i Active Directory integreerimine LogicMonitor
  
Selle õpetuse eesmärk on integreerimine Azure ja LogicMonitor kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   LogicMonitor rentniku jaoks
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks LogicMonitor lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Rakenduste integreerimise jaoks LogicMonitor lubamine
  
Selle jaotise eesmärk on liigendamine LogicMonitor jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Rakenduste integreerimise jaoks LogicMonitor lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **logicmonitor**.

    ![Rakenduse Galerii] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **LogicMonitor**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks LogicMonitor oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **LogicMonitor **rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate LogicMonitor logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Logi sisse URL** väljale Tippige oma URL-i kasutavad kasutajad logida LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise LogicMonitor juures** nuppu **metaandmete alla laadida**ja oma arvutisse salvestada.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Ühekordse sisselogimise konfigureerimine")

5.  Logige sisse oma **LogicMonitor** ettevõtte saidi administraator.

6.  Menüü peal, klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Sätted")

7.  Klõpsake vasakus servas kohe navigeerimine, **Ühekordse sisselogimise**

    ![Ühekordne sisselogimine] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Ühekordne sisselogimine")

8.  **Ühekordne sisselogimine (SSO) sätted** jaotises tehke järgmist.

    ![Ühekordse sisselogimise sätted] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Ühekordse sisselogimise sätted")

    1.  Valige **ühekordse sisselogimise lubamiseks**.
    2.  Kui **Vaikimisi rolli määramine**, valige **kirjutuskaitstud**.
    3.  Avage allalaaditud metaandmete fail notepad, ja seejärel kleepige faili sisu **Identiteedi pakkuja metaandmete** tekstiväli.
    4.  Klõpsake nuppu **Salvesta muudatused**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
AAD kasutajad saaksid sisse logida, ta peab olema ette valmistatud LogicMonitor rakendusse kasutajanime Azure Active Directory abil.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma LogicMonitor ettevõtte saidi administraator.

2.  Menüü üleval, klõpsake nuppu **sätted**ja klõpsake **rollid ja kasutajad**.

    ![Rollid ja kasutajad] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Rollid ja kasutajad")

3.  Klõpsake nuppu **Lisa**.

4.  Tehke jaotises **Lisa konto** järgmist.

    ![Konto lisamine] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Konto lisamine")

    1.  Tippige Azure Active Directory kasutaja soovite säte seotud tekstiväljad **kasutajanimi**, **e-posti**, **parool** ja **parooli kinnitus** väärtused.
    2.  Valige **rollid**, **kuvamine** ja **olek**.
    3.  Klõpsake **esitada**.

>[AZURE.NOTE]Saate kasutada mis tahes muud LogicMonitor kasutaja konto loomise tööriistade või API-de esitatud LogicMonitor ettevalmistamine Azure Active Directory Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>LogicMonitor kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **LogicMonitor** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).




