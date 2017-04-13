<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Learningpool | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Learningpool abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Õpetus: Azure'i Active Directory integreerimine Learningpool
  
Selle õpetuse eesmärk on integreerimine Azure ja Learningpool kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Learningpool ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Learningpool ühekordse sisselogimise rakendusse Learningpool ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Learningpool lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-learningpool"></a>Rakenduste integreerimise jaoks Learningpool lubamine
  
Selle jaotise eesmärk on liigendamine Learningpool jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Learningpool lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Learningpool**.

    ![Rakenduse Galerii] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Learningpool**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Learningpool oma konto abil SAML protokolli federation Azure AD lubamise kohta.
  
Rakenduse Learningpool loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![SAML Turbeloa atribuudid] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML Turbeloa atribuudid")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Learningpool** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid** **SAML Turbeloa atribuutide** dialoogiboksi avamiseks.

    ![Atribuudid] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Atribuudid")

2.  Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ###  

  	|Atribuudi nimi                |Atribuudi väärtus            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenName   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.mail
  	|urn: oid:2.5.4.4|User.surname

    1.  Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.
    3.  Valige loendist **Atribuudi väärtus** atribuudi selle rea kuvatud väärtus.
    4.  Klõpsake nuppu **valmis**.

3.  Klõpsake nuppu **Rakenda muudatused**.

4.  Brauseri nuppu **tagasi** dialoogiboksi **Kiirkäivituse** uuesti avada.

5.  Klõpsake nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Singel sisselogimise konfigureerimine] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Singel sisselogimise konfigureerimine")

6.  Lehel **Kuidas soovite kasutajate Learningpool logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Ühekordse sisselogimise konfigureerimine")

7.  Lehel **Rakenduse URL-i konfigureerimine** **Learningpool Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Learningpool rakenduse (nt: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Rakenduse URL-i konfigureerimine")

8.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil Learningpool** oma metaandmete allalaadimiseks klõpsake **metaandmete allalaadimine**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Ühekordse sisselogimise konfigureerimine")

9.  Faili metaandmete tugimeeskonnale Learningpool edasi.

    >[AZURE.NOTE]Ühekordse sisselogimise peab olema lubatud Learningpool tugimeeskonnalt.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Learningpool sisse logida, nad peavad olema ettevalmistatud Learningpool.
  
On teil konfigureerida ettevalmistamine Learningpool kasutaja toimingu üksusi pole.  
Kasutajad peavad olema loodud Learningpool tugimeeskonnale.

>[AZURE.NOTE]Saate kasutada mis tahes muud Learningpool kasutaja konto loomise tööriistade või API-de esitatud Learningpool AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Learningpool kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Learningpool **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).