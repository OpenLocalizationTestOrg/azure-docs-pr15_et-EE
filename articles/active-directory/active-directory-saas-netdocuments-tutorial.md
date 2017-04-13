<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine NetDocuments | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory NetDocuments abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Õpetus: Azure'i Active Directory integreerimine NetDocuments
  
Selle õpetuse eesmärk on integreerimine Azure ja NetDocuments kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   NetDocuments rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud NetDocuments ühekordse sisselogimise rakendusse NetDocuments ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks NetDocuments lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Rakenduste integreerimise jaoks NetDocuments lubamine
  
Selle jaotise eesmärk on liigendamine NetDocuments jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Rakenduste integreerimise jaoks NetDocuments lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **NetDocuments**.

    ![Rakenduse Galerii] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **NetDocuments**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks NetDocuments oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks NetDocuments konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **NetDocuments** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate NetDocuments logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Rakenduse URL-i konfigureerimine")

    1.  **Logige sisse URL** väljale Tippige oma URL-i kasutavad kasutajad logida NetDocuments rakenduse (nt: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  Tippige tekstiväljale **NetDocuments vastus URL-i** sama väärtuse, kui olete tippinud soovitud **Logi sisse URL-i** tekstiväli.  

        >[AZURE.NOTE]Õige väärtus **Ühendatud identiteedi** lõpus leiate dialoogiboksi (vt pilti samm 9).

    3.  Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise NetDocuments veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil NetDocuments ettevõtte administraatorina.

6.  Valige **administraator**.

7.  Klõpsake nuppu **Lisa ja Eemalda kasutajad ja rühmad**.

    ![Hoidla] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Hoidla")

8.  Klõpsake nuppu **Konfigureeri autentimise Lisavalikud**.

    ![Konfigureerimine Lisavalikud autentimine] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Konfigureerimine Lisavalikud autentimine")

9.  **Ühendatud identiteedi** dialoogiboksis tehke järgmist.

    ![Ühendatud Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Ühendatud Identitty")

    1.  **Mikroneesia identiteedi serveri tüüp**, valige **Active Directory Federation Services**.
    2.  Klõpsake nuppu **Vali fail**metaandmete allalaaditud faili üles laadida.
    3.  Klõpsake nuppu **OK**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate NetDocuments sisse logida, nad peavad olema ettevalmistatud NetDocuments.  
Puhul NetDocuments, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Laulda **NetDocuments** ettevõtte saidile sisse administraatorina.

2.  Klõpsake menüüs ülaosas linki **administraator**.

    ![Administraator] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Administraator")

3.  Klõpsake nuppu **Lisa ja Eemalda kasutajad ja rühmad**.

    ![Hoidla] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Hoidla")

4.  Tippige väljale **Meiliaadress** e-posti aadressi lubatud Azure Active Directory kontot soovite näha, ja seejärel klõpsake nuppu **Lisa kasutaja**.

    ![E-posti aadress] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-posti aadress")

    >[AZURE.NOTE]Azure Active Directory konto omanik saavad meili, mis sisaldab linki konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE]Saate kasutada mis tahes muud NetDocuments kasutaja konto loomise tööriistade või API-de esitatud NetDocuments AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>NetDocuments kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **NetDocuments **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).