<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine MCM | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory MCM kasutamine!" 
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
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Õpetus: Azure'i Active Directory integreerimine MCM
  
Selle õpetuse eesmärk näitavad, kuidas MCM integreerimine Azure Active Directory (Azure AD).

MCM integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs MCM
- Saate lubada automaatselt saada allkirjastatud-on MCM (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD MCM, peate järgmised üksused:

- Azure'i kehtiv tellimus
- Mõne MCM ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. MCM lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise

## <a name="adding-mcm-from-the-gallery"></a>MCM lisamine galeriist
MCM integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada MCM galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist MCM lisamiseks tehke järgmist.**

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **MCM**.

    ![Rakenduse Galerii] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **MCM**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise MCM nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD MCM töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud MCM tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** MCM väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos MCM testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise on MCM testida](#creating-a-mcm-test-user)** - olema töölauafunktsioonid Britta Simon MCM Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine
  
Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise MCM rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise MCM, tehke järgmist.**

1.  Azure'i klassikaline portaalis lehel **MCM** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida MCM** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Microsoft Azure'i AD ühekordse sisselogimise] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure'i AD ühekordse sisselogimise")

3.  Rakenduse sätete konfigureerimine dialoogiboksi lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Rakenduse URL-i konfigureerimine")

    lisamine. Tippige tekstiväljale **Logi sisse URL** : `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise MCM veebisaidil** klõpsake nuppu **allalaadimine metaandmete**ja salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Ühekordse sisselogimise konfigureerimine")

5. Rakenduse jaoks konfigureeritud SSO saamiseks pöörduge MCM tugimeeskonna poole. Metaandmete allalaaditud faili manustamine ja selle ühiskasutusse MCM meeskonnatöö temapoolne SSO häälestamiseks.

6.  Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Ühekordse sisselogimise konfigureerimine")

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Ühekordse sisselogimise konfigureerimine")


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine

Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

###<a name="creating-a-mcm-test-user"></a>MCM testkasutaja loomine
  
Selles jaotises nimega Britta Simon MCM kasutaja loomine. Tehke koostööd MCM tugimeeskonna MCM platvormi kasutajate lisamiseks.

>[AZURE.NOTE]Saate kasutada mis tahes muud MCM kasutaja konto loomise tööriistade või API-de osutavad MCM kasutajakontode AAD.


###<a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine
  
Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise MCM tema juurdepääsuõiguse andmise lubamine.
    
![Kasutajate määramine] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Kasutajate määramine")

**MCM Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
    
    ![Kasutajate määramine] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Kasutajate määramine")

2. Valige rakenduste loendis **MCM**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutajate määramine] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Kasutajate määramine")

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.
    
    ![Kasutajate määramine] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Kasutajate määramine")


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.
 
Kui olete klõpsanud MCM paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on MCM rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)