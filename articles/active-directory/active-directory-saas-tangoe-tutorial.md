<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Tangoe käsk Premium Mobile | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Tangoe käsk Premium Mobile vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Õpetus: Azure'i Active Directory integreerimine Tangoe käsk Premium Mobile

Selles õppetükis saate teada, kuidas Tangoe käsk Premium Mobile integreerimine Azure Active Directory (Azure AD).

Tangoe käsk Premium Mobile integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Tangoe käsk Premium Mobile
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Tangoe käsk Premium Mobile (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks Tangoe käsk Premium Mobile vajate järgmist:

- Azure'i tellimuse
- Mõne Tangoe käsu Premium Mobile ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Tangoe käsk Premium Mobile lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Tangoe käsk Premium Mobile lisamine galeriist
Tangoe käsk Premium Mobile integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Tangoe käsk Premium Mobile galeriist hallatavate SaaS rakenduste loendisse.

**Tangoe käsk Premium Mobile galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Tangoe käsk Premium Mobile**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. Tulemuste paanil valige **Tangoe käsk Premium Mobile**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Tangoe käsk Premium Mobile'i nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis töölauafunktsioonid kasutaja Tangoe käsk Premium Mobile on kasutajale Azure AD. Teisisõnu, peab toimuma Azure AD kasutaja ja seotud kasutaja Tangoe käsk Premium Mobile link seos.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Tangoe käsk Premium Mobile väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise Tangoe käsk Premium Mobile, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise Tangoe käsk Premium Mobile testimiseks](#creating-an-tangoe-test-user)** – olema töölauafunktsioonid Britta Simon Tangoe käsk Premium Mobile Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Tangoe käsk Premium Mobile rakenduse.


**Azure AD ühekordse sisselogimise Tangoe käsk Premium Mobile konfigureerimiseks tehke järgmist.**

1. Klassikaline portaalis lehel **Tangoe käsk Premium Mobile** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6]


2. Lehel **Kuidas soovite kasutajad logida Tangoe käsk Premium Mobile** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    lisamine. **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Tangoe käsk Premium Mobile rakenduse abil järgmist mustrit: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<rentniku väljaandja\>& Target =\<lehe URL-i siht\>"**.

    b. Tippige tekstiväljale **Vastus URL-i** järgmise mustriga URL: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Kui te ei tea õigete väärtuste jaoks URL-id, saate kasutada väärtuste kohal kohatäited ja taotlus on õige väärtused oma Tangoe klienditoe seostada.


4. Lehel **Konfigureeri ühekordse sisselogimise on Tangoe käsk Premium Mobile** , tehke järgmist.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5.  Rakenduse jaoks konfigureeritud SSO saamiseks võtke ühendust oma Tangoe klienditoe seostada ja sisestage järgmine:


    - Metaandmete allalaaditud faili
    - **Väljaandja URL-i**
    - **SAML SSO URL-i**
    - **Ühe väljunud teenuse URL-i**


  
6. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.

Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Loomise rakendust Tangoe käsk Premium Mobile test

Selles jaotises nimega Britta Simon Tangoe käsk Premium Mobile kasutaja loomine. Tangoe käsk Premium mobiilirakenduse peate olema ette valmistatud enne ühekordse sisselogimise rakenduse kõik kasutajad. Palun töö koos Tangoe kliendi tugi seostada rakendusse kõigi nende kasutajate ettevalmistamine. 


> [AZURE.NOTE] Kui teil on vaja luua kasutaja käsitsi või partii kasutajate, peate Tangoe käsk Premium Mobile tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise Tangoe käsk Premium Mobile'i kasutada.

![Kasutaja määramine][200] 

**Tangoe käsk Premium Mobile Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Tangoe käsk Premium Mobile**.

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui klõpsate paanil Accessi paani Tangoe käsk Premium Mobile, saate tuleks saada automaatselt allkirjastatud-on Tangoe käsk Premium Mobile rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
