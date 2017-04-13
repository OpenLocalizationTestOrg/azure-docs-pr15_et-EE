<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine nurgakivi nõudmisel | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory nurgakivi nõudmisel kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Õpetus: Azure'i Active Directory integreerimine nurgakivi nõudmisel

Selle õpetuse eesmärk on integreerimine Azure ja nurgakivi nõudmisel kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Nurgakivi nõudmisel rentniku jaoks

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud nurgakivi nõudmisel ühekordse sisselogimise rakendusse nurgakivi nõudmisel ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Nurgakivi nõudmisel jaoks rakenduse integreerimise lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Nurgakivi nõudmisel jaoks rakenduse integreerimise lubamine

Selle jaotise eesmärk on liigendamine jaoks nurgakivi nõudmisel rakenduste integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Rakenduste integreerimise jaoks nurgakivi nõudmisel lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **nurgakivi nõudmisel**.

    ![Rakenduse Galerii] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Nurgakivi nõudmisel**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Nurgakivi nõudmisel] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Nurgakivi nõudmisel")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks nurgakivi nõudmisel oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **Nurgakivi nõudmisel** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Ühekordse sisselogimise lubamiseks")

2.  **Kuidas soovite kasutajad logida nurgakivi nõudmisel** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Microsoft Azure'i AD ühekordse sisselogimise] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure'i AD ühekordse sisselogimise")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Nurgakivi nõudmisel sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.csod.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise nurgakivi nõudmisel veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Ühekordse sisselogimise konfigureerimine")

5.  Saada tugimeeskonna nurgakivi nõudmisel järgmised üksused:

    1.  Allalaaditud serdi
    2.  **Remote sisselogimise URL-i** väärtus
    3.  **Remote välju URL-i** väärtus.

    >[AZURE.NOTE] Ühekordse sisselogimise peab olema konfigureeritud nurgakivi nõudmisel tugimeeskonnalt.
Saate teatise tugimeeskonnalt kui konfiguratsioon on lõpule viidud.

6.  Valige kinnituse ühekordse sisselogimise konfigureerimine, ja klõpsake nuppu **täielik** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate nurgakivi nõudmisel sisse logida, ta peab olema ettevalmistatud nurgakivi nõudmisel.  
Nurgakivi nõudmisel, ettevalmistamise on käsitsi tööülesande.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Saada teavet (nt: nimi, Emial) tugimeeskonna soovite sätte nurgakivi nõudmisel Azure AD kasutaja kohta.

>[AZURE.NOTE] Saate kasutada mis tahes muud nurgakivi nõudmisel kasutaja konto loomise tööriistade või API-de osutavad nurgakivi nõudmisel kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Nurgakivi nõudmisel kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Nurgakivi nõudmisel **rakenduste integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Kasutajate määramine] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Kasutajate määramine")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
