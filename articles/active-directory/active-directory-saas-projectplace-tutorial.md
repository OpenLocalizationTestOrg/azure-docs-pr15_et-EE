<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Projectplace | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Projectplace abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Õpetus: Azure'i Active Directory integreerimine Projectplace
  
Selle õpetuse eesmärk on integreerimine Azure ja Projectplace kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Projectplace ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Projectplace ühekordse sisselogimise rakendusse Projectplace ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Projectplace lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-projectplace"></a>Rakenduste integreerimise jaoks Projectplace lubamine
  
Selle jaotise eesmärk on liigendamine Projectplace jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Projectplace lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Projectplace**.

    ![Rakenduse Galerii] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Projectplace**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Projectplace oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Projectplace** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Ühe logima konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Projectplace** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Ühe logima konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Projectplace Logi sisse URL** väljale Tippige oma ProjectPlace rentniku URL (nt: "*http://company.projectplace.com*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise Projectplace juures** **metaandmete allalaadimine**nuppu ja salvestage metaandmete faili oma arvutist.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Ühe logima konfigureerimine")

5.  Projectplace tugimeeskonnale saata selle metadatafile.

    >[AZURE.NOTE] Ühekordse sisselogimise konfigureerimine on vaja teha Projectplace tugimeeskonnalt. Saate teatise niipea, kui konfiguratsioon on lõpule viidud.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Ühe logima konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Projectplace sisse logida, nad peavad olema ettevalmistatud Projectplace.  
Puhul Projectplace, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Projectplace** ettevõtte saidi administraator.

2.  Minge lehele **inimesed**ja seejärel klõpsake nuppu **liikmed**.

    ![Inimesed] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Inimesed")

3.  Klõpsake nuppu **Lisa liige**.

    ![Liikmete lisamine] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Liikmete lisamine")

4.  Tehke jaotises **Lisa liige** järgmist:

    ![Uute liikmete] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Uute liikmete")

    1.  **Uute liikmete** tekstivälja, Tippige meiliaadress, kelle soovite seotud tekstiväljad säte kehtiv AAD konto.
    2.  Klõpsake nuppu **saada**

        >[AZURE.NOTE] E-posti, sh konto kinnitada, kuni see muutub aktiivne link saadetakse Azure Active Directory konto omanik.
    
>[AZURE.NOTE]Saate kasutada mis tahes muud Projectplace kasutaja konto loomise tööriistu või API-de esitatud Projectplace AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Projectplace kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Projectplace **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).