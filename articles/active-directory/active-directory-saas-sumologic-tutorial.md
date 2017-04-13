<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SumoLogic | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory SumoLogic abil!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Õpetus: Azure'i Active Directory integreerimine SumoLogic
  
Selle õpetuse eesmärk on integreerimine Azure ja SumoLogic kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   SumoLogic rentniku jaoks
  
Pärast selle õpetuse, olete määranud SumoLogicwill olema võimalus ühekordse sisselogimise oma SumoLogic rakenduse Azure AD kasutajate ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks SumoLogic lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-sumologic"></a>Rakenduste integreerimise jaoks SumoLogic lubamine
  
Selle jaotise eesmärk on liigendamine SumoLogic jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Rakenduste integreerimise jaoks SumoLogic lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **sumologic**.

    ![Rakenduse Galerii] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **SumoLogic**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks SumoLogic oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus on vaja oma SumoLogictenant base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **SumoLogic** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate SumoLogic logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **SumoLogic sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. SumoLogic.com*", ja seejärel klõpsake nuppu **edasi**.

    ![URL-i konfigureerimine aoo] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "URL-i konfigureerimine aoo")

4.  Lehel **Konfigureeri ühekordse sisselogimise SumoLogic veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil SumoLogic ettevõtte administraatorina.

6.  Minge **haldamine \> turvalisus**.

    ![Haldamine] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Haldamine")

7.  Klõpsake **SAML**.

    ![Globaalne turbesätted] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globaalne turbesätted")

8.  **Valige konfiguratsiooni või looge uus** loendist valige **Azure AD**ja klõpsake nuppu **Konfigureeri**.

    ![SAML 2.0 konfigureerimine] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "SAML 2.0 konfigureerimine")

9.  Klõpsake dialoogiboksis **konfigureerimine SAML 2.0** tehke järgmist.

    ![SAML 2.0 konfigureerimine] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "SAML 2.0 konfigureerimine")

    1.  Tippige tekstiväljale **Konfiguratsiooni nimi** **Azure AD**.
    2.  Valige **silumine režiim**.
    3.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil SumoLogic** dialoogi **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **väljaandja** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil SumoLogic** dialoogi kopeerige **Autentimise taotlemine URL-i** väärtus ja seejärel kleepige **Authn taotlemine URL-i** tekstiväli.
    5.  Teie allalaaditud serdi **Base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige kogu sert **x.509 vastav sert** tekstiväli.
    7.  Valige **E-posti atribuut**, nimega **Kasutage SAML teema**.
    8.  Valige **SP algatatud sisselogimise konfigureerimine**.
    9.  Tippige väljale **Login tee** **Azure**.
    10. Klõpsake nuppu **Salvesta**.

10. Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil SumoLogic** dialoogi valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **valmis**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate SumoLogic sisse logida, ta peab olema ette valmistatud SumoLogic abil.  
Puhul SumoLogic, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **SumoLogic** rentniku.

2.  Minge **haldamine \> kasutajate**.

    ![Kasutajatele] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Kasutajatele")

3.  Klõpsake nuppu **Lisa**.

    ![Kasutajatele] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Kasutajatele")

4.  Klõpsake dialoogiboksis **Uus kasutaja** tehke järgmist.

    ![Uus kasutaja] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Uus kasutaja")

    1.  Tippige Azure AD konto, mida soovite **eesnimi**, **Perekonnanimi** ja **e-posti** tekstiväljad säte seotud teavet.
    2.  Valige roll.
    3.  **Oleku**, valige **aktiivne**.
    4.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud SumoLogic kasutaja konto loomise tööriistade või API-de esitatud SumoLogic AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>SumoLogic kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **SumoLogic** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).