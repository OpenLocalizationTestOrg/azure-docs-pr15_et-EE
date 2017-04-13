<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine xMatters nõudmisel | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory xMatters nõudmisel kasutamine!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Õpetus: Azure'i Active Directory integreerimine xMatters nõudmisel
  
Selle õpetuse eesmärk on integreerimine Azure ja xMatters nõudmisel kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Nõudmisel xMatters rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud xMatters nõudmisel ühekordse sisselogimise rakendusse xMatters nõudmisel ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks xMatters nõudmisel lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Rakenduste integreerimise jaoks xMatters nõudmisel lubamine
  
Selle jaotise eesmärk on liigendamine jaoks xMatters nõudmisel rakenduste integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Rakenduste integreerimise jaoks xMatters nõudmisel lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **xMatters nõudmisel**.

    ![Rakenduse Galerii] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **XMatters nõudmisel**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![xMatters nõudmisel] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters nõudmisel")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks XMatters nõudmisel oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **XMatters nõudmisel** rakenduste integreerimine Azure klassikaline portaalis.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida XMatters nõudmisel** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Konfigureerimine Ühekordne sisselogimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Rakenduse URL-i konfigureerimine")

    lisamine. Tippige tekstiväljale **XMatters nõudmisel sisselogimise URL** oma URL, kasutades järgmist mustrit:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Klõpsake nuppu **edasi**.


4.  Lehel **Konfigureeri ühekordse sisselogimise XMatters nõudmisel veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Peate serdi xMatters tugimeeskonnale edasi. Serdi tuleb üles laadida xMatters tugitöötajad enne viimistlemise ühekordse sisselogimise konfigureerimine.

    ![Ühe konfigureerimine sisse logida] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Ühe konfigureerimine sisse logida")

5.  Erinevate web brauseriaknas, logige sisse saidil XMatters nõudmisel ettevõtte administraatorina.

6.  Tööriistaribal ülaosas nuppu **administraator**ja klõpsake vasakul navigeerimisribal **Ettevõtte andmed** .

    ![Administraator] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Administraator")

7.  **SAML konfiguratsiooni** lehele, tehke järgmist.

    ![SAML konfigureerimine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML konfigureerimine")

    1.  Valige **Luba SAML**.
    2.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil XMatters nõudmisel** dialoogiboksi **Identiteedi pakkuja ID** väärtus kopeerida ja seejärel kleepige **Identiteedi pakkuja ID** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil XMatters nõudmisel** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige see **Ühekordse sisselogimise kohta URL** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil XMatters nõudmisel** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **Ühe välju URL-i** tekstiväli.
    5.  Ettevõtte üksikasjade lehe ülaosas, klõpsake nuppu **Salvesta muudatused**.
        ![Ettevõtte andmed] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Ettevõtte andmed")

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe konfigureerimine sisse logida] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Ühe konfigureerimine sisse logida")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate XMatters nõudmisel sisse logida, nad peavad olema ettevalmistatud XMatters nõudmisel.  
Puhul XMatters nõudmisel, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **XMatters nõudmisel** rentniku.

2.  Klõpsake vahekaarti **Kasutajad** .

3.  Klõpsake nuppu **Lisa kasutaja**.

    ![Kasutajatele] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Kasutajatele")

4.  Valige **aktiivne**.

5.  Tehke jaotises **Lisa kasutaja** , toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Kasutaja lisamine")

    1.  Sisestage **kasutajanimi**, **eesnimi**, **perekonnanimi**, **saidi** kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud XMatters nõudmisel kasutaja konto loomise tööriistade või API-de esitatud XMatters nõudmisel kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>XMatters nõudmisel kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **XMatters nõudmisel **rakenduste integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).