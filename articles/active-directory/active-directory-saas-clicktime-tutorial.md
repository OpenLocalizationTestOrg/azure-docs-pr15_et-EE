<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ClickTime | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory ClickTime abil!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Õpetus: Azure'i Active Directory integreerimine ClickTime

Selles õppetükis saate teada, kuidas ClickTime integreerimine Azure Active Directory (Azure AD).

ClickTime integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs ClickTime
- Saate lubada automaatselt saada allkirjastatud-on ClickTime (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD ClickTime, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne ClickTime ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. ClickTime lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise

##<a name="adding-clicktime-from-the-gallery"></a>ClickTime lisamine galeriist

Selle jaotise eesmärk on liigendamine ClickTime jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Rakenduste integreerimise jaoks ClickTime lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ClickTime**.

    ![Rakenduse Galerii] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **ClickTime**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise ClickTime nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis ClickTime töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud ClickTime tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** ClickTime väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos ClickTime testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne ClickTime testida](#creating-a-clicktime-test-user)** - olema töölauafunktsioonid Britta Simon ClickTime Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ClickTime oma konto abil SAML protokolli federation Azure AD lubamise kohta.  


>[AZURE.IMPORTANT] Selleks et oma ClickTime rentniku ühekordse sisselogimise konfigureerimiseks, peate pöörduma esmalt ClickTime tehnilise toe saamiseks see funktsioon on sisse lülitatud.

**Konfigureerida Azure AD ühekordse sisselogimise ClickTime, tehke järgmist.**

1.  Azure'i klassikaline portaalis lehel **ClickTime** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Ühekordse sisselogimise lubamiseks")

2.  Lehel **Kuidas soovite kasutajate ClickTime logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Konfigureerimine Ühekordne sisselogimine")

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    lisamine. Tippige tekstiväljale **IdentifierL** URL, kasutades järgmist mustrit: **https://app.clicktime.com/sp/**
    
    b. Tippige tekstiväljale **Vastus URL-i** URL, kasutades järgmist mustrit: **https://app.clicktime.com/Login/**

    c. Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise ClickTime veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Konfigureerimine Ühekordne sisselogimine")

4.  Erinevate web brauseriaknas, logige sisse saidil ClickTime ettevõtte administraatorina.

5.  Ülaosas tööriistaribal käsku **eelistused**ja seejärel klõpsake nuppu **Turvasätted**.

6.  **Ühekordse sisselogimise eelistused** konfigureerimine jaotises tehke järgmist.

    ![Turbesätted] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Turbesätted")

    lisamine.  Valige **Luba** sisselogimine ühekordse sisselogimise (SSO) kasutades **Azure AD**.
    
    b.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil ClickTime** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja lõpp-punkti** tekstiväli.

    c.  Avage **Notepadis**base-64-kodeeritud sertifikaat, kopeerige sisu ja seejärel kleepige **x.509 vastav sert** tekstiväli.
    
    d.  Klõpsake nuppu **Salvesta**.

7.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Konfigureerimine Ühekordne sisselogimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate ClickTime sisse logida, nad peavad olema ettevalmistatud ClickTime.  
Puhul ClickTime, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **ClickTime** rentniku.

2.  Ülaosas tööriistaribal klõpsake **ettevõtte**ja seejärel klõpsake nuppu **inimesed**.

    ![Inimesed] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Inimesed")

3.  Klõpsake **isiku lisamine**.

    ![Isiku lisamine] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Isiku lisamine")

4.  Uue isiku jaotises tehke järgmist.

    ![Inimesed] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Inimesed")

    lisamine.  Tippige väljale **meiliaadress** oma Azure AD konto meiliaadress.
    
    b.  Tippige väljale **täisnimi** nimi Azure AD konto.  

    >[AZURE.NOTE] Kui soovite, saate täiendavaid uue isiku objekti atribuudid.

    c.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud ClickTime kasutaja konto loomise tööriistade või API-de esitatud ClickTime ettevalmistamise Azure AD Kasutajakontod.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon ClickTime tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200]

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

**ClickTime Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **ClickTime**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]

## <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine
Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui olete klõpsanud ClickTime paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on ClickTime rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png