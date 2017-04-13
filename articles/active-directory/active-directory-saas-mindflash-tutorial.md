<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Mindflash | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Mindflash abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Õpetus: Azure'i Active Directory integreerimine Mindflash
  
Selle õpetuse eesmärk on integreerimine Azure ja Mindflash kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Mindflash ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Mindflash ühekordse sisselogimise rakendusse Mindflash ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Mindflash lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-mindflash"></a>Rakenduste integreerimise jaoks Mindflash lubamine
  
Selle jaotise eesmärk on liigendamine Mindflash jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Mindflash lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Mindflash**.

    ![Rakenduse Galerii] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Mindflash**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Mindflash oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Mindflash** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Mindflash logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.mindflash.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise Mindflash juures** **metaandmete allalaadimine**nuppu ja salvestage metaandmete faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Ühekordse sisselogimise konfigureerimine")

5.  Mindflash tugimeeskonnale saata selle metadatafile.

    >[AZURE.NOTE] Ühekordse sisselogimise konfigureerimine on vaja teha Mindflash tugimeeskonnalt. Saate teatise niipea, kui konfiguratsioon on lõpule viidud.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Mindflash sisse logida, nad peavad olema ettevalmistatud Mindflash.  
Puhul Mindflash, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Mindflash** ettevõtte saidi administraator.

2.  Minge **kasutajate haldamine**.

    ![Kasutajate haldamine] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Kasutajate haldamine")

3.  Klõpsake nuppu **Lisa kasutajad**ja seejärel nuppu **Uus**.

4.  **Uute kasutajate lisamine** jaotises tehke järgmist.

    ![Uute kasutajate lisamine] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Uute kasutajate lisamine")

    1.  Tippige **eesnimi**, **perekonnanimi** ja **e-posti** kehtiv AAD konto soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Lisa**.

>[AZURE.NOTE]Saate kasutada mis tahes muud Mindflash kasutaja konto loomise tööriistade või API-de esitatud Mindflash AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Mindflash kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Mindflash **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).