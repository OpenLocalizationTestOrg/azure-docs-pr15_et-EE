<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Coupa | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Coupa kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Õpetus: Azure'i Active Directory integreerimine Coupa

Selle õpetuse eesmärk on integreerimine Azure ja Coupa kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Coupa ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Coupa ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Coupa lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-coupa"></a>Rakenduste integreerimise jaoks Coupa lubamine

Selle jaotise eesmärk on liigendamine Coupa jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Coupa lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Coupa**.

    ![Rakenduse Galerii] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Coupa**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Coupa oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Coupa konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Logige saidile Coupa ettevõtte administraatorina.

2.  Minge **häälestamise \> turvalisus juhtelemendi**.

    ![Turbemeetmed] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Turbemeetmed")

3.  Klõpsake Coupa metaandmete faili allalaadimiseks oma arvutisse **alla laadida ja impordi SP metaandmete**.

    ![Metaandmete Coupa SP] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Metaandmete Coupa SP")

4.  Erinevate brauseriaknas, logige Azure klassikaline portaali.

5.  Klõpsake lehel **Coupa** rakenduse integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Ühekordse sisselogimise konfigureerimine")

6.  **Kuidas soovite kasutajad logida Coupa** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Ühekordse sisselogimise konfigureerimine")

7.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Rakenduse URL-i konfigureerimine")

    1.  **Logige sisse URL-i** tekstivälja, tippige URL, mida kasutatakse teie kasutajad logida Coupa rakenduse (nt: "*http://company.Coupa.com*").
    2.  Metaandmete Coupa allalaaditud faili avamiseks ja seejärel kopeerige **AssertionConsumerService index/URL-i**.
    3.  Kleepige tekstiväljale **Coupa vastus URL-i** **AssertionConsumerService index/URL-i** väärtus.
    4.  Klõpsake nuppu **edasi**.

8.  Lehel **Konfigureeri ühekordse sisselogimise Coupa juures** metaandmete faili allalaadimiseks klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis kohalikult.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Ühekordse sisselogimise konfigureerimine")

9.  Coupa ettevõtte saidil, minge **häälestamise \> turvalisus juhtelemendi**.

    ![Turbemeetmed] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Turbemeetmed")

10. Jaotises **sisselogimisel Coupa mandaadi abil** tehke järgmist.

    ![Logige sisse Coupa mandaadi abil] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Logige sisse Coupa mandaadi abil")

    1.  Valige **Logi sisse SAML**.
    2.  Klõpsake nuppu **Sirvi** Azure Active metaandmed allalaaditud faili üleslaadimiseks.
    3.  Klõpsake nuppu **Salvesta**.

11. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Coupa sisse logida, nad peab olema ettevalmistatud Coupa.  
Puhul Coupa, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **Coupa** ettevõtte administraatorina.

2.  Menüü peal, valikut **häälestamine**ja klõpsake käsku **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Kasutajatele")

3.  Klõpsake nuppu **Loo**.

    ![Kasutajate loomine] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Kasutajate loomine")

4.  Jaotise **Kasutaja loomiseks** tehke järgmist.

    ![Kasutaja üksikasjad] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Kasutaja üksikasjad")

    1.  Tippige **Login**, **eesnimi**, **Perekonnanimi**, **ühekordse sisselogimise ID**, **e-posti** atribuute soovite seotud tekstiväljad säte kehtib Azure Active Directory konto.
    2.  Klõpsake nuppu **Loo**.

    >[AZURE.NOTE] Azure Active Directory konto omanik saab e-posti konto kinnitada, kuni see muutub aktiivne link.

>[AZURE.NOTE] Saate kasutada mis tahes muud Coupa kasutaja konto loomise tööriistade või API-de osutavad Coupa kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Coupa kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Coupa **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
