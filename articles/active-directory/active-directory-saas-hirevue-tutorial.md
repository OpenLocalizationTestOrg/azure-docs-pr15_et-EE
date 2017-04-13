<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine HireVue | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja HireVue vahel."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hirevue"></a>Õpetus: Azure'i Active Directory integreerimine HireVue

Selles õppetükis saate teada, kuidas HireVue integreerimine Azure Active Directory (Azure AD).

HireVue integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs HireVue
- Saate lubada automaatselt saada allkirjastatud-on HireVue (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD HireVue, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne HireVue ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. HireVue lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-hirevue-from-the-gallery"></a>HireVue lisamine galeriist
HireVue integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada HireVue galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist HireVue lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]
2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **HireVue**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_01.png)

7. Tulemuste paanil valige **HireVue**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise HireVue nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis HireVue töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud HireVue tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** HireVue väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos HireVue testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne HireVue testida](#creating-a-predictix-price-reporting-test-user)** - olema töölauafunktsioonid Britta Simon HireVue Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise HireVue rakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise HireVue, tehke järgmist.**

1. Klassikaline portaalis lehel **HireVue** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida HireVue** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_04.png) 

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad sisselogimise HireVue rakenduse abil järgmist mustrit: 


  	| Keskkonnas | URL-I |
  	|---|---|
  	| Tootmise | `https://<company name>.hirevue.com` |
  	| Lavastus| `https://<company name>.stghv.com` |
 
    
    b. Tippige tekstiväljale **identifikaator** URN, kasutades järgmist mustrit.


  	| Keskkonnas | URN |
  	|---|---|
  	|Tootmise | `urn:federation:hirevue.com:saml:sp:prod` |
  	|Lavastus | `urn:federation:hirevue.com:saml:sp:staging` |



    c. Klõpsake nuppu **Järgmine**
 
4. Lehel **Konfigureeri ühekordse sisselogimise HireVue veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_05.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO saamiseks võtke ühendust klienditoega HireVue [samlsupport@hirevue.com](mailTo:samlsupport@hirevue.com) ja saatke neile järgmine:

    • Allalaaditud **serdi**

    • **Üksuse ID**

    • **SAML SSO URL-i**

    • **Ühekordse sisselogimise teenuse URL-i välja**

6. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist:  ![Azure AD test kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehel tehke järgmist: ![Azure AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hirevue-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-hirevue-test-user"></a>Loomise rakendust HireVue test

Selles jaotises nimega Britta Simon HireVue kasutaja loomine. Tehke koostööd HireVue tugimeeskonna HireVue platvormi kasutajate lisamiseks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon HireVue tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**HireVue Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **HireVue**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui olete klõpsanud HireVue paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on HireVue rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_205.png
