<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine targad | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory targad kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Õpetus: Azure'i Active Directory integreerimine targad

Selle õpetuse eesmärk on integreerimine Azure ja targad kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Nutikas rentniku jaoks

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud targad ühekordse sisselogimise taotluse saidile tark ettevõte (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks targad lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-clever-tutorial/IC798977.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-clever"></a>Rakenduste integreerimise jaoks targad lubamine

Selle jaotise eesmärk on liigendamine targad jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Rakenduste integreerimise jaoks targad lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-clever-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-clever-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-clever-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **targad**.

    ![Rakenduse Galerii] (./media/active-directory-saas-clever-tutorial/IC798978.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **targad**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Nutikas] (./media/active-directory-saas-clever-tutorial/IC798979.png "Nutikas")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks targad oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Nutikas rakenduse eeldab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![Atribuudid] (./media/active-directory-saas-clever-tutorial/IC798980.png "Atribuudid")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **targad** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clever-tutorial/IC784682.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida targad** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clever-tutorial/IC798981.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Tark Logi sisse URL** väljale tippige URL, mida kasutatakse kasutajatele sisselogimise nutika rakenduse järgi (nt: *https://clever.com/in/azsandbox*), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-clever-tutorial/IC798982.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise targad veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clever-tutorial/IC798983.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse tark ettevõte saidi administraator.

6.  Klõpsake tööriistaribal nuppu **Kiirsõnumside sisselogimine**.

    ![Kiirsõnumite sisselogimine] (./media/active-directory-saas-clever-tutorial/IC798984.png "Kiirsõnumite sisselogimine")

7.  **Kiirsõnumite** sisselogimislehel, tehke järgmist.

    ![Kiirsõnumite sisselogimine] (./media/active-directory-saas-clever-tutorial/IC798985.png "Kiirsõnumite sisselogimine")

    1.  Tippige **sisselogimise URL-i**.  

        >[AZURE.NOTE] **Sisselogimise URL-i** on kohandatud väärtus.
Nutikas tugimeeskonnale saad tegelik väärtus.

    2.  **Identiteedi süsteem**, valige **ADFS-i**.
    3.  Klõpsake nuppu **Salvesta**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-clever-tutorial/IC798986.png "Ühekordse sisselogimise konfigureerimine")

9.  Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** .

    ![Atribuudid] (./media/active-directory-saas-clever-tutorial/IC795920.png "Atribuudid")

10. Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ![SAML Turbeloa atribuudid] (./media/active-directory-saas-clever-tutorial/IC795921.png "SAML Turbeloa atribuudid")

  	|Atribuudi nimi|Atribuudi väärtus|
  	|---|---|
  	|clever.Student.Credentials.District\_kasutajanimi|User.userPrincipalName|

    1.  Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.
    3.  **Atribuudi väärtust** tekstiväli, valige selle rea atribuudi väärtust.
    4.  Klõpsake nuppu **valmis**.

11. Klõpsake nuppu **Rakenda muudatused**.

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate targad sisse logida, nad peavad olema ettevalmistatud targad.  
Targad, ettevalmistamise on käsitsi tööülesande tark tugimeeskonnale teostada.

>[AZURE.NOTE] Saate kasutada mis tahes muud tark kasutaja konto loomise tööriistade või API-de esitatud intelligentsus kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Targad kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **targad **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-clever-tutorial/IC798987.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-clever-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
