<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Zscaler beeta | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Zscaler beeta kasutamine!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Õpetus: Azure'i Active Directory integreerimine Zscaler beeta
  
Selle õpetuse eesmärk on integreerimine Azure ja ZScaler beeta kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   ZScaler beeta ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud ZScaler beeta ühekordse sisselogimise rakendusse ZScaler beeta ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise ZScaler beeta lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Puhverserveri sätete konfigureerimine
4.  Kasutaja ettevalmistamise konfigureerimine
5.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-zscaler-beta-tutorial/IC800223.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-zscaler-beta"></a>Rakenduste integreerimise ZScaler beeta lubamine
  
Selle jaotise eesmärk on liigendamine ZScaler beeta rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-zscaler-beta-perform-the-following-steps"></a>Rakenduste integreerimise ZScaler beeta lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-beta-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zscaler-beta-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zscaler-beta-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zscaler-beta-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ZScaler beeta**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zscaler-beta-tutorial/IC800224.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ZScaler beeta**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ZScaler ühe] (./media/active-directory-saas-zscaler-beta-tutorial/IC800216.png "ZScaler ühe")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ZScaler beeta oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma ZScaler beeta rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **ZScaler beeta** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800225.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida ZScaler beeta** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800226.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ZScaler beeta Logi sisse URL** väljale tippige URL, mida kasutatakse kasutajatele sisselogimise ZScaler beeta rakenduse, ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800227.png "Rakenduse URL-i konfigureerimine")

    >[AZURE.NOTE] Saate avada tegelik väärtus keskkonna kaudu ZScaler beeta tugimeeskonnale kui teil on vaja.

4.  Lehel **Konfigureeri ühekordse sisselogimise ZScaler beeta veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800228.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidile ZScaler beeta ettevõtte administraatorina.

6.  Klõpsake menüü peal, **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-beta-tutorial/IC800206.png "Haldus")

7.  Klõpsake jaotises **Halda administraatoreid ja rollid**, **kasutajate haldamine & autentimist**.

    ![Halda kasutajate ja autentimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800207.png "Halda kasutajate ja autentimine")

8.  Jaotises **Valige autentimise suvandid oma ettevõtte jaoks** tehke järgmist.

    ![Autentimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800208.png "Autentimine")

    1.  Valige **autentimise abil SAML ühekordse sisselogimise**.
    2.  Klõpsake **SAML ühekordse sisselogimise parameetrite konfigureerimine**.

9.  Lehel **Konfigureerimine SAML ühekordse sisselogimise parameetrid** dialoogiboksi järgmiste toimingute ja seejärel klõpsake nuppu **valmis**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800209.png "Ühekordne sisselogimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ZScaler beeta** dialoogiboksi kopeerige **Autentimise taotlemine URL-i** väärtus ja seejärel kleepige **URL-i, millele saadetakse kasutajate autentimise SAML portaali** tekstiväli.
    2.  Tippige tekstiväljale **atribuut, mis sisaldab kasutajanime** **NameID**.
    3.  Klõpsake oma allalaaditud serdi üleslaadimine **Zscaler pem**.
    4.  Valige **Luba SAML automaatne ettevalmistamise**.

10. Klõpsake lehel **Kasutaja autentimise konfigureerimine** dialoogiboksi tehke järgmist.

    ![Haldus] (./media/active-directory-saas-zscaler-beta-tutorial/IC800210.png "Haldus")

    1.  Klõpsake nuppu **Salvesta**.
    2.  Valige **Aktiveeri kohe**.

11. Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise veebisaidil ZScaler beeta** lehel Valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **valmis**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800229.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-proxy-settings"></a>Puhverserveri sätete konfigureerimine

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Puhverserveri sätete konfigureerimiseks Internet Exploreris

1.  Käivitage **Internet Explorer**.

2.  Valige dialoogiboksis **Interneti-suvandid** avamiseks menüü **Tööriistad** käsk **Interneti-suvandid** .

    ![Interneti-suvandid] (./media/active-directory-saas-zscaler-beta-tutorial/IC769492.png "Interneti-suvandid")

3.  Klõpsake vahekaarti **ühendused** .

    ![Ühendused] (./media/active-directory-saas-zscaler-beta-tutorial/IC769493.png "Ühendused")

4.  Klõpsake nuppu **Kohtvõrgu sätted** , et avada dialoogiboks **Kohtvõrgu sätted** .

5.  Jaotises puhverserveri tehke järgmist.

    ![Puhverserver] (./media/active-directory-saas-zscaler-beta-tutorial/IC769494.png "Puhverserver")

    1.  Valige suvand Kasuta puhverserverit oma Kohtvõrgu jaoks.
    2.  Tippige väljale aadress **gateway.zscalerBeta.net**.
    3.  Tippige tekstiväljale Port **80**.
    4.  Valige **kohalike aadresside puhul puhverserverist**.
    5.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **kohtvõrgu (LAN) sätted** .

6.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Interneti-suvandid** .

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate ZScaler beeta sisse logida, ta peab olema ette valmistatud ZScaler beeta.  
Puhul ZScaler beeta, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Zscaler** rentniku.

2.  Klõpsake nuppu **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-beta-tutorial/IC781035.png "Haldus")

3.  Klõpsake **kasutaja haldus**.

    ![Lisamine] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Lisamine")

4.  Vahekaardil **Kasutajad** , klõpsake nuppu **Lisa**.

    ![Lisamine] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Lisamine")

5.  Tehke jaotises Lisa kasutaja, toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-zscaler-beta-tutorial/IC781038.png "Kasutaja lisamine")

    1.  Tippige **kasutajanimi**, **Kuva kasutajanimi**, **parool**, **Kinnitage parool**ja seejärel valige **rühmad** ja **osakonna** kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud ZScaler beeta kasutaja konto loomise tööriistade või API-de esitatud ZScaler beeta kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zscaler-beta-perform-the-following-steps"></a>ZScaler beeta kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **ZScaler beeta** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zscaler-beta-tutorial/IC800230.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zscaler-beta-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).