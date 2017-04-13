<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine keskse töölaua | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada keskse töölaua Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Õpetus: Azure'i Active Directory integreerimine keskse töölaud

Selle õpetuse eesmärk on integreerimine Azure ja keskse töölaua kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Keskse töölaua ühekordse sisselogimise lubatud tellimuse / keskse töölaua rentniku

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Lubada rakenduste integreerimise Kesk lauaarvuti jaoks
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-central-desktop"></a>Lubada rakenduste integreerimise Kesk lauaarvuti jaoks

Selle jaotise eesmärk on liigendamine keskse lauaarvuti jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Rakenduste integreerimise Kesk lauaarvuti jaoks lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Kesk töölaud**.

    ![Rakenduse Galerii] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Keskse töölaua**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Keskse töölaud] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Keskse töölaud")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks keskse töölauale oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma keskse töölaua rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Keskse töölaua** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate keskse töölaua logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** järgmiste toimingute ja seejärel klõpsake nuppu **edasi**. 

    -   **Kesk töölaua sisselogimise URL** väljale Tippige oma keskse töölaua rentniku URL (nt: *http://contoso.centraldesktop.com*).
    -   Tippige tekstiväljale administreerimiskeskuse töölaua vasta URL-i oma keskse töölaua AssertionConsumerService URL (nt: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Saate väärtuse Kesk töölaua metaandmeid (nt: *http://contoso.centraldesktop.com*).

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil keskse töölaua** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ning salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfigureerimine Ühekordne sisselogimine")

5.  Logige sisse oma **Keskse töölaua** rentniku.

6.  Minge lehele **sätted**, klõpsake nuppu **Täpsemalt**ja seejärel klõpsake nuppu **Ühekordse sisselogimise**.

    ![Setup - täpsemalt] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - täpsemalt")

7.  **Ühekordse sisselogimise kohta sätete** lehel tehke järgmist.

    ![Ühekordse sisselogimise sätted] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Ühekordse sisselogimise sätted")

    1.  Valige **Luba SAML v2 ühekordse sisselogimise**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil keskse töölaua** kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **SSO URL** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil keskse töölaua** kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **SSO sisselogimise URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil keskse töölaua** kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **URL-i SSO välju** tekstiväli.

8.  Jaotises **Sõnumi allkirja kinnitusmeetod** tehke järgmist.

    ![Sõnumi allkirja kinnitusmeetod] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Sõnumi allkirja kinnitusmeetod")

    1.  Valige **sert**.
    2.  Valige loendist **SSO serdi** **RSH SHA256**.
    3.  Teksti faili allalaaditud serdi loomine, tekstifaili sisu ja seejärel kleepige **SSO serdi** välja.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Valige **Kuva oma SAMLv2 sisselogimislehe link**.

9.  Klõpsake nuppu **Värskenda**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

AAD kasutajad saaksid sisse logida, ta peab olema ette valmistatud keskse töölauarakenduse abil. Selles jaotises kirjeldatakse, kuidas keskse töölauarakenduses AAD kasutajakontode loomine.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Kui soovite kasutajakontode Kesk töölauale:

1.  Logige sisse oma keskse töölaua rentniku.

2.  Minge **inimesed \> sisemiste liikmete**.

3.  Klõpsake **sisemiste liikmete lisamine**.

    ![Inimesed] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Inimesed")

4.  Tippige tekstiväljale **E-posti aadressi uute liikmete** AAD konto, mida soovite näha, ja seejärel klõpsake nuppu **edasi**.

    ![Uute liikmete e-posti aadressid] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Uute liikmete e-posti aadressid")

5.  Klõpsake nuppu **Lisa sisemise liikme(d)**.

    ![Sisemise liikme lisamiseks] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Sisemise liikme lisamiseks")

    >[AZURE.NOTE] Olete lisanud kasutajad saavad meili, mis sisaldab neile vajalikule konto aktiveerimiseks klõpsake kinnitamiseks link.

>[AZURE.NOTE] Saate kasutada mis tahes muud keskse töölaua kasutaja konto loomise tööriistade või API-de esitatud keskse töölaua kasutajakontode AAD

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Keskse töölaua kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Keskse töölaua** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
