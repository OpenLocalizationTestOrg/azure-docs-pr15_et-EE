<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Intacct | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory Intacct abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Õpetus: Azure'i Active Directory integreerimine Intacct
  
Selle õpetuse eesmärk on integreerimine Azure ja Intacct kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Intacct rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Intacct ühekordse sisselogimise rakendusse Intacct ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Intacct lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-intacct"></a>Rakenduste integreerimise jaoks Intacct lubamine
  
Selle jaotise eesmärk on liigendamine Intacct jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Intacct lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Intacct**.

    ![Rakenduse Galerii] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Intacct**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Intacct oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Intacct** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Intacct** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Intacct Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://Intacct.com/company*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Intacct juures** nuppu **Laadi alla serdi**ja salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Intacct ettevõtte administraatorina.

6.  Klõpsake menüü **ettevõte** ja klõpsake **Ettevõtte teave**.

    ![Ettevõtte] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Ettevõtte")

7.  Klõpsake vahekaarti **Turvalisus** ja seejärel klõpsake nuppu **Redigeeri**.

    ![Turvalisus] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Turvalisus")

8.  **Ühekordse sisselogimise (SSO)** jaotises tehke järgmist.

    ![Ühekordse sisselogimise] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Ühekordse sisselogimise")

    1.  Valige **ühekordse sisselogimise lubamiseks**.
    2.  **Identiteedi pakkuja tüüp**, valige **SAML 2.0**.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Intacct** dialoogiboksi kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **URL väljaandja** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Intacct** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ning kleepige need **Sisselogimise URL-i** tekstiväli.
    5.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.
        
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **serdi** tekstiväli
    7.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Intacct sisse logida, nad peavad olema ettevalmistatud Intacct.  
Puhul Intacct, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Intacct** rentniku.

2.  **Ettevõtte** vahekaarti ja seejärel klõpsake nuppu **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Kasutajatele")

3.  Klõpsake menüü **Lisa**

    ![Lisamine] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Lisamine")

4.  Jaotises **Kasutajateave** tehke järgmist.

    ![Kasutajateave] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Kasutajateave")

    1.  Tippige **Kasutaja ID**, **perekonnanimi**, **eesnimi**, **e-posti aadressi**, **pealkirja** ja **telefoni** soovite säte seotud tekstiväljad Azure AD konto.
    2.  Valige konto Azure AD soovite ette valmistada **administraatoriõigused** .
    3.  Klõpsake nuppu **Salvesta**.
        
        >[AZURE.NOTE] AAD konto omanik saadetakse teile e-posti ja järgige oma konto kinnitada, kuni see muutub aktiivne link.

>[AZURE.NOTE] Saate kasutada mis tahes muud Intacct kasutaja konto loomise tööriistade või API-de esitatud Intacct AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Intacct kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Intacct **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).