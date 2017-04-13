<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Zscaler | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Zscaler abil!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Õpetus: Azure'i Active Directory integreerimine Zscaler
  
Selle õpetuse eesmärk on integreerimine Azure ja Zscaler kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Rentniku jaoks sisse Zscaler
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Zscaler lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Puhverserveri sätete konfigureerimine
4.  Kasutaja ettevalmistamise konfigureerimine
5.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-zscaler"></a>Rakenduste integreerimise jaoks Zscaler lubamine
  
Selle jaotise eesmärk on liigendamine Zscaler jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Zscaler lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Zscaler**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Zscaler**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Zscaler oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus on vaja serdi üleslaadimine Zscaler.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Zscaler** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Ühekordse sisselogimise lubamiseks")

2.  Lehel **Kuidas soovite kasutajate Zscaler logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühe konfigureerimine sisse logida] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Ühe konfigureerimine sisse logida")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Zscaler sisselogimise URL** väljale Tippige oma sisselogimise URL-i kaudu Zscaler teil ja klõpsake nuppu **edasi**. 

    >[AZURE.NOTE] Kui te ei tea, mis teie sisselogimise URL-is on, pöörduge tugimeeskonna Zscaler.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Zscaler veebisaidil** , tehke järgmist:

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Konfigureerimine Ühekordne sisselogimine")

    1.  Klõpsake nuppu **Laadi alla serdi**ja salvestage serdi fail kohalikult **c:\\Zscaler.cer**.
    2.  Kopeerige **URL-i autentimine taotluse** teie lõikelauale.

5.  Logige sisse oma Zscaler rentniku.

6.  Klõpsake menüü peal, **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Haldus")

7.  Klõpsake jaotises **Halda administraatoreid ja rollid**, **kapi kasutajad ja autentimist**.

    ![Administraatorid ja rollide haldamine] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Administraatorid ja rollide haldamine")

8.  Jaotises **Valige autentimise suvandit oma ettevõtte jaoks** tehke järgmist.

    ![Valige autentimise suvandid] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Valige autentimise suvandid")

    1.  Valige **autentimise abil SAML ühekordse sisselogimise**.
    2.  Klõpsake **SAML ühekordse sisselogimise parameetrite konfigureerimine**.

9.  Lehel **Konfigureerimine SAML ühekordse sisselogimise parameetrid** dialoogiboksi järgmiste toimingute ja seejärel klõpsake nuppu **valmis**.

    ![Laadige üles sert] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Laadige üles sert")

    1.  Kleepige tekstiväljale **URL-i SAML portaali, millele saadetakse kasutajate autentimise** väärtus **autentimise taotluse URL-i** Azure klassikaline portaalist välja.
    2.  Tippige tekstiväljale **atribuut, mis sisaldab kasutajanime** **NameID**.
    3.  Väljale **Üleslaadimine SSL-i avaliku serdi** üleslaadimine Azure klassikaline portaalist allalaaditud serdi.
    4.  Valige **Luba SAML automaatne ettevalmistamise**.

10. Klõpsake lehel **Kasutaja autentimise konfigureerimine** dialoogiboksi tehke järgmist.

    ![Kasutaja autentimise konfigureerimine] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Kasutaja autentimise konfigureerimine")

    1.  Klõpsake nuppu **Salvesta**.
    2.  Valige **Aktiveeri kohe**.

11. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-proxy-settings"></a>Puhverserveri sätete konfigureerimine

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Puhverserveri sätete konfigureerimiseks Internet Exploreris

1.  Käivitage **Internet Explorer**.

2.  Valige dialoogiboksis **Interneti-suvandid** avamiseks menüü **Tööriistad** käsk **Interneti-suvandid** .

    ![Interneti-suvandid] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Interneti-suvandid")

3.  Klõpsake vahekaarti **ühendused** .

    ![Ühendused] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Ühendused")

4.  Klõpsake nuppu **Kohtvõrgu sätted** , et avada dialoogiboks **Kohtvõrgu sätted** .

5.  Jaotises puhverserveri tehke järgmist.

    ![Puhverserver] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Puhverserver")

    1.  Valige suvand Kasuta puhverserverit oma Kohtvõrgu jaoks.
    2.  Tippige väljale aadress **gateway.zscalertwo.net**.
    3.  Tippige tekstiväljale Port **80**.
    4.  Valige **kohalike aadresside puhul puhverserverist**.
    5.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **kohtvõrgu (LAN) sätted** .

6.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Interneti-suvandid** .

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Zscaler sisse logida, nad peab olema ettevalmistatud Zscaler.  
Puhul Zscaler, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Zscaler** rentniku.

2.  Klõpsake nuppu **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Haldus")

3.  Klõpsake **kasutaja haldus**.

    ![Kasutajate haldamine] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Kasutajate haldamine")

4.  Vahekaardil **Kasutajad** , klõpsake nuppu **Lisa**.

    ![Lisamine] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Lisamine")

5.  Tehke jaotises Lisa kasutaja, toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Kasutaja lisamine")

    1.  Tippige **kasutajanimi**, **Kuva kasutajanimi**, **parool**, **Kinnitage parool**ja seejärel valige **rühmad** ja **osakonna** kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Zscaler kasutaja konto loomise tööriista või API-de esitatud Zscaler AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Zscaler kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Zscaler** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
