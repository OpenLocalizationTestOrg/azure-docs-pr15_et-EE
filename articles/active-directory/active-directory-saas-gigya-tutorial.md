<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Gigya | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Gigya abil!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Õpetus: Azure'i Active Directory integreerimine Gigya
  
Selle õpetuse eesmärk on integreerimine Azure ja Gigya kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Gigya ühekordse sisselogimise kohta lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Gigya ühekordse sisselogimise rakendusse Gigya ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Gigya lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Ühekordse sisselogimise konfigureerimine")
##<a name="enabling-the-application-integration-for-gigya"></a>Rakenduste integreerimise jaoks Gigya lubamine
  
Selle jaotise eesmärk on liigendamine Gigya jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Gigya lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Gigya**.

    ![Rakenduse Galerii] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Gigya**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Gigya oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Gigya** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Gigya** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Gigya Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.gigya.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Gigya juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Gigya ettevõtte administraatorina.

6.  Minge **sätted \> SAML Login**, ja seejärel klõpsake nuppu **Lisa** .

    ![SAML sisselogimine] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML sisselogimine")

7.  **SAML Login** jaotises tehke järgmist.

    ![SAML konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML konfigureerimine")

    1.  Tippige **nimi** rühmitusaluse nimi oma konfiguratsioon.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Gigya** dialoogiboksi kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **väljaandja** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Gigya** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige see **Ühekordse sisselogimise teenuse URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Gigya** dialoogiboksi kopeerige **Nimevormingut identifikaator** väärtus ja seejärel kleepige see **ID nimevormingut** tekstiväli.
    5.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.
        
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see siis tekstivälja **x.509 vastav sert** .
    7.  Klõpsake nuppu **Salvesta sätted**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Gigya sisse logida, nad peavad olema ettevalmistatud Gigya.  
Puhul Gigya, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Gigya** ettevõtte saidi administraator.

2.  Minge **administraator \> kasutajate haldamine**, ja seejärel klõpsake nuppu **Kutsu kasutajad**.

    ![Kasutajate haldamine] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Kasutajate haldamine")

3.  Klõpsake dialoogiboksis kutsuda kasutajad teha järgmist:

    ![Kasutajate kutsumine] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Kasutajate kutsumine")

    1.  Tippige tekstiväljale **e-posti** meilipseudonüümi lubatud Azure Active Directory konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Kutsu kasutaja**.
    
        >[AZURE.NOTE] Azure Active Directory konto omanik saavad meili, mis sisaldab linki konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud Gigya kasutaja konto loomise tööriistade või API-de esitatud Gigya AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Gigya kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Gigya **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).