<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine bambus HR | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory bambus HR kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Õpetus: Azure'i Active Directory integreerimine bambus HR

Selle õpetuse eesmärk on integreerimine Azure ja BambooHR kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   BambooHR ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud BambooHR ühekordse sisselogimise rakendusse BambooHR ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks BambooHR lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Rakenduste integreerimise jaoks BambooHR lubamine

Selle jaotise eesmärk on liigendamine BambooHR jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Rakenduste integreerimise jaoks BambooHR lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **BambooHR**.

    ![Rakenduse Galerii] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **BambooHR**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks BambooHR oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **BambooHR** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Stsenaarium] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Stsenaarium")

2.  Lehel **Kuidas soovite kasutajate BambooHR logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **BambooHR Logi sisse URL** väljale Tippige oma URL-i kasutavad kasutajad logida BambooHR rakenduse (nt: https://company.bamboohr.com), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise BambooHR juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil BambooHR ettevõtte administraatorina.

6.  Kodulehele, tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Ühekordne sisselogimine")

    1.  Klõpsake valikut **rakendused**.
    2.  Klõpsake vasakpoolses menüüs rakendused **Ühekordse sisselogimise**.
    3.  Klõpsake **SAML ühekordse sisselogimise**.

7.  Jaotises **SAML ühekordse sisselogimise** tehke järgmist.

    ![SAML Ühekordne sisselogimine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Ühekordne sisselogimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil BambooHR** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige **SSO sisselogimise URL-i** tekstiväli.
    2.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **x.509 vastav sert** tekstiväli
    4.  Klõpsake nuppu **Salvesta**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate BambooHR sisse logida, nad peavad olema ettevalmistatud BambooHR.  
Puhul BambooHR, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige administraatorina sisse **BambooHR** saidile.

2.  Klõpsake tööriistaribal nuppu **sätted**.

    ![Seadmine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Seadmine")

3.  Klõpsake nuppu **Ülevaade**.

4.  Valige vasakpoolsel navigeerimispaanil **Turvalisus \> kasutajate**.

5.  Sisestage kehtiv AAD konto, mida soovite seotud tekstiväljad säte kasutaja nimi, parool ja meiliaadress.

6.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud BambooHR kasutaja konto loomise tööriistade või API-de esitatud BambooHR AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>BambooHR kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **BambooHR **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
