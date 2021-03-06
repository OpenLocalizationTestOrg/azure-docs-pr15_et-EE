<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine 360° võrgus | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja 360° võrgus vahel."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-360-online"></a>Õpetus: Azure'i Active Directory integreerimine 360° võrgus


Selle õpetuse eesmärk näitavad, kuidas 360° võrgus integreerimine Azure Active Directory (Azure AD).

360° integreerimine Azure AD Online pakub teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs 360° Online Server
- Saate lubada automaatselt saada allkirjastatud-on 360 ° veebis (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD 360° võrgus, peate järgmised üksused:

- Tellimuse Azure AD
- 360° Online'i rentniku


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. 360° võrgus lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise



## <a name="adding-360-online-from-the-gallery"></a>360° võrgus lisamine galeriist
360° võrgus integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada 360° võrgus galeriist hallatavate SaaS rakenduste loendisse.


**360° võrgus galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **360 ° võrgus**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/tutorial_360online_01.png)

7. Tulemuste paanil valige **360 ° võrgus**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .
 
    ![Rakenduste](./media/active-directory-saas-360online-tutorial/tutorial_360online_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise 360 ° Online'i põhjal testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, milliseid töölauafunktsioonid 360° Online'i Azure AD kasutaja on kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud 360° Online'i tuleb luua.


Konfigureerimine ja testimine Azure AD ühekordse sisselogimise 360 ° võrgus, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[360 ° võrgus loomise katse kasutaja](#creating-a-360-online-test-user)** - töölauafunktsioonid, Britta Simon 360 ° Online, mis on seotud Azure AD kujutis tema olema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise oma 360° Online rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise 360° võrgus, tehke järgmist.**


1. Funktsiooni Azure klassikaline portaalis lehel **360 ° võrgus** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][13] 

2. Lehel **Kuidas soovite kasutajate 360 ° võrgus sisse logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-360online-tutorial/tutorial_360online_03.png) 

3. **Rakenduse URL-i konfigureerimine** dialoogiboksi lehe järgmiste toimingute ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-360online-tutorial/tutorial_360online_04.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad sisselogimise oma 360 ° Online rakenduse abil järgmist mustrit:`https://<company name>.public360online.com`

    b. Klõpsake nuppu **Järgmine**

4. **Rakenduse URL-i konfigureerimine** dialoogiboksi lehe järgmiste toimingute ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-360online-tutorial/tutorial_360online_05.png) 

    lisamine. Klõpsake **metaandmete alla laadida**ja oma arvutisse salvestada.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO saamiseks võtke ühendust oma 360° võrgutoe meeskonnatöö kaudu [360online@software-innovation.com](mailto:360online@software-innovation.com) ja allalaaditud metaandmete faili manustamiseks meili.

6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 


4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_05.png) 

    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.

    b. Tippige väljale **Kasutajanimi** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_07.png) 


8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-360online-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   




### <a name="creating-a-360-online-test-user"></a>360° Online testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon 360 ° veebis kasutaja loomiseks. 

360° võrgus loodud kasutaja saamiseks peate pöörduma kaudu Online tugimeeskonna 360° [360online@software-innovation.com](mailto:360online@software-innovation.com).


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon Azure ühekordse sisselogimise kasutada oma 360° võrgus juurdepääsu andmine.

![Kasutaja määramine][200] 

**360° võrgus Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
 
    ![Kasutaja määramine][201] 


2. Valige rakenduste loendis **360 ° võrgus**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-360online-tutorial/tutorial_360online_50.png) 


1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui klõpsate 360° võrgus paani paanil juurdepääsu, saate peaks saada automaatselt allkirjastatud-on teie 360 ° Online rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)







<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[10]: ./media/active-directory-saas-360online-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-360online-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-360online-tutorial/tutorial_general_08.png
[13]: ./media/active-directory-saas-360online-tutorial/tutorial_general_09.png
[20]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-360online-tutorial/tutorial_general_205.png
