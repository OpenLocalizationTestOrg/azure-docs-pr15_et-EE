<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ShiftPlanning | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory ShiftPlanning abil!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-shiftplanning"></a>Õpetus: Azure'i Active Directory integreerimine ShiftPlanning
  
Selle õpetuse eesmärk on integreerimine Azure ja ShiftPlanning kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   ShiftPlanning ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud ShiftPlanning ühekordse sisselogimise rakendusse ShiftPlanning ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks ShiftPlanning lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-shiftplanning"></a>Rakenduste integreerimise jaoks ShiftPlanning lubamine
  
Selle jaotise eesmärk on liigendamine ShiftPlanning jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-shiftplanning-perform-the-following-steps"></a>Rakenduste integreerimise jaoks ShiftPlanning lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ShiftPlanning**.

    ![Rakenduse Galerii] (./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ShiftPlanning**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ShiftPlanning] (./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ShiftPlanning oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **ShiftPlanning** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate ShiftPlanning logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ShiftPlanning Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.shiftplanning.com/includes/saml/*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil ShiftPlanning** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil **ShiftPlanning** ettevõtte administraatorina.

6.  Klõpsake menüüs ülaosas linki **administraator**.

    ![Administraator] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Administraator")

7.  **Ühekordse sisselogimise**klõpsake **integreerimine**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Ühekordne sisselogimine")

8.  **Ühekordse sisselogimise** jaotises tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Ühekordne sisselogimine")

    1.  Valige **SAML lubatud**.
    2.  Valige **Luba parooliga sisselogimine**
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ShiftPlanning** dialoogiboksi **Remote sisselogimise URL-i** väärtus kopeerida ja seejärel kleepige **URL-i SAML väljaandja** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ShiftPlanning** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **Remote välju URL-i** tekstiväli.
    5.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **x.509 vastav sert** tekstiväli
    7.  Klõpsake nuppu **Salvesta sätted**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate ShiftPlanning sisse logida, nad peavad olema ettevalmistatud ShiftPlanning.  
Puhul ShiftPlanning, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **ShiftPlanning** ettevõtte saidi administraator.

2.  Klõpsake linki **administraator**.

    ![Administraator] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Administraator")

3.  Klõpsake **töötajate**.

    ![Töötajate] (./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Töötajate")

4.  Klõpsake jaotises **toimingud**nuppu **Lisada töötaja**.

    ![Töötajate lisamine] (./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Töötajate lisamine")

5.  **Töötajate lisamine** jaotises tehke järgmist.

    ![Töötajate salvestamine] (./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Töötajate salvestamine")

    1.  Tippige **eesnimi**, **Perekonnanimi** ja **e-posti** kehtiv AAD konto soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Salvesta töötajad**.

>[AZURE.NOTE]Saate kasutada mis tahes muud ShiftPlanning kasutaja konto loomise tööriistade või API-de esitatud ShiftPlanning AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-shiftplanning-perform-the-following-steps"></a>ShiftPlanning kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **ShiftPlanning **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).