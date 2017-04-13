<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Brightspace, Desire2Learn | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Brightspace, Desire2Learn Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Õpetus: Azure'i Active Directory integreerimine Brightspace Desire2Learn järgi

Selle õpetuse eesmärk on integreerimine Azure ja Brightspace, Desire2Learn kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Mõne Brightspace, Desire2Learn ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab ühe logida sisse oma Brightspace Desire2Learn ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või kasutades [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md)rakenduse Azure AD kasutajate Brightspace Desire2Learn poolt määratud.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Brightspace, Desire2Learn lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Rakenduste integreerimise jaoks Brightspace, Desire2Learn lubamine

Selle jaotise eesmärk on liigendamine rakenduste integreerimise jaoks Brightspace, Desire2Learn lubamise kohta.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Brightspace, Desire2Learn lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Brightspace Desire2Learn järgi**.

    ![Apllication Galerii] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication Galerii")

7.  Tulemuste paanil valige **Brightspace Desire2Learn järgi**, ja klõpsake **lõpuleviimine** rakenduse lisamiseks.

    ![Brightspace Desire2Learn järgi] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace Desire2Learn järgi")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Brightspace Desire2Learn, oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Brightspace, Desire2Learn** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate poolt Desire2Learn Brightspace logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Rakenduse URL-i konfigureerimine")

    1.  **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajate sisselogimiseks oma **Brightspace Desire2Learn järgi** (nt: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise Brightspace Desire2Learn poolt veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete alla laadida**, ja salvestage metaandmeid teie arvutis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Ühekordse sisselogimise konfigureerimine")

5.  Metaandmete allalaaditud faili saata oma Brightspace Desire2Learn tugimeeskonnalt.

    >[AZURE.NOTE] Teie Brightspace Desire2Learn tugitöötajad on tegelik SSO konfigureerimine.
Saate teatise, kui tellimuse jaoks on lubatud SSO-d.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Brightspace, Desire2Learn sisse logida, ta peab olema ette valmistatud üheks Brightspace Desire2Learn järgi.  
Puhul Brightspace, Desire2Learn, Kasutajakontod vaja luua oma Brightspace Desire2Learn tugimeeskonnalt.

>[AZURE.NOTE] Saate kasutada mis tahes muud Brightspace Desire2Learn kasutaja konto loomise tööriistade või API-de esitatavate Brightspace, Desire2Learn ettevalmistamine Azure Active Directory Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Brightspace, Desire2Learn kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **, Desire2Learn Brightspace **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
