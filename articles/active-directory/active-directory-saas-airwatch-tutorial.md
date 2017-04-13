<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ofort | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory ofort kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Õpetus: Azure'i Active Directory integreerimine ofort

Selle õpetuse eesmärk on integreerimine Azure ja ofort kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Ofort ühekordse sisselogimise lubatud tellimuse

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud ofort ühekordse sisselogimise rakendusse ofort ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks ofort lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Ofort] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "Ofort")
##<a name="enabling-the-application-integration-for-airwatch"></a>Rakenduste integreerimise jaoks ofort lubamine

Selle jaotise eesmärk on liigendamine ofort jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Rakenduste integreerimise jaoks ofort lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ofort**.

    ![Rakenduse Galerii] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ofort**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Ofort] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "Ofort")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ofort oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **ofort** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida ofort** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Ofort Logi sisse URL** väljale Tippige oma URL-i kasutavad kasutajad sisse logida ofort rakenduse (nt: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise ofort juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil ofort ettevõtte administraatorina.

6.  Vasakpoolsel navigeerimispaanil käsku **kontod**ja seejärel klõpsake nuppu **Administraatorid**.

    ![Administraatorid] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administraatorid")

7.  Laiendage menüü **sätted** ja valige **Kataloogiteenustega**.

    ![Sätted] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Sätted")

8.  Klõpsake vahekaarti **kasutaja** **Base DN** textfield, tippige oma domeeni nimi ja klõpsake siis nuppu **Salvesta**.

    ![Kasutaja] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Kasutaja")

9.  Klõpsake vahekaarti **Server** .

    ![Server] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. Tehke järgmist.

    ![Üleslaadimine] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Üleslaadimine")

    1.  **Directory tüüp**, valige väärtus **pole**.
    2.  Valige suvand **Kasuta SAML autentimine**.
    3.  Klõpsake allalaaditud serdi üleslaadimine **üles laadida**.

11. Jaotises **taotlemiseks** tehke järgmist.

    ![Taotlemine] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Taotlemine")

    1.  **Taotleda Köitmine tüüp**, valige **POSTITA**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ofort** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja ühekordse sisselogimise kohta URL-i** tekstiväli.
    3.  **NameID vorming**, valige **Meiliaadress**.
    4.  Klõpsake nuppu **Salvesta**.

12. Klõpsake vahekaarti **kasutaja** uuesti.

    ![Kasutaja] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Kasutaja")

13. **Atribuut** jaotises tehke järgmist.

    ![Atribuut] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Atribuut")

    1.  Tippige tekstiväljale **Objekti identifikaator** **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  Tippige väljale **kasutajanimi** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  Tippige väljale **Kuvatav nimi** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  Tippige tekstiväljale **eesnimi** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  Tippige tekstiväljale **Perekonnanimi** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  Tippige tekstiväljale **e-posti** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Klõpsake nuppu **Salvesta**.

14. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate ofort sisse logida, nad peavad olema ettevalmistatud ofort.  
Ofort, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **ofort** ettevõtte administraatorina.

2.  Vasakul navigeerimispaanil käsku **kontod**ja seejärel klõpsake nuppu **Kasutajad**.

    ![Kasutajatele] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Kasutajatele")

3.  Klõpsake **Loendivaadet** **kasutajate** menüüst, ja klõpsake **lisamine \> Lisa kasutaja**.

    ![Kasutaja lisamine] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Kasutaja lisamine")

4.  Klõpsake **lisamine / redigeerimine kasutaja** dialoogiboksis tehke järgmist.

    ![Kasutaja lisamine] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Kasutaja lisamine")

    1.  Sisestage **kasutajanimi**, **parool**, **Kinnitage parool**, **eesnimi**, **Perekonnanimi**, soovite seotud tekstiväljad säte lubatud Azure Active Directory konto **Meiliaadress** .
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud ofort kasutaja konto loomise tööriistade või API-de osutavad ofort kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Ofort kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **ofort **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
