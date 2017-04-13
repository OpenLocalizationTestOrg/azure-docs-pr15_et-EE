<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Veracode | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Veracode abil!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Õpetus: Azure'i Active Directory integreerimine Veracode
  
Selle õpetuse eesmärk on integreerimine Azure ja Veracode kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Veracode ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Veracode ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Veracode lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-veracode"></a>Rakenduste integreerimise jaoks Veracode lubamine
  
Selle jaotise eesmärk on liigendamine Veracode jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Veracode lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Veracode**.

    ![Rakenduse Galerii] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Veracode**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Veracode oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Rakenduse Veracode loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![Atribuudid] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atribuudid")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Veracode** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Veracode logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse sätete konfigureerimine** lehel klõpsake nuppu **edasi**.

    ![Rakenduse sätete konfigureerimine] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Rakenduse sätete konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Veracode veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Veracode ettevõtte administraatorina.

6.  Ülemises menüüs nuppu **sätted**ja seejärel klõpsake nuppu **administraator**.

    ![Haldus] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Haldus")

7.  Klõpsake vahekaarti **SAML** .

8.  Jaotises **Ettevõtte SAML sätted** tehke järgmist.

    ![Haldus] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Haldus")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Veracode** dialoogiboksi **Väljaandja URL-i** väärtus kopeerida, ja seejärel kleepige **väljaandja** tekstiväli
    2.  Laadige oma allalaaditud serdi, klõpsake **Faili valimine**.
    3.  Valige **Luba ise registreerimise**.

9.  **Enda loodud registreerimise sätted** jaotises järgmiste toimingute ja klõpsake siis nuppu **Salvesta**.

    ![Haldus] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Haldus")

    1.  Kui **Uus kasutaja aktiveerimine**, valige **Ei aktiveerimine vajalik**.
    2.  **Kasutaja andmete värskendused**, valige **Eelistused Veracode kasutaja andmed**.
    3.  **SAML atribuut üksikasjade**kuvamiseks valige järgmist:
        -   **Kasutaja rollid**
        -   **Poliitika administraator**
        -   **Läbivaataja**
        -   **Juhi turvalisus**
        -   **Kommenteeritud**
        -   **Saatja**
        -   **Looja**
        -   **Kõik skannida tüübid**
        -   **Meeskonnatöö liikmelisus**
        -   **Vaikimisi meeskonnatöö**

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Ühekordse sisselogimise konfigureerimine")

11. Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** .

    ![Atribuudid] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Atribuudid")

12. Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ![Atribuudid] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atribuudid")

  	| Atribuudi nimi | Atribuudi väärtus |
  	|:---------------|:----------------|
  	| eesnimi      | User.givenName  |
  	| perekonnanimi       | User.surname    |
  	| e-posti          | User.mail       |

    1.  Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.
    
    2.  Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.

    3.  **Atribuudi väärtust** tekstiväli, valige selle rea atribuudi väärtust.

    4.  Klõpsake nuppu **valmis**.

13. Klõpsake nuppu **Rakenda muudatused**.

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Veracode sisse logida, nad peavad olema ettevalmistatud Veracode.  
Puhul Veracode, ettevalmistamise on automatiseeritud ülesanne.  
On teie jaoks toimingu üksusi pole.
  
Kasutajate luuakse automaatselt vajadusel ühekordse sisselogimise katsel ajal.

>[AZURE.NOTE] Saate kasutada mis tahes muud Veracode kasutaja konto loomise tööriistade või API-de esitatud Veracode AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Veracode kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Veracode **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).