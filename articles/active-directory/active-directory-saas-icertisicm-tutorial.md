<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Icertis lepingu Management Platform | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Icertis lepingu halduse platvormi vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a>Õpetus: Azure'i Active Directory integreerimine Icertis lepingu Management Platform

Selle õpetuse eesmärk näitavad, kuidas Icertis lepingu Management Platform integreerimine Azure Active Directory (Azure AD).

Integreerimine Azure AD Icertis lepingu Management Platform pakub teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Icertis lepingu Management Platform
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Icertis lepingu halduse platvormi (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks Icertis lepingu halduse platvorm vajate järgmist:

- Tellimuse Azure AD
- Mõne Icertis lepingu Management Platform ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Icertis lepingu Management Platform lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a>Icertis lepingu Management Platform lisamine galeriist
Icertis lepingu Management Platform integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Icertis lepingu Management Platform galeriist hallatavate SaaS rakenduste loendisse.

**Icertis lepingu Management Platform galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
    
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Icertis lepingu Management Platform**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_01.png)

7. Tulemuste paanil valige **Icertis lepingu Management Platform**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise Icertis lepingu halduse platvormi nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Icertis lepingu Management Platform töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja Icertis lepingu Management Platform seotud kasutaja peab toimuma.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Icertis lepingu Management Platform väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise Icertis lepingu halduse platvorm, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise Icertis lepingu Management Platform testimiseks](#creating-a-icertis-contract-management-platform-test-user)** – olema töölauafunktsioonid Britta Simon Icertis lepingu halduse platvorm, mis on seotud Azure AD kujutis tema.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Icertis lepingu Management Platform rakenduse.

**Azure AD ühekordse sisselogimise Icertis lepingu halduse platvorm konfigureerimiseks tehke järgmist.**

1. Klassikaline portaalis lehel **Icertis lepingu Management Platform** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajad logida Icertis lepingu Management Platform** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_03.png) 

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi tegema järgmised toimingud ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_04.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i abil järgmist mustrit:`https://<company name>.icertis.com`

    b. Klõpsake nuppu **Järgmine**


    > [AZURE.NOTE] Pange tähele, et need ei ole tegeliku väärtuse. Peate värskendama neid väärtusi, URL tegelik Logi sisse. Need väärtused saamiseks pöörduge Icertis lepingu Management Platform.

4. Lehel **konfigureerimine ühekordse sisselogimise Icertis lepingu halduse platvormi** järgmiste toimingute ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_05.png)

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.

5. Rakenduse jaoks konfigureeritud SSO-d, võtke ühendust Icertis lepingu Management Platform tugimeeskonnale ja saatke neile järgmine: 

    - **Metaandmete allalaaditud** faili 
    
     **Üksuse ID** - 
        
    - **SAML SSO URL-i** 
        
    - **Ühe väljunud teenuse URL-i**

6. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.
    
![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_09.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_03.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_05.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_06.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_08.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-icertis-contract-management-platform-test-user"></a>Icertis lepingu Management Platform testkasutaja loomine

Selles jaotises nimega Britta Simon Icertis lepingu Management Platform kasutaja loomine. Tehke koostööd Icertis lepingu Management Platform tugimeeskonna Icertis lepingu halduse platvormi kasutajate lisamiseks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Icertis lepingu Management Platform tema juurdepääsuõiguse andmise lubamine.
    
![Kasutaja määramine][200]

**Icertis lepingu Management Platform Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **Icertis lepingu Management Platform**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_50.png)

3. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud Icertis lepingu Management Platform paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Icertis lepingu Management Platform rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_205.png
