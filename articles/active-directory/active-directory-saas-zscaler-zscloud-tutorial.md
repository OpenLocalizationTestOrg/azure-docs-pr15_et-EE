<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Zscaler ZSCloud | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Zscaler ZSCloud kasutamine!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Õpetus: Azure'i Active Directory integreerimine Zscaler ZSCloud
  
Selle õpetuse eesmärk on integreerimine Azure ja ZScaler ZSCloud kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   ZScaler ZSCloud ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, olete määranud ZScaler ZSCloud Azure AD kasutajate saab ühekordse sisselogimise ZScaler ZSCloud ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md) rakendusse
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks ZScaler ZSCloud lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Puhverserveri sätete konfigureerimine
4.  Kasutaja ettevalmistamise konfigureerimine
5.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800275.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-zscaler-zscloud"></a>Rakenduste integreerimise jaoks ZScaler ZSCloud lubamine
  
Selle jaotise eesmärk on liigendamine ZScaler ZSCloud jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-zscaler-zscloud-perform-the-following-steps"></a>Rakenduste integreerimise jaoks ZScaler ZSCloud lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ZScaler ZSCloud**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800276.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ZScaler ZSCloud**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ZScaler ZSCloud] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800277.png "ZScaler ZSCloud")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ZScaler ZSCloud oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma ZScaler ZSCloud rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **ZScaler ZSCloud** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800278.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate ZScaler ZSCloud logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800279.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ZScaler ZSCloud Logi sisse URL** väljale tippige URL, mida kasutatakse kasutajatele sisselogimise ZScaler ZSCloud rakenduse, ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800280.png "Rakenduse URL-i konfigureerimine")

    >[AZURE.NOTE] Saate avada tegelik väärtus keskkonna kaudu ZScaler ZSCloud tugimeeskonnale kui teil on vaja.

4.  Lehel **Konfigureeri ühekordse sisselogimise ZScaler ZSCloud veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800281.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil ZScaler ZSCloud ettevõtte administraatorina.

6.  Klõpsake menüü peal, **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800206.png "Haldus")

7.  Klõpsake jaotises **Halda administraatoreid ja rollid**, **kasutajate haldamine & autentimist**.

    ![Halda kasutajate ja autentimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800207.png "Halda kasutajate ja autentimine")

8.  Jaotises **Valige autentimise suvandid oma ettevõtte jaoks** tehke järgmist.

    ![Autentimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800208.png "Autentimine")

    1.  Valige **autentimise abil SAML ühekordse sisselogimise**.
    2.  Klõpsake **SAML ühekordse sisselogimise parameetrite konfigureerimine**.

9.  Lehel **Konfigureerimine SAML ühekordse sisselogimise parameetrid** dialoogiboksi järgmiste toimingute ja seejärel klõpsake nuppu **valmis**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800209.png "Ühekordne sisselogimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ZScaler ZSCloud** dialoogiboksi kopeerige **Autentimise taotlemine URL-i** väärtus ja seejärel kleepige see tekstiväli **, millele saadetakse kasutajate autentimise SAML portaali URL-i** .
    2.  Tippige tekstiväljale **atribuut, mis sisaldab kasutajanime** **NameID**.
    3.  Klõpsake oma allalaaditud serdi üleslaadimine **Zscaler pem**.
    4.  Valige **Luba SAML automaatne ettevalmistamise**.

10. Klõpsake lehel **Kasutaja autentimise konfigureerimine** dialoogiboksi tehke järgmist.

    ![Haldus] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800210.png "Haldus")

    1.  Klõpsake nuppu **Salvesta**.
    2.  Valige **Aktiveeri kohe**.

11. Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise veebisaidil ZScaler ZSCloud** lehel Valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **valmis**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800282.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-proxy-settings"></a>Puhverserveri sätete konfigureerimine

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Puhverserveri sätete konfigureerimiseks Internet Exploreris

1.  Käivitage **Internet Explorer**.

2.  Valige dialoogiboksis **Interneti-suvandid** avamiseks menüü **Tööriistad** käsk **Interneti-suvandid** .

    ![Interneti-suvandid] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC769492.png "Interneti-suvandid")

3.  Klõpsake vahekaarti **ühendused** .

    ![Ühendused] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC769493.png "Ühendused")

4.  Klõpsake nuppu **Kohtvõrgu sätted** , et avada dialoogiboks **Kohtvõrgu sätted** .

5.  Jaotises puhverserveri tehke järgmist.

    ![Puhverserver] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC769494.png "Puhverserver")

    1.  Valige suvand Kasuta puhverserverit oma Kohtvõrgu jaoks.
    2.  Tippige väljale aadress **gateway.zscalerone.net**.
    3.  Tippige tekstiväljale Port **80**.
    4.  Valige **kohalike aadresside puhul puhverserverist**.
    5.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **kohtvõrgu (LAN) sätted** .

6.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Interneti-suvandid** .

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate ZScaler ZSCloud sisse logida, ta peab olema ette valmistatud ZScaler ZSCloud abil.  
Puhul ZScaler ZSCloud, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Zscaler** rentniku.

2.  Klõpsake nuppu **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781035.png "Haldus")

3.  Klõpsake **kasutaja haldus**.

    ![Lisamine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781037.png "Lisamine")

4.  Vahekaardil **Kasutajad** , klõpsake nuppu **Lisa**.

    ![Lisamine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781037.png "Lisamine")

5.  Tehke jaotises Lisa kasutaja, toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781038.png "Kasutaja lisamine")

    1.  Tippige **kasutajanimi**, **Kuva kasutajanimi**, **parool**, **Kinnitage parool**ja valige **rühmad** ja **osakonna** kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud ZScaler ZSCloud kasutaja konto loomise tööriistade või API-de esitatud ZScaler ZSCloud AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zscaler-zscloud-perform-the-following-steps"></a>ZScaler ZSCloud kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **ZScaler ZSCloud** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800283.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).