<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Panopto | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Panopto abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Õpetus: Azure'i Active Directory integreerimine Panopto
  
Selle õpetuse eesmärk on integreerimine Azure ja Panopto kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Panopto rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Panopto ühekordse sisselogimise rakendusse Panopto ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Panopto lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-panopto"></a>Rakenduste integreerimise jaoks Panopto lubamine
  
Selle jaotise eesmärk on liigendamine Panopto jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Panopto lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Panopto**.

    ![Appkication Galerii] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication Galerii")

7.  Tulemuste paanil valige **Panopto**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Panopto oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Panopto** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Panopto logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Panopto sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Panopto.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil Panopto** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Panopto ettevõtte administraatorina.

6.  Klõpsake tööriistaribal vasakul, valige **süsteem**ja klõpsake **Identiteedipakkujad**.

    ![Süsteemi] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Süsteemi")

7.  Klõpsake **Lisa pakkuja**.

    ![Identiteedipakkujad] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Identiteedipakkujad")

8.  Tehke jaotises SAML pakkuja järgmist:

    ![SaaS konfigureerimine] (./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS konfigureerimine")

    1.  Valige loendist **Tüüp** **SAML20**
    2.  Tippige tekstiväljale **Eksemplari nimi** eksemplari nimi.
    3.  Tippige väljale **Sõbralik kirjeldus** sõbralik kirjeldus.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Panopto** dialoogiboksi kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **väljaandja** tekstiväli.
    5.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Panopto** dialoogiboksi kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **Põrgatama lehe URL-i** tekstiväli.
    6.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    7.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see siis tekstivälja **Avalik võti**
    8.  Klõpsake nuppu **Salvesta**.
        ![Salvestamine] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Salvestamine")

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida ettevalmistamine Panopto kasutaja toimingu üksusi pole.  
Kui mõni määratud kasutaja proovib Panopto Accessi paneeli abil sisse logida, Panopto kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud Panopto.

>[AZURE.NOTE]Saate kasutada mis tahes muud Panopto kasutaja konto loomise tööriistade või API-de esitatud Panopto ettevalmistamise Azure AD Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Panopto kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Panopto **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).