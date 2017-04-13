<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Zoho Mail | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Zoho e-posti kasutamine!." 
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
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Õpetus: Azure'i Active Directory integreerimine Zoho Mail
  
Selle õpetuse eesmärk on integreerimine Azure ja Zoho Mail kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Zoho Mail rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Zoho e-post ühekordse sisselogimise rakendusse Zoho Mail ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise Zoho meilisõnumite lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Rakenduste integreerimise Zoho meilisõnumite lubamine
  
Selle jaotise eesmärk on liigendamine Zoho meili jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Rakenduste integreerimise Zoho meilisõnumite lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Zoho Mail**.

    ![Rakenduse Galerii] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Zoho Mail**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Zoho Mail] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Zoho e-post oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Zoho e-posti** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida Zoho Mail** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Rakenduse URL-i konfigureerimine")

    lisamine. Tippige tekstiväljale **Zoho Mail Logi sisse URL** oma URL, kasutades järgmist mustrit:`http://<company name>.ZohoMail.com`

    b. Klõpsake nuppu **edasi**.


4.  Lehel **Konfigureeri ühekordse sisselogimise Zoho meilisõnumitest** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas logige saidile Zoho Mail ettevõtte administraatorina.

6.  Avage **Juhtpaneel**.

    ![Juhtpaneel] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Juhtpaneel")

7.  Klõpsake vahekaarti **SAML autentimine** .

    ![SAML autentimine] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML autentimine")

8.  **SAML autentimine üksikasjad** jaotises tehke järgmist.

    ![SAML autentimine üksikasjad] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML autentimine üksikasjad")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Zoho meilisõnumitest** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Zoho meilisõnumitest** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Zoho meilisõnumitest** dialoogiboksi **Muuda parooli URL-i** väärtus kopeerida ja seejärel kleepige **URL-i Muuda parooli** tekstiväli.
    4.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    5.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **PublicKey** tekstiväli.
    6.  Kui **algoritmi**, valige **RSA**.
    7.  Klõpsake nuppu **OK**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate logige Zoho Mail, ta peab olema ette valmistatud Zoho meilisõnumisse.  
Zoho e-posti ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **Zoho Mail** ettevõtte administraatorina.

2.  Minge **Juhtpaneel \> meili- ja dokumentide**.

3.  Minge **kasutaja üksikasjad \> Lisa kasutaja**.

    ![Kasutaja lisamine] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Kasutaja lisamine")

4.  Klõpsake dialoogiboksis **kasutajate lisamine** tehke järgmist.

    ![Kasutaja lisamine] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Kasutaja lisamine")

    1.  Tippige **eesnimi**, **Perekonnanimi**, **E-posti ID**, soovite seotud tekstiväljad säte lubatud Azure Active Directory konto **parool** .
    2.  Klõpsake nuppu **OK**.  

        >[AZURE.NOTE] Azure Active Directory konto omanik saab e-posti konto kinnitada, kuni see muutub aktiivne link.

>[AZURE.NOTE] Saate kasutada mis tahes muud Zoho Mail kasutaja konto loomise tööriistade või API-de osutavad Zoho e-posti AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Kasutajate määramine Zoho Mail, tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Zoho e-posti **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).