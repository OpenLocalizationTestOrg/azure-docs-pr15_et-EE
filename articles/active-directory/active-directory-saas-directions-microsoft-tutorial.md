<properties 
    pageTitle="Õpetus: Juhiseid Microsoft Azure Active Directory integreerimine | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada juhiseid Microsoft Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Õpetus: Juhiseid Microsoft Azure Active Directory integreerimine

Selle õpetuse eesmärk on kuvamiseks Microsoft Azure Active Directory ja juhiseid.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Juhised, Microsoft tellimuse

Kui teil pole veel Microsofti tellimuse ühendatud juhised, e-posti taotluse "*service@DirectionsOnMicrosoft.com*".

Pärast selle õpetuse, saab ühekordse sisselogimise kasutamine Ühekordne sisselogimine rakendusse olete määranud juhiseid Microsoft Azure Active Directory kasutajad.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimine Microsoft suunast lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Rakenduste integreerimine Microsoft suunast lubamine

Selle jaotise eesmärk on liigendamine suunast Microsoftile rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Rakenduste integreerimine Microsoft suunast lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **juhiseid Microsoft**.

    ![Rakenduse Galerii] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **juhised Microsoft**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Stsenaarium] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Stsenaarium")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks oma kontoga juhiseid Microsoft Azure AD abil SAML protokolli federation lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **juhiseid Microsoft** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Ühekordse sisselogimise lubamiseks")

2.  Lehel **Kuidas soovite kasutajad logida Microsoft juhiseid,** valige **Microsoft Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Microsoft Azure'i AD Singel sisselogimine] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure'i AD Singel sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** Logi sisse URL väljale tippige **https://www.directionsonmicrosoft.com/user/login**ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise juhiseid Microsofti veebisaidil** klõpsake nuppu **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Ühekordse sisselogimise konfigureerimine")

5.  Metaandmete faili saata juhiseid Microsofti toe meeskond (*service@DirectionsOnMicrosoft.com*). Microsofti toe meeskond teie liikmestaatus ühendatud saidi leidmiseks juhiseid lubamiseks lisage oma ettevõtte andmete meili.

    >[AZURE.NOTE] Ühekordse sisselogimise suunast Microsoft peab olema lubatud juhiseid sisse Microsofti toe poole.
Saate teate, kui ühekordse sisselogimise on lubatud.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

On teil konfigureerida ettevalmistamise Microsoft juhiseid, et kasutaja toimingu üksusi pole.  
Kui on määratud kasutaja üritab juhiseid, Microsoft Accessi paneeli abil sisse logida, Microsoft juhiseid, kontrollib, kas kasutajal on olemas. Kui kasutajakonto pole saadaval veel, on see automaatselt loodud juhiseid Microsoft.
##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Juhised Microsoft kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **juhised Microsoftile **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Jah")
