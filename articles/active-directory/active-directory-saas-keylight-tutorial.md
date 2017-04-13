<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Keylight | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Keylight vahel."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Õpetus: Azure'i Active Directory integreerimine Keylight

Selles õppetükis saate teada, kuidas Keylight integreerimine Azure Active Directory (Azure AD).

Keylight integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Keylight
- Saate lubada automaatselt saada allkirjastatud-on Keylight (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Keylight, peate järgmised üksused:

- Azure'i tellimuse
- Mõne Keylight ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Keylight lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-keylight-from-the-gallery"></a>Keylight lisamine galeriist
Keylight integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Keylight galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Keylight lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Keylight**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. Tulemuste paanil valige **Keylight**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Keylight nimega "Britta Simon" testkasutaja põhjal.

Azure AD ühekordse sisselogimise koos Keylight testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Keylight testida](#creating-a-keylight-test-user)** - olema töölauafunktsioonid Britta Simon Keylight Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Keylight rakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise Keylight, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Keylight** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 


2. **Kuidas soovite kasutajad logida Keylight** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    lisamine. Logige sisse URL väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Keylight rakenduse abil järgmist mustrit: **"https://\<ettevõtte nime\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Lehel **Konfigureeri ühekordse sisselogimise Keylight veebisaidil** , tehke järgmist:
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Klõpsake Keylight SSO lubamiseks tehke järgmist:
 
    lisamine. Sisselogimise kontole Keylight administraatorina.

    b. Menüü üleval, klõpsake **isiku**ja **Keylight häälestus**.
       
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. Klõpsake vasakul treeview, **SAML**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. Klõpsake dialoogiboksis **SAML sätted** nuppu **Redigeeri**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Lehel **Redigeeri SAML sätted** dialoogiboksi tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/405.png) 

    lisamine. Seadke **SAML autentimine** **aktiivne**.


    b. Azure AD klassikaline portaalis, kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli.

    c. Azure AD klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja välju URL-i** tekstiväli.

    d. Valige oma Keylight allalaaditud serdi valimiseks **Valige fail** ja klõpsake **avatud** laadige üles sert.


    e. **SAML kasutaja Id asukoha** määramine **NameIdentifier osa lause teema**.
   
    f. Sisestage soovitud **Keylight teenusepakkuja, kasutades järgmist mustrit: **https://&lt;ettevõtte nime&gt;. keylightgrc.com**.

    g. **Automaatne – kasutajate** määramine **aktiivne**.

    h. **Automaatne-sätte konto tüüp** väärtuseks **Täielik kasutaja**.

    Ma. **Automaatne-sätte turvalisus roll**, valige **Standard kasutaja SAML**.
   
    j. Nagu **Automaatne-sätte turvalisus config**, valige **Standard Kasutaja konfiguratsioon**.
   
    k. Tippige tekstiväljale atribuut **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Tippige tekstiväljale **eesnimi atribuut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. Tippige tekstiväljale **viimase nime atribuut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Klõpsake nuppu **Salvesta**.
   
  
   
  
6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises test kasutaja nimega Britta Simon Azure klassikaline portaali loomine.

Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]



**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-keylight-test-user"></a>Keylight testkasutaja loomine

Selles jaotises nimega Britta Simon Keylight kasutaja loomine. Keylight toetab just in time ettevalmistamise, mis on vaikimisi sisse lülitatud.

On teile selle jaotise pole toimingu toode. Kui juurdepääs Keylight, kui kasutajal pole veel olemas, luuakse uus kasutaja. 

> [AZURE.NOTE] Kui teil on vaja luua kasutaja käsitsi, peate Keylight tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon Keylight tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Keylight Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Keylight**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui olete klõpsanud Keylight paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Keylight rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
