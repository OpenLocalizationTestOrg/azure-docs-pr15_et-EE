<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Benefitsolver | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory Benefitsolver abil!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Õpetus: Azure'i Active Directory integreerimine Benefitsolver

Selle õpetuse eesmärk on integreerimine Azure ja Benefitsolver kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Benefitsolver ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Benefitsolver ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Benefitsolver lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Rakenduste integreerimise jaoks Benefitsolver lubamine

Selle jaotise eesmärk on liigendamine Benefitsolver jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Benefitsolver lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Benefitsolver**.

    ![Rakenduse Galerii] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Benefitsolver**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Benefitsolver oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Rakenduse Benefitsolver loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![Atribuudid] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atribuudid")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Benefitsolver** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Benefitsolver** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Ühekordse sisselogimise konfigureerimine")

3.  Klõpsake lehel **Rakenduse sätete konfigureerimine** tehke järgmist.

    ![Rakenduse sätete konfigureerimine] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Rakenduse sätete konfigureerimine")

    1.  Tippige tekstiväljale **Logi sisse URL-i** **http://azure.benefitsolver.com**.
    2.  Tippige tekstiväljale **Vastus URL-i** **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Klõpsake nuppu **edasi**.

4.  Lehel **Konfigureeri ühekordse sisselogimise Benefitsolver veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Ühekordse sisselogimise konfigureerimine")

5.  Metaandmete allalaaditud faili Benefitsolver tugimeeskonnale saata.

    >[AZURE.NOTE] Benefitsolver tugimeeskonnale on tegelik SSO konfigureerimine.
Saate teatise, kui tellimuse jaoks on lubatud SSO-d.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Ühekordse sisselogimise konfigureerimine")

7.  Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** .

    ![Atribuudid] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Atribuudid")

8.  Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ![Atribuudid] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Atribuudid")

  	|Atribuudi nimi|Atribuudi väärtus|
  	|---|---|
  	|ClientID|Peate selle väärtuse toomine Benefitsolver tugimeeskonnale.|
  	|ClientKey|Peate selle väärtuse toomine Benefitsolver tugimeeskonnale.|
  	|LogoutURL|Peate selle väärtuse toomine Benefitsolver tugimeeskonnale.|
  	|Töötaja ID|Peate selle väärtuse toomine Benefitsolver tugimeeskonnale.|

    1.  Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.
    3.  **Atribuudi väärtust** tekstiväli, valige selle rea atribuudi väärtust.
    4.  Klõpsake nuppu **valmis**.

9.  Klõpsake nuppu **Rakenda muudatused**.

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Benefitsolver sisse logida, ta peab olema ettevalmistatud Benefitsolver.  
Benefitsolver, on töötajate andmeid oma rakenduse täidetud rahvaloendus faili HRIS süsteemist (tavaliselt öösel) kaudu.  

>[AZURE.NOTE] Saate kasutada mis tahes muud Benefitsolver kasutaja konto loomise tööriistade või API-de esitatud Benefitsolver AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Benefitsolver kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Benefitsolver **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
