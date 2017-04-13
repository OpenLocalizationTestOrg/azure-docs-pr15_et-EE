<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Workday | Microsoft Azure'i" 
    description="Saate teada, kuidas funktsiooni Workday Azure Active Directory abil lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Õpetus: Azure'i Active Directory integreerimine Workday
  
Selle õpetuse eesmärk on integreerimine Azure ja Workday kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Rentniku jaoks sisse Workday
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Workday lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutaja ettevalmistamise konfigureerimine

![Stsenaarium] (./media/active-directory-saas-workday-tutorial/IC782919.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-workday"></a>Rakenduste integreerimise jaoks Workday lubamine
  
Selle jaotise eesmärk on liigendamine Salesforce'i jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Workday lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-workday-tutorial/IC700994.png "Rakenduste")

4.  **Rakenduse Galerii**avamiseks klõpsake nuppu **Lisa An rakendus**ja seejärel klõpsake nuppu **Lisa rakenduse kasutamise oma asutuse jaoks**.

    ![Mida te soovite teha?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Mida te soovite teha?")

5.  Tippige **väljale Otsi** **Workday**.

    ![WORKDAY] (./media/active-directory-saas-workday-tutorial/IC701021.png "WORKDAY")

6.  Tulemuste paanil valige **Workday**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![WORKDAY] (./media/active-directory-saas-workday-tutorial/IC701022.png "WORKDAY")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Workday oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base – 64 encoded serdi loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Klõpsake lehel **Workday** rakenduse integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-workday-tutorial/IC782920.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida Workday** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-workday-tutorial/IC782921.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** järgmiste toimingute ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-workday-tutorial/IC782957.png "Rakenduse URL-i konfigureerimine")

    lisamine. **Logige sisse URL-i** tekstivälja, tippige URL, mida kasutatakse teie kasutajad sisse logida kasutades järgmist mustrit Workday:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  Tippige tekstiväljale **Workday vastus URL-i** Workday vasta URL, kasutades järgmist mustrit:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Vastus URL-i peab olema on alamdomeeni (nt: www, wd2, wd3, wd3-impl, wd5, wd5-impl). 
    >Kasutades umbes "*http://www.myworkday.com*" töötab, kuid "*http://myworkday.com*" ei ole. 
 
4.  Lehel **Konfigureeri ühekordse sisselogimise Workday veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-workday-tutorial/IC782922.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Workday ettevõtte administraatorina.

6.  Minge **menüü \> töölaua**.

    ![Töölaua] (./media/active-directory-saas-workday-tutorial/IC782923.png "Töölaua")

7.  Minge **konto haldamine**.

    ![Konto haldamine] (./media/active-directory-saas-workday-tutorial/IC782924.png "Konto haldamine")

8.  Minge **rentniku häälestamine – turvalisus redigeerimine**.

    ![Rentniku turvalisus redigeerimine] (./media/active-directory-saas-workday-tutorial/IC782925.png "Rentniku turvalisus redigeerimine")

9.  Tehke jaotises **Ümbersuunamine URL-ide** järgmist:

    ![Ümbersuunamise URL-id] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Ümbersuunamise URL-id")

    lisamine. Klõpsake nuppu **Lisa rida**.

    b. Tekstivälja **Login ümber suunata URL-i** ja **Mobile ümber suunata URL-i** tekstivälja, tippige **Workday rentniku URL-i** sisestatud Azure klassikaline portaali lehel **Rakenduse URL-i konfigureerimine** .
    
    c. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Workday** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i**ja seejärel kleepige **Välju ümber suunata URL-i** tekstiväli.

    d.  Tippige tekstiväljale **keskkonna** keskkonna nimi.  


    >[AZURE.NOTE] Keskkonna atribuudi väärtuseks on seotud rentniku URL-i väärtus:
    >
    >-   Kui domeeninime Workday rentniku URL-i algab impl (nt: *https://impl.workday.com/\<rentniku\>/login-saml2.htmld*), **keskkond, mis on** atribuut peab olema seatud rakendamist.
    >-   Kui domeeni nimi algab midagi muud, peate pöörduma Workday saada **keskkonna** väärtuse.

10. **SAML häälestus** jaotises tehke järgmist.

    ![SAML häälestamine] (./media/active-directory-saas-workday-tutorial/IC782926.png "SAML häälestamine")

    lisamine.  Valige **Luba SAML autentimine**.

    b.  Klõpsake nuppu **Lisa rida**.

11. SAML Identiteedipakkujad jaotises tehke järgmist.

    ![SAML Identiteedipakkujad] (./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identiteedipakkujad")

    lisamine. Tippige tekstiväljale identiteedi pakkuja nimi pakkuja nimi (nt: *SPInitiatedSSO*).

    b. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Workday** dialoogiboksi **Identiteedi pakkuja ID** väärtus kopeerida ja kleepida selle **väljaandja** tekstiväli.

    c. Valige **Luba Workday Initialted Logi välja**.

    d. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Workday** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **Välju taotlemine URL-i** tekstiväli.


    e. Klõpsake **Identiteedi pakkuja avalik võti serti**ja seejärel klõpsake nuppu **Loo**. 

    ![Loomine] (./media/active-directory-saas-workday-tutorial/IC782928.png "Loomine")

    f. Klõpsake **loomine x509 avalik võti**. 
        
    ![Loomine] (./media/active-directory-saas-workday-tutorial/IC782929.png "Loomine")


1. Jaotises **Vaade x509 avalik võti** tehke järgmist. 

    ![Vaate x509 avalik võti] (./media/active-directory-saas-workday-tutorial/IC782930.png "Vaate x509 avalik võti") 

    lisamine. Tippige **nimi** väljale teie serdi nimi (nt: *PPE\_SP*).
        
    b. Tippige tekstiväljale **Kehtiv kaudu** lubatud atribuut väärtuse serdi.
    
    c.  **Lubatud soovite** tekstivälja, tippige kehtiv serdi atribuudi väärtuseks.
        
    >[AZURE.NOTE] Saate avada on kehtiv kuupäev ja on lubatud, topeltklõpsates seda allalaaditud serdi kuupäeva. Kuupäevad on loetletud vahekaarti **üksikasjad** .

    d. Teie allalaaditud serdi **Base-64-kodeeritud** faili loomine.  

    >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    e.  Avage oma base-64-kodeeritud sertifikaat Notepadis ja seejärel kopeerige see sisu.
    
    f.  Kleepige tekstiväljale **sert** oma lõikelaua sisu.
    
    g.  Klõpsake nuppu **OK**.

12.  Tehke järgmist. 

    ![SSO konfigureerimine] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO konfigureerimine")

    lisamine.  Luba selle **x509 privaatne võti paari**.

    b.  Tippige tekstiväljale **Teenusepakkuja ID -d** **http://www.workday.com**.

    c.  Valige **Luba SP algatatud SAML autentimine**.

    d.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Workday** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige **IdP SSO teenuse URL-i** tekstiväli.
     
    e. Valige **Deflate SP algatatud autentimise taotluse**.

    f. Valige **Autentimise taotlemine allkirja meetodit** **SHA256**. 
        
    ![Autentimise taotlemine allkirja meetod] (./media/active-directory-saas-workday-tutorial/IC782932.png "Autentimise taotlemine allkirja meetod") 
 
    g. Klõpsake nuppu **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Workday veebisaidil** klõpsake nuppu **edasi**. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-workday-tutorial/IC782934.png "Konfigureerimine Ühekordne sisselogimine")

13. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Konfigureerimine Ühekordne sisselogimine")



##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Workday ettevalmistatud testkasutaja saamiseks peate Workday tugimeeskonna poole.  
Workday tugimeeskonna loob kasutaja jaoks.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Workday kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Workday **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-workday-tutorial/IC782935.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-workday-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).