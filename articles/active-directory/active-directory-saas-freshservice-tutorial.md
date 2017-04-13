<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine FreshService | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory FreshService abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Õpetus: Azure'i Active Directory integreerimine FreshService
  
Selle õpetuse eesmärk on integreerimine Azure ja FreshService kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   FreshService ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud FreshService ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks FreshService lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-freshservice"></a>Rakenduste integreerimise jaoks FreshService lubamine
  
Selle jaotise eesmärk on liigendamine FreshService jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Rakenduste integreerimise jaoks FreshService lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **FreshService**.

    ![Rakenduse Galerii] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **FreshService**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks FreshService oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks FreshService konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **FreshService** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate FreshService logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **FreshService Logi sisse URL** väljale Tippige oma URL-i kasutavad kasutajad logida Freshdesk rakenduse (nt: "*http://democompany.freshservice.com/*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise FreshService veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil FreshService ettevõtte administraatorina.

6.  Klõpsake menüüs ülaosas linki **administraator**.

    ![Administraator] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Administraator")

7.  **Kliendi portaalis**, klõpsake **Turvalisus**.

    ![Turvalisus] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Turvalisus")

8.  Jaotises **Turvalisus** , tehke järgmist.

    ![Ühekordse sisselogimise] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Ühekordse sisselogimise")

    1.  **Ühekordse sisselogimise OnON**aktiveerimine
    2.  Valige **SAML SSO-d**.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil FreshService** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **SAML sisselogimise URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil FreshService** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    5.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Turvalisus serdi sõrmejälje** tekstiväli.
    
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate FreshService sisse logida, nad peavad olema ettevalmistatud FreshService.  
Puhul FreshService, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **FreshService** ettevõtte saidi administraator.

2.  Klõpsake menüüs ülaosas linki **administraator**.

    ![Administraator] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Administraator")

3.  Klõpsake jaotises **Kasutajahalduse** **taotlejad**.

    ![Taotlejad] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Taotlejad")

4.  Klõpsake nuppu **Uus taotleja**.

    ![Uue taotlejad] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Uue taotlejad")

5.  Jaotises **Uus taotleja** , tehke järgmist.

    ![Uue taotleja] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Uue taotleja")

    1.  Sisestage kehtiv Azure Active Directory kontot, mida soovite **eesnimi** ja **e-posti** atribuutide ettevalmistamine seotud tekstiväljad.
    2.  Klõpsake nuppu **Salvesta**.

    >[AZURE.NOTE] Azure Active Directory konto omanik saab e-posti, sh konto kinnitada, kuni see muutub aktiivne link

>[AZURE.NOTE] Saate kasutada mis tahes muud FreshService kasutaja konto loomise tööriistade või API-de esitatud FreshService AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>FreshService kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **FreshService **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).