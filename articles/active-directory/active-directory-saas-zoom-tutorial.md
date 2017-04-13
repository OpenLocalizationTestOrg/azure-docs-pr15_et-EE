<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Suum | Microsoft Azure'i" 
    description="Saate teada, kuidas funktsiooni Suum Azure Active Directory abil lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Õpetus: Azure'i Active Directory integreerimine suumimine
  
Selle õpetuse eesmärk on integreerimine Azure ja Suum kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Suumi rentniku jaoks
  
Pärast selle õpetuse, olete määranud Suum Azure AD kasutajate saab ühekordse sisselogimise rakendusse Suum ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või kasutades [juurdepääsu paani tutvustus](active-directory-saas-access-panel-introduction.md)
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Suum lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-zoom"></a>Rakenduste integreerimise jaoks Suum lubamine
  
Selle jaotise eesmärk on liigendamine zoom rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Suum lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **suurendada**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **suumida**, ja klõpsake **lõpuleviimine** rakenduse lisamiseks.

    ![Suumimine] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Suumimine")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Suum oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **suurendada** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas kas soovite kasutajad logida Suum** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Suurendada sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.zoom.us*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil Suum** nuppu **Laadi alla serdi**ja salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Suum ettevõtte administraatorina.

6.  Klõpsake vahekaarti **Ühekordse sisselogimise** .

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Ühekordne sisselogimine")

7.  Klõpsake vahekaarti **Turvalisus juhtelement** ja seejärel avage **Ühekordse sisselogimise** .

8.  Ühekordse sisselogimise jaotises tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Ühekordne sisselogimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Suum** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige see **sisselogimise lehe URL-i** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Suum** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **väljunud lehe URL-i** tekstiväli.
    3.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **identiteedi pakkuja serdi** tekstiväli
    5.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Suum** dialoogiboksi **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **väljaandja** tekstiväli.
    6.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Suum sisse logida, need peab olema ettevalmistatud Suum.  
Suum ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige administraatorina **suumimine** ettevõtte saidile.

2.  Klõpsake vahekaarti **Konto haldus** ja klõpsake **Kasutajate haldamine**.

3.  Klõpsake jaotises kasutajate haldamine **kasutajate lisamine**.

    ![Kasutajate haldamine] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Kasutajate haldamine")

4.  **Kasutajate lisamine** lehel tehke järgmist.

    ![Kasutajate lisamine] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Kasutajate lisamine")

    1.  **Kasutaja tüüp**, valige **tavaline**.
    2.  Tippige tekstiväljale **e-kirju** e-posti aadressi kehtiv AAD konto, mida soovite ette valmistada.
    3.  Klõpsake nuppu **Lisa**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Suum kasutaja konto loomise tööriistu või API-de esitatud sätte AAD suumi Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Suumi kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  **Suumi **rakenduse integreerimise, klõpsake lehel **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).