<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Zscaler kahe | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Zscaler kaks Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Õpetus: Azure'i Active Directory integreerimine Zscaler kahe

Selle õpetuse eesmärk on integreerimine Azure ja ZScaler kaks kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Kahe ZScaler ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, olete määranud ZScaler kaks Azure AD kasutajate saab ühekordse sisselogimise rakendusse ZScaler kahe ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või kasutades [juurdepääsu paani tutvustus](active-directory-saas-access-panel-introduction.md)
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks on kaks ZScaler lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Puhverserveri sätete konfigureerimine
4.  Kasutaja ettevalmistamise konfigureerimine
5.  Kasutajate määramine

![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Ühekordse sisselogimise konfigureerimine")

##<a name="enabling-the-application-integration-for-zscaler-two"></a>Rakenduste integreerimise jaoks on kaks ZScaler lubamine
  
Selle jaotise eesmärk on liigendamine rakenduste integreerimise jaoks on kaks ZScaler lubamise kohta.

###<a name="to-enable-the-application-integration-for-zscaler-two-perform-the-following-steps"></a>Rakenduste integreerimise jaoks on kaks ZScaler lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ZScaler kaks**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ZScaler kaks**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ZScaler kahe] (./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "ZScaler kahe")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ZScaler kaks oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma ZScaler kaks rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis **ZScaler kaks** integreerimine lehel nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajate logige ZScaler kaks** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ZScaler kahe Logi sisse URL** väljale tippige URL, mida kasutatakse kasutajatele sisselogimise ZScaler kahe rakenduse, ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "Rakenduse URL-i konfigureerimine")

    >[AZURE.NOTE] Saate avada tegelik väärtus keskkonna kaudu ZScaler kaks tugimeeskonnale kui teil on vaja.

4.  Lehel **Konfigureeri ühekordse sisselogimise veebisaidil ZScaler kaks** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas logige sisse oma ettevõtte ZScaler kaks saidi administraatorina.

6.  Klõpsake menüü peal, **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Haldus")

7.  Klõpsake jaotises **Halda administraatoreid ja rollid**, **kasutajate haldamine & autentimist**.

    ![Halda kasutajate ja autentimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Halda kasutajate ja autentimine")

8.  Jaotises **Valige autentimise suvandid oma ettevõtte jaoks** tehke järgmist.

    ![Autentimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Autentimine")

    1.  Valige **autentimise abil SAML ühekordse sisselogimise**.
    2.  Klõpsake **SAML ühekordse sisselogimise parameetrite konfigureerimine**.

9.  Lehel **Konfigureerimine SAML ühekordse sisselogimise parameetrid** dialoogiboksi järgmiste toimingute ja seejärel klõpsake nuppu **valmis**.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Ühekordne sisselogimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ZScaler kaks** dialoogiboksi kopeerige **Autentimise taotlemine URL-i** väärtus ja seejärel kleepige **URL-i, millele saadetakse kasutajate autentimise SAML portaali** tekstiväli.
    2.  Tippige tekstiväljale **atribuut, mis sisaldab kasutajanime** **NameID**.
    3.  Klõpsake oma allalaaditud serdi üleslaadimine **Zscaler pem**.
    4.  Valige **Luba SAML automaatne ettevalmistamise**.

10. Klõpsake lehel **Kasutaja autentimise konfigureerimine** dialoogiboksi tehke järgmist.

    ![Haldus] (./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Haldus")

    1.  Klõpsake nuppu **Salvesta**.
    2.  Valige **Aktiveeri kohe**.

11. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ZScaler kaks** dialoogiboksi valige ühekordse sisselogimise konfigureerimine kinnituse ja **nuppu**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-proxy-settings"></a>Puhverserveri sätete konfigureerimine

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Puhverserveri sätete konfigureerimiseks Internet Exploreris

1.  Käivitage **Internet Explorer**.

2.  Valige dialoogiboksis **Interneti-suvandid** avamiseks menüü **Tööriistad** käsk **Interneti-suvandid** .

    ![Interneti-suvandid] (./media/active-directory-saas-zscaler-two-tutorial/IC769492.png "Interneti-suvandid")

3.  Klõpsake vahekaarti **ühendused** .

    ![Ühendused] (./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Ühendused")

4.  Klõpsake nuppu **Kohtvõrgu sätted** , et avada dialoogiboks **Kohtvõrgu sätted** .

5.  Jaotises puhverserveri tehke järgmist.

    ![Puhverserver] (./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Puhverserver")

    1.  Valige suvand Kasuta puhverserverit oma Kohtvõrgu jaoks.
    2.  Tippige väljale aadress **gateway.zscalerone.net**.
    3.  Tippige tekstiväljale Port **80**.
    4.  Valige **kohalike aadresside puhul puhverserverist**.
    5.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **kohtvõrgu (LAN) sätted** .

6.  Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Interneti-suvandid** .

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate logige ZScaler kaks, ta peab olema ette valmistatud abil ZScaler kaks.  
Kahe ZScaler puhul ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Zscaler** rentniku.

2.  Klõpsake nuppu **haldus**.

    ![Haldus] (./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Haldus")

3.  Klõpsake **kasutaja haldus**.

    ![Lisamine] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Lisamine")

4.  Vahekaardil **Kasutajad** , klõpsake nuppu **Lisa**.

    ![Lisamine] (./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Lisamine")

5.  Tehke jaotises Lisa kasutaja, toimige järgmiselt.

    ![Kasutaja lisamine] (./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Kasutaja lisamine")

    1.  Tippige **kasutajanimi**, **Kuva kasutajanimi**, **parool**, **Kinnitage parool**ja seejärel valige **rühmad** ja **osakonna** kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud ZScaler kahe kasutaja konto loomise tööriistade või API-de esitatud ZScaler kaks kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zscaler-two-perform-the-following-steps"></a>Määrata kasutajad ZScaler kaks, tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake **ZScaler kaks** integreerimine lehel **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).