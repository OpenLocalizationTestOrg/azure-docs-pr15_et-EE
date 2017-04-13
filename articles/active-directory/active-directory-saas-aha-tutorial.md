<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Aha! | Microsoft Azure'i" 
    description="Siit saate teada, kuidas kasutada Aha! ühekordse sisselogimise lubamiseks Azure Active Directory, automatiseeritud ettevalmistamise ja palju muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Õpetus: Azure'i Active Directory integreerimine Aha!

Selle õpetuse eesmärk on integreerimine Azure ja Aha kuvamiseks!  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Mõne ahaa! ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, Azure AD kasutajate olete määranud Aha! on võimalik, et ühe logige sisse oma Aha rakenduse! ettevõtte saidil (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Lubada rakenduste integreerimise jaoks Aha!
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-aha-tutorial/IC798944.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-aha"></a>Lubada rakenduste integreerimise jaoks Aha!

Selle jaotise eesmärk on liigendamine lubamine rakenduste integreerimise jaoks Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Aha lubamiseks!, tehke järgmist:

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-aha-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-aha-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-aha-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Aha!**.

    ![Rakenduse Galerii] (./media/active-directory-saas-aha-tutorial/IC798945.png "Rakenduse Galerii")

7.  Valige paanil tulemuste **Aha!**, ja seejärel klõpsake nuppu **täielik** rakenduse lisamiseks.

    ![Ahaa!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Ahaa!")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selles jaotises eesmärk liigendamine lubamine kasutajate autentimiseks Aha! oma konto Azure AD federation SAML protokolli abil.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis, klõpsake selle **Aha!** rakenduste integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-aha-tutorial/IC798946.png "Ühekordse sisselogimise konfigureerimine")

2.  Klõpsake soovitud **Kuidas soovite kasutajad logida Aha!** lehe, valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-aha-tutorial/IC798947.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Aha! Logige sisse URL-i** tekstivälja, tippige URL-i kasutavad kasutajad sisselogimise oma Aha! Rakendus (nt: "*https://company.aha.io/session/new*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-aha-tutorial/IC798948.png "Rakenduse URL-i konfigureerimine")

4.  Klõpsake soovitud **Aha ühekordse sisselogimise konfigureerimine!** lehe metaandmete faili alla laadida, klõpsake nuppu **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-aha-tutorial/IC798949.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse oma Aha! ettevõtte saidi administraator.

6.  Menüü peal, klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-aha-tutorial/IC798950.png "Sätted")

7.  Klõpsake nuppu **konto**.

    ![Profiil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profiil")

8.  Klõpsake **turbe- ja ühekordse sisselogimise**.

    ![Turvalisus ja Ühekordne sisselogimine] (./media/active-directory-saas-aha-tutorial/IC798952.png "Turvalisus ja Ühekordne sisselogimine")

9.  Valige jaotises **Ühekordse sisselogimise** **Identiteedipakkuja**, **SAML2.0**.

    ![Turvalisus ja Ühekordne sisselogimine] (./media/active-directory-saas-aha-tutorial/IC798953.png "Turvalisus ja Ühekordne sisselogimine")

10. **Ühekordse sisselogimise** konfiguratsiooni lehele, tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-aha-tutorial/IC798954.png "Ühekordne sisselogimine")

    1.  Tippige **nimi** rühmitusaluse nimi oma konfiguratsioon.
    2.  Valige **Konfigureeri abil**, **Metaandmete fail**.
    3.  Metaandmete allalaaditud faili üleslaadimiseks klõpsake nuppu **Sirvi**.
    4.  Klõpsake nuppu **Värskenda**.

11. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-aha-tutorial/IC798955.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate logige Aha!, ta peab olema ettevalmistatud Aha!.  
Puhul Aha!, ettevalmistamise on automatiseeritud ülesanne.  
On teie jaoks toimingu üksusi pole.
  
Kasutajate luuakse automaatselt vajadusel ühekordse sisselogimise katsel ajal.

>[AZURE.NOTE] Saate kasutada mis tahes muud Aha! Kasutaja konto loomise tööriistade või API-de esitatud Aha! Kui soovite AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Määrata kasutajad Aha!, tehke järgmist:

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake soovitud **ahaa!** rakenduste integreerimise klõpsake **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-aha-tutorial/IC798956.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-aha-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
