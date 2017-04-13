<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine integreerimine Druva | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Druva kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Õpetus: Azure'i Active Directory integreerimine Druva integreerimine

Selle õpetuse eesmärk on integreerimine Azure ja Druva kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Druva ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Druva ühekordse sisselogimise rakendusse Druva ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Druva lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-druva-tutorial/IC795084.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-druva"></a>Rakenduste integreerimise jaoks Druva lubamine

Selle jaotise eesmärk on liigendamine Druva jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Druva lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-druva-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-druva-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-druva-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Druva**.

    ![Rakenduse Galerii] (./media/active-directory-saas-druva-tutorial/IC795085.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Druva**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Druva oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base – 64 encoded serdi faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

Rakenduse Druva loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![SAML Turbeloa atribuudid] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML Turbeloa atribuudid")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Druva** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-druva-tutorial/IC795027.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida Druva** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-druva-tutorial/IC795088.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Druva Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Druva rakenduse (nt: "*https://cloud.druva.com/home/*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-druva-tutorial/IC795089.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Druva veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-druva-tutorial/IC795090.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Druva ettevõtte administraatorina.

6.  Minge **haldamine \> sätted**.

    ![Sätted] (./media/active-directory-saas-druva-tutorial/IC795091.png "Sätted")

7.  Klõpsake dialoogiboksis ühekordse sisselogimise sätted tehke järgmist.

    ![Singl sisselogimise sätted] (./media/active-directory-saas-druva-tutorial/IC795092.png "Singl sisselogimise sätted")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Druva** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **ID pakkuja sisselogimise URL-i** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Druva** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **ID pakkuja välju URL-i** tekstiväli.
    3.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **ID pakkuja serdi** tekstiväli
    5.  Avage leht **sätted** , klõpsake nuppu **Salvesta**.

8.  Klõpsake lehel **sätted** nuppu **Luua SSO Turbeloa**.

    ![Sätted] (./media/active-directory-saas-druva-tutorial/IC795093.png "Sätted")

9.  **Ühekordse sisselogimise autentimise Turbeloa** dialoogiboksis tehke järgmist.

    ![SSO luba] (./media/active-directory-saas-druva-tutorial/IC795094.png "SSO luba")

    1.  Klõpsake nuppu **Kopeeri**.
    2.  Klõpsake nuppu **Sule**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-druva-tutorial/IC795095.png "Ühekordse sisselogimise konfigureerimine")

11. Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** .

    ![Atribuudid] (./media/active-directory-saas-druva-tutorial/IC795096.png "Atribuudid")

12. Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

  	|Atribuudi nimi|Atribuudi väärtus|
  	|---|---|
  	|insync\_auth\_Turbeloa|<*lõikelaua väärtus*>|

    1.  Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.
    3.  Tippige tekstiväljale **Atribuudi väärtus** atribuudi selle rea kuvatud väärtus.
    4.  Klõpsake nuppu **valmis**.

13. Klõpsake nuppu **Rakenda muudatused**.
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Druva sisse logida, nad peavad olema ettevalmistatud Druva.  
Puhul Druva, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **Druva** ettevõtte administraatorina.

2.  Minge **haldamine \> kasutajate**.

    ![Kasutajate haldamine] (./media/active-directory-saas-druva-tutorial/IC795097.png "Kasutajate haldamine")

3.  Klõpsake nuppu **Loo uus**.

    ![Kasutajate haldamine] (./media/active-directory-saas-druva-tutorial/IC795098.png "Kasutajate haldamine")

4.  Klõpsake dialoogiboksis Uus kasutaja loomiseks tehke järgmist.

    ![NewUser loomine] (./media/active-directory-saas-druva-tutorial/IC795099.png "NewUser loomine")

    1.  Tippige meiliaadress ja soovite seotud tekstiväljad säte kehtib Azure Active Directory kasutajakonto nimi.
    2.  Klõpsake nuppu **Loo kasutaja**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Druva kasutaja konto loomise tööriistade või API-de osutavad Druva kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Druva kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Druva **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-druva-tutorial/IC795100.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-druva-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
