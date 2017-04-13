<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine BlueJeans | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory BlueJeans kasutamine!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Õpetus: Azure'i AD BlueJeans integreerimine

Selle õpetuse eesmärk on integreerimine Azure ja BlueJeans kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   BlueJeans ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud BlueJeans ühekordse sisselogimise rakendusse BlueJeans ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks BlueJeans lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Rakenduste integreerimise jaoks BlueJeans lubamine

Selle jaotise eesmärk on liigendamine BlueJeans jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Rakenduste integreerimise jaoks BlueJeans lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **BlueJeans**.

    ![Rakenduse Galerii] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **BlueJeans**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks BlueJeans oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **BlueJeans** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajad logida BlueJeans** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **BlueJeans Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.BlueJeans.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise BlueJeans veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil **BlueJeans** ettevõtte administraatorina.

6.  Minge **administraator \> rühmade sätete \> turvalisus**.

    ![Administraator] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Administraator")

7.  Jaotises **Turvalisus** , tehke järgmist.

    ![SAML ühekordse sisselogimise] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML ühekordse sisselogimise")

    1.  Valige **SAML ühekordse sisselogimise**.
    2.  Valige **Luba automaatne ettevalmistamise**.

8.  Teisaldamine tehke järgmist:

    ![Serdi tee] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Serdi tee")

    1.  **Valige**fail ja laadige alla laaditud serdi.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil BlueJeans** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    3.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil BlueJeans** dialoogiboksi **Muuda parooli URL-i** väärtus kopeerida ja seejärel kleepige **URL-i parooli muutmine** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil BlueJeans** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.

9.  Liikuma tehke järgmist:

    ![Muudatuste salvestamine] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Muudatuste salvestamine")

    1.  Tippige tekstiväljale **kasutaja ID-d** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  Tippige tekstiväljale **e-posti** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Klõpsake nuppu **Salvesta muudatused**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate BlueJeans sisse logida, nad peavad olema ettevalmistatud BlueJeans.  
Puhul BlueJeans, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **BlueJeans** ettevõtte administraatorina.

2.  Minge **administraator \> kasutajate haldamine \> Lisa kasutaja**.

    ![Administraator] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Administraator")

    >[AZURE.IMPORTANT] Menüü **Lisa kasutaja** on ainult juhul, kui klõpsake **vahekaarti Turvalisus ja** **Luba automaatne ettevalmistamise** on märkimata.

3.  Tehke jaotises **Lisa kasutaja,** toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Kasutaja lisamine")

    1.  Tippige **BlueJeans kasutajanimi**, **meiliaadress**, on **BlueJeans koosoleku ID**, **Moderaator pääsukood**, **Täisnimi**, **ettevõtte** kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Lisa kasutaja**.

>[AZURE.NOTE] Saate kasutada mis tahes muud BlueJeans kasutaja konto loomise tööriistu või API-de osutavad BlueJeans kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>BlueJeans kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **BlueJeans **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
