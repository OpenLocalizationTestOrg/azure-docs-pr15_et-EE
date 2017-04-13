<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Sprinklr | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Sprinklr abil!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Õpetus: Azure'i Active Directory integreerimine Sprinklr
  
Selle õpetuse eesmärk on integreerimine Azure ja Sprinklr kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Sprinklr rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Sprinklr ühekordse sisselogimise rakendusse Sprinklr ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Sprinklr lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Rakenduste integreerimise jaoks Sprinklr lubamine
  
Selle jaotise eesmärk on liigendamine Sprinklr jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Sprinklr lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Sprinklr**.

    ![Rakenduse Galerii] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Sprinklr**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Sprinklr oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Sprinklr** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Sprinklr logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Sprinklr sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. sprinklr.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Sprinklr veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Sprinklr ettevõtte administraatorina.

6.  Minge **halduse \> sätted**.

    ![Haldus] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Haldus")

7.  Minge **haldamine partneri \> ühekordse sisselogimise** klõpsake vasakpoolsel paanil.

    ![Partneri haldamine] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Partneri haldamine")

8.  Klõpsake nuppu **+ ühekordse sisselogimise lisandmoodulid**.

    ![Ühekordse sisselogimise-tarvikud] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Ühekordse sisselogimise-tarvikud")

9.  **Ühekordse sisselogimise** lehel tehke järgmist.

    ![Ühekordse sisselogimise-tarvikud] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Ühekordse sisselogimise-tarvikud")

    1.  Väljale **nimi** tippige oma konfiguratsioon nimi (nt: *WAADSSOTest*).
    2.  Valige **lubatud**.
    3.  Valige **Kasuta uut SSO serti**.
    4.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    5.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **Identiteedi pakkuja serdi** tekstiväli,
    6.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Sprinklr** dialoogiboksi **Identiteedi pakkuja ID** väärtus kopeerida ja kleepida selle **Üksuse Id** tekstiväli.
    7.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Sprinklr** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli.
    8.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Sprinklr** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja välju URL-i** tekstiväli.
    9.  **SAML kasutaja ID tüüp**, valige **argument sisaldab kasutaja "s sprinklr.com kasutajanimi**.
    10. **SAML kasutaja ID asukoht**, valige **kasutaja-ID on teema lause elemendi nime identifikaator**.
    11. Sulgege **salvestada**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
AAD kasutajad saaksid sisse logida, peab ta ettevalmistatud juurdepääsu Sprinklr rakenduse sees.  
Selles jaotises kirjeldatakse, kuidas sees Sprinklr AAD kasutajakontode loomine.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Ettevalmistamise Sprinklr kasutaja konto, tehke järgmist.

1.  Logige oma saidile Sprinklr ettevõtte administraatorina.

2.  Minge **halduse \> sätted**.

    ![Haldus] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Haldus")

3.  Minge **hallata kliendi \> kasutajate** vasakpoolsel paanil.

    ![Sätted] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Sätted")

4.  Klõpsake nuppu **Lisa kasutaja**.

    ![Sätted] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Sätted")

5.  Klõpsake dialoogiboksis **kasutaja redigeerimiseks** tehke järgmist:

    ![Kasutaja redigeerimine] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Kasutaja redigeerimine")

    1.  **E-posti**, **eesnimi** ja **Perekonnanimi** tekstiväljad, tippige konto Azure AD kasutaja teabe soovite ette valmistada.
    2.  Valige **parooli keelatud**.
    3.  Valige soovitud **keel**.
    4.  Valige **kasutaja tüüp**.
    5.  Klõpsake nuppu **Värskenda**.

    >[AZURE.IMPORTANT] **Parooli keelatud** peab olema märgitud kasutaja identiteedi pakkuja kaudu sisse logida.

6.  Minge **rolli**ja seejärel tehke järgmist.

    ![Partneri rollid] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partneri rollid")

    1.  Valige loendist **Global** **kõik\_õiguste**.
    2.  Klõpsake nuppu **Värskenda**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Sprinklr kasutaja konto loomise tööriistade või API-de esitatud Sprinklr ettevalmistamise Azure AD Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Sprinklr kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Sprinklr **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).