<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Cisco Webex | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Cisco Webex Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Õpetus: Azure'i Active Directory integreerimine Cisco Webex

Selle õpetuse eesmärk on integreerimine Azure ja Cisco Webex kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Cisco Webex rentniku jaoks

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Cisco Webex ühekordse sisselogimise rakendusse Cisco Webex ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Cisco Webex lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>Rakenduste integreerimise jaoks Cisco Webex lubamine

Selle jaotise eesmärk on liigendamine Cisco Webex jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Cisco Webex lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Cisco Webex**.

    ![Rakenduse Galerii] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Cisco Webex**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Cisco Webex oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base – 64 encoded serdi loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Cisco Webex** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida Cisco Webex** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** järgmiste toimingute ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Rakenduse URL-i konfigureerimine")

    1.  Tippige tekstiväljale **Laulda klõpsake URL-i** Cisco Webex rentniku URL-i (nt: *http://contoso.webex.com*).
    2.  Tippige tekstiväljale **Cisco Webex vastus URL-i** oma **Cisco Webex AssertionConsumerService URL** (nt: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Lehel **Konfigureeri ühekordse sisselogimise Cisco Webex veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Cisco Webex ettevõtte administraatorina.

6.  Klõpsake menüü peal, **Saidi Administreerimine**.

    ![Saidi Administreerimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Saidi Administreerimine")

7.  Klõpsake jaotises **Halda saidi** **SSO konfigureerimine**.

    ![SSO konfigureerimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO konfigureerimine")

8.  Ühendatud Web SSO konfigureerimine jaotises tehke järgmist.

    ![Ühendatud SSO konfigureerimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Ühendatud SSO konfigureerimine")

    1.  Valige loendist **Federation protokolli** **SAML 2.0**.
    2.  Teie allalaaditud serdi **Base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avage oma base-64-kodeeritud sertifikaat Notepadis ja seejärel kopeerige see sisu.
    4.  Klõpsake nuppu **Impordi SAML metaandmete**ja seejärel kleepige oma base – 64 encoded sert.
    5.  Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Cisco Webex veebisaidil** lehel **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **väljaandja SAML (IdP ID) jaoks** tekstivälja.
    6.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Cisco Webex** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ning kleepige need **Kliendi SSO teenuse sisselogimise URL-i** tekstiväli.
    7.  Valige loendist **NameID vormingus** **e-posti aadress**.
    8.  Tippige tekstiväljale **AuthnContextClassRef** **Urn: oasis: nimed: tc: SAML:2.0:ac:classes:Password**.
    9.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Cisco Webex** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **Kliendi SSO teenuse välju URL-i** tekstiväli.
    10. Klõpsake nuppu **Värskenda**.

9.  Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Cisco Webex veebisaidil** lehel Valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **valmis**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Cisco Webex sisse logida, nad peavad olema ettevalmistatud Cisco Webex.  
Cisco Webex, ettevalmistamise on käsitsi tööülesande.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Cisco Webex** rentniku.

2.  Minge **kasutajate haldamine \> Lisa kasutaja**.

    ![Kasutajate lisamine] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Kasutajate lisamine")

3.  Klõpsake jaotises Lisa kasutaja, tehke järgmist.

    ![Kasutaja lisamine] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Kasutaja lisamine")

    1.  Valige **Konto tüüp**, **Host**.
    2.  Tippige teave Azure AD olemasoleva kasutaja järgmised tekstiväljad: **eesnimi, perekonnanimi**, **kasutajanimi**, **e-posti**, **parool**, **Kinnitage parool**.
    3.  Klõpsake nuppu **Lisa**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Cisco Webex kasutaja konto loomise tööriistade või API-de osutavad Cisco Webex kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Cisco Webex kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Cisco Webex **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
