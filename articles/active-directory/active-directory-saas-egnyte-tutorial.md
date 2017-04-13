<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Egnyte | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Egnyte abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Õpetus: Azure'i Active Directory integreerimine Egnyte
  
Selle õpetuse eesmärk on integreerimine Azure ja Egnyte kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Egnyte ühekordse sisselogimise lubatud tellimuse
  
Pärast selle õpetuse, olete määranud Egnyte Azure AD kasutajate saab ühekordse sisselogimise rakendusse Egnyte ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md) kaudu
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Egnyte lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-egnyte-tutorial/IC787812.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-egnyte"></a>Rakenduste integreerimise jaoks Egnyte lubamine
  
Selle jaotise eesmärk on liigendamine Egnyte jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-egnyte-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Egnyte lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-egnyte-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-egnyte-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-egnyte-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **egnyte**.

    ![Rakenduse Galerii] (./media/active-directory-saas-egnyte-tutorial/IC787813.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Egnyte**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Egnyte] (./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Egnyte oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Egnyte** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-egnyte-tutorial/IC787815.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Egnyte** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-egnyte-tutorial/IC787816.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Egnyte sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.egnyte.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-egnyte-tutorial/IC787817.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Egnyte juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-egnyte-tutorial/IC787818.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Egnyte ettevõtte administraatorina.

6.  Klõpsake nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-egnyte-tutorial/IC787819.png "Sätted")

7.  Klõpsake menüü **sätted**.

    ![Sätted] (./media/active-directory-saas-egnyte-tutorial/IC787820.png "Sätted")

8.  **Konfiguratsiooni** vahekaarti ja seejärel vahekaarti **Turvalisus**.

    ![Turvalisus] (./media/active-directory-saas-egnyte-tutorial/IC787821.png "Turvalisus")

9.  **Ühekordse sisselogimise autentimise** jaotises tehke järgmist.

    ![Ühekordse sisselogimise autentimine] (./media/active-directory-saas-egnyte-tutorial/IC787822.png "Ühekordse sisselogimise autentimine")

    1.  **Ühekordse sisselogimise autentimise**, valige **SAML 2.0**.
    2.  Valige **identiteedipakkuja**, **AzureAD**.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Egnyte** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **identiteedi pakkuja sisselogimise URL-i** tekstiväli.
    4.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Egnyte** dialoogiboksi **Üksuse ID** väärtus kopeerida ja seejärel kleepige **identiteedi pakkuja üksuse ID** tekstiväli.
    5.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **identiteedi pakkuja serdi** tekstiväli.
    7.  Nimega **vaikimisi kasutaja vastenduse**, valige **meiliaadress**.
    8.  **Kasutage domeeni kohased väljaandja väärtuse**, valige **keelatud**.
    9.  Klõpsake nuppu **Salvesta**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-egnyte-tutorial/IC787823.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Egnyte sisse logida, nad peavad olema ettevalmistatud Egnyte.  
Puhul Egnyte, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **Egnyte** Egnyte ettevõtte administraatorina.

2.  Minge **sätted \> kasutajad ja rühmad**.

3.  Klõpsake nuppu **Lisa uus kasutaja**ja seejärel valige kasutaja, mille soovite lisada.

    ![Kasutajatele] (./media/active-directory-saas-egnyte-tutorial/IC787824.png "Kasutajatele")

4.  **Uue tavakasutaja** jaotises tehke järgmist.

    ![Uue tavakasutaja] (./media/active-directory-saas-egnyte-tutorial/IC787825.png "Uue tavakasutaja")

    1.  Tippige **e-posti**, **kasutajanimi** ja muud üksikasjad lubatud Azure Active Directory konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

    >[AZURE.NOTE] Azure Active Directory konto omanik saab teate e-posti.

>[AZURE.NOTE] Saate kasutada mis tahes muud Egnyte kasutaja konto loomise tööriistade või API-de esitatud Egnyte AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-egnyte-perform-the-following-steps"></a>Egnyte kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Egnyte **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-egnyte-tutorial/IC787826.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-egnyte-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).