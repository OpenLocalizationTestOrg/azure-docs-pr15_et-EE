<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Everbridge | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Everbridge vahel."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Õpetus: Azure'i Active Directory integreerimine Everbridge

Selle õpetuse eesmärk näitavad, kuidas Everbridge integreerimine Azure Active Directory (Azure AD).

Everbridge integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Everbridge
- Saate lubada automaatselt saada allkirjastatud-on Everbridge (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Everbridge, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Everbridge ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Everbridge lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-everbridge-from-the-gallery"></a>Everbridge lisamine galeriist
Everbridge integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Everbridge galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Everbridge lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
    
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Everbridge**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_01.png)
7. Tulemuste paanil valige **Everbridge**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Everbridge nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Everbridge töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Everbridge tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Everbridge väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Everbridge testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne Everbridge testida](#creating-a-everbridge-test-user)** - olema töölauafunktsioonid Britta Simon Everbridge Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Everbridge rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise Everbridge, tehke järgmist.**

1. Klassikaline portaalis lehel **Everbridge** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Everbridge** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_03.png) 

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi tegema järgmised toimingud ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_04.png)

    lisamine. Tippige tekstiväljale **identifikaator** URL, kasutades järgmist mustrit:`https://sso.everbridge.net/{<company name>}`

    b. Tippige tekstiväljale **Vastus URL-i** URL, kasutades järgmist mustrit:`https://manager.everbridge.net/saml/SSO/{<company name>}/alias/defaultAlias`

    c. Klõpsake nuppu **Järgmine**

4. Lehel **konfigureerimine ühekordse sisselogimise veebisaidil Everbridge** järgmiste toimingute ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_05.png)

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.

5. Saada SSO rakenduse jaoks konfigureeritud, peate oma Everbridge rentniku administraatorina sisselogimise.

6. Menüü peal, klõpsake vahekaarti **sätted** ja valige **Ühekordse sisselogimise** jaotises **Turve**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)

    lisamine. Tippige väljale **nimi** identifikaator pakkuja nimi (nt: teie ettevõtte nime).

    b. Tippige tekstiväljale **API nimi** API nimi.

    C. Klõpsake **Faili valimine** nuppu metaandmete faili, mille saate alla laadida **Samm**4 üles laadida.

    d. **SAML identiteedi asukoht**, valige "Identiteedi NameIdentifier elemendi lause teema on".

    e. Kopeerige URL SAML SSO Azure AD Everbridge **identiteedi pakkuja sisselogimise URL-i** .
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)

    f. Valige **Teenuse pakkuja algatatud taotlemine Köitmine**, HTTP Redirect.

7. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

Valige loendis kasutajate **Britta Simon**.
    
![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_09.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_05.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_06.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-everbridge-tutorial/create_aaduser_08.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-everbridge-test-user"></a>Everbridge testkasutaja loomine

Selles jaotises nimega Britta Simon Everbridge kasutaja loomine. Võtke ühendust klienditoega Everbridge e <mailto:support@everbridge.com> Everbridge platvormi kasutajate lisamiseks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Everbridge tema juurdepääsuõiguse andmise lubamine.
    
![Kasutaja määramine][200]

**Everbridge Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **Everbridge**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_50.png)

3. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.

Kui olete klõpsanud Everbridge paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Everbridge rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_205.png
