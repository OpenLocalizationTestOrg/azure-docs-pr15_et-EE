<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Cherwell | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Cherwell abil!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Õpetus: Azure'i Active Directory integreerimine Cherwell

Selle õpetuse eesmärk on integreerimine Azure ja Cherwell kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Cherwell ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Cherwell ühekordse sisselogimise rakendusse Cherwell ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Cherwell lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-cherwell"></a>Rakenduste integreerimise jaoks Cherwell lubamine

Selle jaotise eesmärk on liigendamine Cherwell jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Cherwell lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  Tulemuste paanil valige **Cherwell**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Cherwell oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Cherwell** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Cherwell** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Rakenduse URL-i konfigureerimine")

    lisamine.  **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajate sisselogimiseks oma **Cherwell** (nt: *https://\<ettevõtte nime\>.cherwellondemand.com/cherwellclient*).

    b.  Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise Cherwell veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Ühekordse sisselogimise konfigureerimine")

    lisamine.  Klõpsake nuppu **serdi allalaadimine**ja seejärel salvestage serdi kohalikult teie arvutis.

    b.  Kopeerige **URL-i identiteedi pakkuja**.

    c.  **Ühekordse sisselogimise teenuse URL-i**kopeerimine

    d.  Klõpsake nuppu **edasi**.

5.  Esitage allalaaditud serdi, **Identiteedi pakkuja URL-i** ja **Ühekordse sisselogimise teenuse URL-i** Cherwell tugimeeskonnale.

    >[AZURE.NOTE] Cherwell tugimeeskonnale on tegelik SSO konfigureerimine.
Saate teatise, kui tellimuse jaoks on lubatud SSO-d.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Cherwell sisse logida, nad peavad olema ettevalmistatud Cherwell.  
Puhul Cherwell, Kasutajakontod vaja luua oma Cherwell tugimeeskonna poole.

>[AZURE.NOTE] Saate kasutada mis tahes muud Cherwell kasutaja konto loomise tööriistade või API-de esitatud Cherwell ettevalmistamine Azure Active Directory Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Cherwell kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Cherwell** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
