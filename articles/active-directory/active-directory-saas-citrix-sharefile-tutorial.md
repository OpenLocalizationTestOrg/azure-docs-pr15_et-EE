<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Citrix ShareFile | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Citrix ShareFile Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Õpetus: Azure'i Active Directory integreerimine Citrix ShareFile

Selle õpetuse eesmärk on integreerimine Azure ja Citrix ShareFile kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Citrix ShareFile rentniku jaoks

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Citrix ShareFile ühekordse sisselogimise rakendusse Citrix ShareFile ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Citrix ShareFile lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Rakenduste integreerimise jaoks Citrix ShareFile lubamine

Selle jaotise eesmärk on liigendamine Citrix ShareFile jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Citrix ShareFile lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Citrix ShareFile**.

    ![Rakenduse Galerii] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Citrix ShareFile**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Citrix ShareFile oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Citrix ShareFile** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Ühekordse sisselogimise lubamiseks")

2.  Lehel **Kuidas soovite kasutajad logida Citrix ShareFile** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Citrix ShareFile Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga `https://<tenant-name>.shareFile.com`, ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Citrix ShareFile veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![ConfigureSingle sisselogimine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil **Citrix ShareFile** ettevõtte administraatorina.

6.  Klõpsake tööriistaribal ülaosas linki **administraator**.

7.  Valige vasakpoolsel navigeerimispaanil **Konfigureerimine ühekordse sisselogimise**.

    ![Konto haldamine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Konto haldamine")

8.  Klõpsake soovitud **ühekordse sisselogimise / SAML 2.0 konfiguratsiooni** dialoogiboksi lehe jaotises **Põhisätted**, tehke järgmist:

    ![Ühekordne sisselogimine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Ühekordne sisselogimine")

    1.  Klõpsake nuppu **Luba SAML**.
    2.  Kopeerige Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Citrix ShareFile** dialoogiboksi **Üksuse ID** väärtus ja seejärel kleepige see soovitud **teie IDP väljaandja / üksuse ID** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Citrix ShareFile** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **soovitud konfigureerimine ühekordse sisselogimise veebisaidil Citrix ShareFile** dialoogiboksi, kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    5.  Klõpsake nuppu **Muuda** **x.509 vastav sert** välja kõrval ja laadige Azure AD klassikaline portaalist allalaaditud serdi.
        ![Põhisätted] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Põhisätted")

9.  Klõpsake haldusportaali Citrix ShareFile **salvestada** .

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Citrix ShareFile sisse logida, ta peab olema ettevalmistatud Citrix ShareFile.  
Puhul Citrix ShareFile, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Citrix ShareFile** rentniku.

2.  Klõpsake **kasutajate haldamine \> hallata kasutajate Avaleht \> + loomine töötaja**.

    ![Töötaja loomine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Töötaja loomine")

3.  Sisestage **e-posti**, **eesnimi** ja **perekonnanimi** , kehtiv Azure AD konto, mida soovite näha.

    ![Põhiteave] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Põhiteave")

4.  Klõpsake nuppu **Lisa kasutaja**.

    >[AZURE.NOTE] AAD konto omanik saadetakse teile e-posti ja järgige oma konto kinnitada, kuni see muutub aktiivne link.

>[AZURE.NOTE] Saate kasutada mis tahes muud Citrix ShareFile kasutaja konto loomise tööriistade või API-de osutavad Citrix ShareFile AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Citrix ShareFile kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Citrix ShareFile **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
