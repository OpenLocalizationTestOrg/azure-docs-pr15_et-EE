<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine RightAnswers | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory RightAnswers abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Õpetus: Azure'i Active Directory integreerimine RightAnswers
  
Selle õpetuse eesmärk on integreerimine Azure ja RightAnswers kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   RightAnswers ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud RightAnswers ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks RightAnswers lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-rightanswers"></a>Rakenduste integreerimise jaoks RightAnswers lubamine
  
Selle jaotise eesmärk on liigendamine RightAnswers jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>Rakenduste integreerimise jaoks RightAnswers lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **RightAnswers**.

    ![Rakenduse Galerii] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **RightAnswers**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks RightAnswers oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **RightAnswers** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate RightAnswers logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse sätete konfigureerimine** , **Logige sisse URL** väljale Tippige lehe URL, mida kasutatakse teie kasutajad sisselogimise RightAnswers rakenduse (nt: *https://fortify.rightanswers.com/portal/ss/*), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse sätete konfigureerimine] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Rakenduse sätete konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise RightAnswers veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Ühekordse sisselogimise konfigureerimine")

5.  Metaandmete allalaaditud faili RightAnswers tugimeeskonnale saata.

    >[AZURE.NOTE] RightAnswers tugimeeskonnale on tegelik SSO konfigureerimine.
Saate teatise, kui tellimuse jaoks on lubatud SSO-d.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate RightAnswers sisse logida, nad peavad olema ettevalmistatud RightAnswers.  
Puhul RightAnswers, ettevalmistamise on automatiseeritud ülesanne.  
On teie jaoks toimingu üksusi pole.
  
Kasutajate luuakse automaatselt vajadusel ühekordse sisselogimise katsel ajal.

>[AZURE.NOTE]Saate kasutada mis tahes muud RightAnswers kasutaja konto loomise tööriistade või API-de esitatud RightAnswers AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>RightAnswers kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **RightAnswers **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).