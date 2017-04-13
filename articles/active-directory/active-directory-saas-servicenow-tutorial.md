<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ServiceNow ja ServiceNow Express | Microsoft Azure'i"
    description="Siit saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja ServiceNow ServiceNow Express vahel."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Õpetus: Azure'i Active Directory integreerimine ServiceNow ja ServiceNow Express.


Selles õppetükis saate teada, kuidas ServiceNow ja ServiceNow Express integreerimine Azure Active Directory (Azure AD).

ServiceNow ja ServiceNow Express integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs ServiceNow ja ServiceNow Express
- Saate lubada kasutajatele automaatselt saada allkirjastatud-on ServiceNow ja ServiceNow Express (ühekordse sisselogimise) koos oma Azure AD kontod
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

ServiceNow ja ServiceNow Express Azure AD integreerimise konfigureerimiseks vajate järgmist:

- Tellimuse Azure AD
- ServiceNow, eksemplari või rentnikus, ServiceNow Calgary versioon või uuem versioon
- ServiceNow Express, näiteks ServiceNow Express Helsingi versioon või uuem versioon
- ServiceNow rentniku peab olema lubatud [Mitme pakkuja ühekordse sisselogimise kohta lisandmoodul](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) . Seda saab teha esitades teenusetaotlus veebisaidil <https://hi.service-now.com/> 


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. ServiceNow lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise ServiceNow või ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow lisamine galeriist
ServiceNow või ServiceNow Express integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada ServiceNow galeriist hallatavate SaaS rakenduste loendisse. 

**Galeriist ServiceNow lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **ServiceNow**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. Tulemuste paanil valige **ServiceNow**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises saate konfigureerida ja Azure AD ühekordse sisselogimise ServiceNow või ServiceNow Express põhinev testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, mis ServiceNow töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud ServiceNow tuleb luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** ServiceNow väärtus **kasutaja nimi** väärtus. Azure AD ühekordse sisselogimise koos ServiceNow testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise jaoks ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Seadistamine Azure AD ühekordse sisselogimise ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** -, et kasutajad saaksid seda funktsiooni kasutada.
3. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne ServiceNow testida](#creating-a-servicenow-test-user)** - olema töölauafunktsioonid Britta Simon ServiceNow Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
6. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

> [AZURE.NOTE] Kui soovite konfigureerida ServiceNow argument etappi 2. Samuti, kui soovite konfigureerida ServiceNow Express argument etappi 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>ServiceNow Azure AD ühekordse sisselogimise konfigureerimine

1.  Azure AD klassikaline portaalis lehel **ServiceNow** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate ServiceNow logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigureerimine Ühekordne sisselogimine")

3.  Klõpsake lehel **Rakenduse sätete konfigureerimine** tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Rakenduse URL-i konfigureerimine")

    lisamine. **ServiceNow Logi sisse URL** väljale Tippige oma URL, mida kasutab sisselogimise pärast mustri ServiceNow rakenduse kasutajatele: `https://<instance-name>.service-now.com`.

    b. Tippige tekstiväljale **identifikaator** oma URL-i kasutavad kasutajad sisselogimise ServiceNow rakenduse piirangutega: `https://<instance-name>.service-now.com`.

    c. Klõpsake nuppu **Järgmine**

4.  On Azure AD automaatselt ServiceNow SAML autentimise konfigureerimine, sisestage ServiceNow eksemplari nimi, administraatori kasutajanime ja administraatori parooli **Automaatne konfigureerimine ühekordse sisselogimise** vorm ja klõpsake nuppu *Konfigureeri*. Pange tähele, et administraatori kasutajanime esitatud peab olema määratud **security_admin** roll ServiceNow nii töötada. Muidu ServiceNow kasutamiseks Azure AD SAML identiteedipakkuja juures käsitsi konfigureerimiseks nuppu **käsitsi konfigureerimine taotlus ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi** ja tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Rakenduse URL-i konfigureerimine")



5.  Klõpsake lehel **konfigureerimine ühekordse sisselogimise veebisaidil ServiceNow** Salvesta serdi fail kohalikku arvutisse **allalaadimine sert**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigureerimine Ühekordne sisselogimine")

1. Sisselogimise ServiceNow rakenduse administraatorina.
2. Järgmised juhised _seos - mitme pakkuja ühekordse sisselogimise Installer_ lisandmooduli aktiveerimiseks tehke järgmist.

    lisamine. Vasakus servas asuval navigeerimispaanil liikuge jaotisse **Süsteemi määratlemine** ja seejärel klõpsake nuppu **lisandmoodulid**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Lisandmooduli aktiveerimine")
    
    b. _Seos - mitme pakkuja ühekordse sisselogimise Installer_otsing.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Lisandmooduli aktiveerimine")

    c. Valige soovitud lisandmoodul. Servas nuppu ja valige **Aktiveeri/uuendada**.
    
    d. Klõpsake nuppu **Aktiveeri** .

2. Klõpsake vasakus servas asuval navigeerimispaanil nuppu **Atribuudid**.  

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Rakenduse URL-i konfigureerimine")


3. **Mitme pakkuja SSO atribuutide** dialoogiboksis tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Rakenduse URL-i konfigureerimine")

    lisamine. Kui **lubada mitme pakkuja SSO-d**, valige **Jah**.

    b. Nimega **silumine logimine, kas teil on mitu pakkuja SSO integreerimise lubamine**, valige **Jah**.

    c. Tippige tekstiväljale **kasutaja välja tabeli, mis...** **user_name**.

    d. Klõpsake nuppu **Salvesta**.



1. Klõpsake vasakul navigeerimispaanil **x509 sertifikaatide**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Konfigureerimine Ühekordne sisselogimine")


1. Klõpsake dialoogiboksis **X.509 serdid** nuppu **Uus**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Konfigureerimine Ühekordne sisselogimine")


1. Klõpsake dialoogiboksis **X.509 serdid** tehke järgmist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigureerimine Ühekordne sisselogimine")

    lisamine. Klõpsake nuppu **Uus**.

    b. Väljale **nimi** tippige oma konfiguratsioon nimi (nt: **TestSAML2.0**).

    c. Valige **aktiivne**.

    d. Valige **Vorming**, **PEM**.

    e. **Tüüp**, valige **Poe Cert usaldada**.
    
    f. Avage oma Base64 kodeeritud serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **PEM serdi** tekstiväli.

    g. Klõpsake nuppu **Värskenda**.


1. Klõpsake vasakul navigeerimispaanil **Identiteedipakkujad**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Konfigureerimine Ühekordne sisselogimine")

1. Klõpsake dialoogiboksis **Identiteedipakkujad** **Uus**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Konfigureerimine Ühekordne sisselogimine")


1. Klõpsake dialoogiboksis **Identiteedipakkujad** **SAML2 Update1?**:

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Konfigureerimine Ühekordne sisselogimine")


1. Klõpsake dialoogiboksis Atribuudid SAML2 Update1 tehke järgmist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Konfigureerimine Ühekordne sisselogimine")


    lisamine. väljale **nimi** tippige oma konfiguratsioon nimi (nt: **SAML 2.0**).

    b. **Kasutaja väli** tekstivälja, tippige **e-posti** või **user_id**, sõltuvalt sellest, millist välja abil tuvastada ServiceNow juurutamise kasutajad. 
    
    > [AZURE.NOTE] Saate configue Azure AD eraldavad minnes kordumatuks rakenduses SAML luba Azure AD kasutaja ID (kasutajanimi) või e-posti aadress on **ServiceNow > Atribuudid > ühekordse sisselogimise** Azure klassikaline portaali ja vastendus soovitud välja atribuudi **nameidentifier** lahtrisse. Azure AD (nt kasutaja turvasubjektinimi) talletatud valitud atribuudi väärtus peab vastama väärtus, mis on talletatud ServiceNow sisestatud välja (nt user_id)

    c. Klassikaline portaalis Azure AD **Identiteedi pakkuja ID** väärtus kopeerida ja seejärel kleepige **URL-i identiteedi pakkuja** tekstiväli.

    d. Azure AD klassikaline portaalis, kopeerige **Autentimise taotlemine URL-i** väärtus ja seejärel kleepige see **identiteedipakkuja juures AuthnRequest** tekstiväli.

    e. Azure AD klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige see **identiteedipakkuja juures SingleLogoutRequest** tekstiväli.

    f. Tippige tekstiväljale **ServiceNow kodulehele** URL-i avalehe ServiceNow eksemplari.

    > [AZURE.NOTE] ServiceNow eksemplari kodulehele on literaalide oma **ServieNow rentniku URL-i** ja **/navpage.do** (nt:`https://fabrikam.service-now.com/navpage.do`).
 

    g. Klõpsake soovitud **üksuse ID / väljaandja** tekstivälja, tippige oma ServiceNow rentniku URL.

    h. Tippige tekstiväljale **Sihtrühma URL-i** oma ServiceNow rentniku URL. 

    Ma. Tippige tekstiväljale **Protokoll siduv ka IDP SingleLogoutRequest** **urn: oasis: nimed: tc: SAML:2.0:bindings:HTTP-ümber suunata**.

    j. Tippige tekstiväljale NameID poliitika **urn: oasis: nimed: tc: SAML:1.1:nameid-vorming: määramata**.

    k. Tühjendage ruut **on AuthnContextClass loomine**.

    l. Tippige **AuthnContextClassRef meetod** `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. See on vaja ainult siis, kui teil on ainult pilvepõhise ettevõte. Kui kasutate eeldusel ADFS-i või MFA jaoks autentimine, siis peaksite konfigureerima selle väärtuse. 

    m. Tippige tekstiväljale **Kell Skew** **60**.

    n. Nagu **Ühekordse sisselogimise kohta skripti**, valige **MultiSSO_SAML2_Update1**.

    o. Kui **x509 serdi**, valige eelmises etapis loodud sert.

    p. Klõpsake **esitada**. 



6. Azure AD klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigureerimine Ühekordne sisselogimine")

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.
 
    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigureerimine Ühekordne sisselogimine")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>ServiceNow Express Azure AD ühekordse sisselogimise konfigureerimine

1.  Azure AD klassikaline portaalis lehel **ServiceNow** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate ServiceNow logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigureerimine Ühekordne sisselogimine")

3.  Klõpsake lehel **Rakenduse sätete konfigureerimine** tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Rakenduse URL-i konfigureerimine")

    lisamine. **ServiceNow Logi sisse URL** väljale Tippige oma URL, mida kasutab sisselogimise pärast mustri ServiceNow rakenduse kasutajatele: `https://<instance-name>.service-now.com`.

    b. **Väljaandja URL** väljale Tippige oma URL-i kasutavad kasutajad sisselogimise ServiceNow rakenduse pärast mustri `https://<instance-name>.service-now.com`.

    c. Klõpsake nuppu **Järgmine**

4.  Klõpsake **käsitsi konfigureerimine taotlus ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi** ja tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Rakenduse URL-i konfigureerimine")

5.  Lehel **Konfigureeri ühekordse sisselogimise ServiceNow juures** nuppu **Laadi alla serdi**, serdi faili kohapeal arvutisse salvestada ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigureerimine Ühekordne sisselogimine")

6. Sisselogimise ServiceNow Express rakenduse administraatorina.

7. Klõpsake vasakul navigeerimispaanil **Ühekordse sisselogimise**.  

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Rakenduse URL-i konfigureerimine")


8. **Ühekordse sisselogimise** dialoogiboksi, klõpsake paremas ülanurgas ikooni konfigureerimine ja määrake järgmised atribuudid.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Rakenduse URL-i konfigureerimine")

    lisamine. Lülitab **Luba mitme pakkuja SSO** paremale.

    b. Tavalise **silumine mitme pakkuja SSO integreerimiseks logimise lubamine** paremale.

    c. Tippige tekstiväljale **kasutaja välja tabeli, mis...** **user_name**.


9. **Ühekordse sisselogimise** dialoogiboksi, klõpsake nuppu **Lisa uus sert**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Konfigureerimine Ühekordne sisselogimine")



10. Klõpsake dialoogiboksis **X.509 serdid** tehke järgmist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigureerimine Ühekordne sisselogimine")

    lisamine. Väljale **nimi** tippige oma konfiguratsioon nimi (nt: **TestSAML2.0**).

    b. Valige **aktiivne**.

    c. Valige **Vorming**, **PEM**.

    d. **Tüüp**, valige **Poe Cert usaldada**.

    e. Teie allalaaditud serdi Base64 kodeeritud faili loomine.
   
    > [AZURE.NOTE] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Avage oma Base64 kodeeritud serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **PEM serdi** tekstiväli.

    g. Klõpsake nuppu **Värskenda**.


11. **Ühekordse sisselogimise** dialoogiboksi, klõpsake nuppu **Lisa uus IdP**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Konfigureerimine Ühekordne sisselogimine")


12. **Lisada uue identiteedipakkuja** dialoogiboksis jaotises **Konfigureerimine identiteedipakkuja**, tehke järgmist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Konfigureerimine Ühekordne sisselogimine")


    lisamine. Väljale **nimi** tippige oma konfiguratsioon nimi (nt: **SAML 2.0**).

    b. Klassikaline portaalis Azure AD **Identiteedi pakkuja ID** väärtus kopeerida ja seejärel kleepige **URL-i identiteedi pakkuja** tekstiväli.

    c. Azure AD klassikaline portaalis, kopeerige **Autentimise taotlemine URL-i** väärtus ja seejärel kleepige see **identiteedipakkuja juures AuthnRequest** tekstiväli.

    d. Azure AD klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige see **identiteedipakkuja juures SingleLogoutRequest** tekstiväli.

    e. **Identiteedi pakkuja sert**, valige eelmises etapis loodud sert.


13. Klõpsake nuppu **Täpsemad sätted**ja jaotises **Täiendavad identiteedi pakkuja atribuudid**, tehke järgmist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Konfigureerimine Ühekordne sisselogimine")

    lisamine. Tippige tekstiväljale **Protokoll siduv ka IDP SingleLogoutRequest** **urn: oasis: nimed: tc: SAML:2.0:bindings:HTTP-ümber suunata**.

    b. Tippige tekstiväljale **NameID poliitika** **urn: oasis: nimed: tc: SAML:1.1:nameid-vorming: määramata**.    

    c. Tippige **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password** **AuthnContextClassRef meetod**.
    
    d. Tühjendage ruut **on AuthnContextClass loomine**.

14. Klõpsake jaotises **Täiendavad teenuse pakkuja atribuudid**, tehke järgmist:

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Konfigureerimine Ühekordne sisselogimine")

    lisamine. Tippige tekstiväljale **ServiceNow kodulehele** URL-i avalehe ServiceNow eksemplari.

    > [AZURE.NOTE] ServiceNow eksemplari kodulehele on literaalide oma **ServieNow rentniku URL-i** ja **/navpage.do** (nt: `https://fabrikam.service-now.com/navpage.do`).

    b. Klõpsake soovitud **üksuse ID / väljaandja** tekstivälja, tippige oma ServiceNow rentniku URL.

    c. Tippige tekstiväljale **Sihtrühma URI** oma ServiceNow rentniku URL. 

    d. Tippige tekstiväljale **Kell Skew** **60**.

    e. **Kasutaja väli** tekstivälja, tippige **e-posti** või **user_id**, sõltuvalt sellest, millist välja abil tuvastada ServiceNow juurutamise kasutajad.
    
    > [AZURE.NOTE] Saate configue Azure AD eraldavad minnes kordumatuks rakenduses SAML luba Azure AD kasutaja ID (kasutajanimi) või e-posti aadress on **ServiceNow > Atribuudid > ühekordse sisselogimise** Azure klassikaline portaali ja vastendus soovitud välja atribuudi **nameidentifier** lahtrisse. Azure AD (nt kasutaja turvasubjektinimi) talletatud valitud atribuudi väärtus peab vastama väärtus, mis on talletatud ServiceNow sisestatud välja (nt user_id)

    f. Klõpsake nuppu **Salvesta**. 


15. Azure AD klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigureerimine Ühekordne sisselogimine")

16. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.
 
    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutaja ettevalmistamine Active Directory kasutajakontosid, et ServiceNow lubamise kohta.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1. Klõpsake Azure klassikaline haldusportaali, lehel **ServiceNow** rakenduse integreerimise **konfigureerimine kasutaja ettevalmistamise**. 

    ![Kasutajate ettevalmistamine] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Kasutajate ettevalmistamine")


2. Sisestage lehel **Sisestage mandaat ServiceNow lubamiseks automaatse kasutaja ettevalmistamise** konfiguratsiooni järgmisi sätteid: kasutaja ettevalmistamise konfigureerimine 

     lisamine. Tippige tekstiväljale **ServiceNow eksemplari nimi** ServiceNow eksemplari nimi.

     b. Tippige tekstiväljale **ServiceNow administraatori kasutajanime** ServiceNow administraatori konto nimi.

     c. Tippige tekstiväljale **ServiceNow administraatori parooli** selle konto parool.

     d. Klõpsake kinnitamaks, et teie konfiguratsiooni **valideerimiseks** .

     e. Klõpsake **järgmist** lehe avamiseks nuppu **edasi** .

     f. Kui soovite ettevalmistamise kõigile kasutajatele see rakendus, valige "**ettevalmistamise automaatselt selle rakenduse kataloogi kõik kasutajakontod**". 

    ![Järgmised sammud] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Järgmised sammud")

     g. Klõpsake lehel **järgmised toimingud** **lõpuleviimine** salvestada oma konfiguratsioon.

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   


### <a name="creating-a-servicenow-test-user"></a>ServiceNow testkasutaja loomine

Selles jaotises nimega Britta Simon ServiceNow kasutaja loomine. Selles jaotises nimega Britta Simon ServiceNow kasutaja loomine. Kui te ei tea, kuidas lisada kasutaja ServiceNow või ServiceNow Express konto, pöörduge ServiceNow toe poole.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon ServiceNow tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**ServiceNow Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **ServiceNow**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kõik kasutajad **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.

Kui klõpsate paanil Accessi paani ServiceNow, teil peaks saada automaatselt allkirjastatud-on ServiceNow rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
