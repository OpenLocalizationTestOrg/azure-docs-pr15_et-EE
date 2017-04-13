<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine AppDynamics | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory AppDynamics abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Õpetus: Azure'i Active Directory integreerimine AppDynamics

Selle õpetuse eesmärk on integreerimine Azure ja AppDynamics kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   AppDynamics ühekordse sisselogimise lubatud tellimuse

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud AppDynamics ühekordse sisselogimise rakendusse AppDynamics ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks AppDynamics lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Rakenduste integreerimise jaoks AppDynamics lubamine

Selle jaotise eesmärk on liigendamine AppDynamics jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Rakenduste integreerimise jaoks AppDynamics lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **AppDynamics**.

    ![Rakenduse Galerii] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **AppDynamics**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks AppDynamics oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **AppDynamics** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Ühe logima konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate AppDynamics logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Ühe logima konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **AppDynamics Logi sisse URL** väljale Tippige oma URL, mida kasutatakse kasutajatele sisselogimise AppDynamics ("*https://companyname.saas.appdynamics.com*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise AppDynamics veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Ühe logima konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil AppDynamics ettevõtte administraatorina.

6.  Ülaosas tööriistaribal nuppu **sätted**ja seejärel klõpsake nuppu **haldus**.

    ![Haldus] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Haldus")

7.  Klõpsake vahekaarti **Autentimisteenuse** .

    ![Autentimisteenuse pakkuja] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Autentimisteenuse pakkuja")

8.  **Autentimisteenuse** jaotises tehke järgmist.

    ![SAML konfigureerimine] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML konfigureerimine")

    1.  **Autentimisteenuse**, valige **SAML**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil AppDynamics** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil AppDynamics** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    4.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    5.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **serdi** tekstiväli
    6.  Klõpsake nuppu **Salvesta**.
        ![Salvestamine] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Salvestamine")

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Ühe logima konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate AppDynamics sisse logida, ta peab olema ettevalmistatud AppDynamics.  
Puhul AppDynamics, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige oma saidile AppDynamics ettevõtte administraatorina.

2.  Avage **Kasutajad**ja seejärel klõpsake nuppu **+** **Loomine kasutaja** dialoogiboksi avamiseks.

    ![Kasutajatele] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Kasutajatele")

3.  Jaotises **Kasutaja loomiseks** tehke järgmist.

    ![Kasutaja loomine] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Kasutaja loomine")

    1.  Sisestage **kasutajanimi**, **nime**, **e-posti**, **Uus parool**, **Korrake uus parool** kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud AppDynamics kasutaja konto loomise tööriistade või API-de esitatud AppDynamics ettevalmistamise Azure AD Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>AppDynamics kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **AppDynamics **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
