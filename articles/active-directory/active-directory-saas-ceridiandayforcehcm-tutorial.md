<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Ceridian Dayforce HCM | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Ceridian Dayforce HCM vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Õpetus: Azure'i Active Directory integreerimine Ceridian Dayforce HCM

Selle õpetuse eesmärk näitavad, kuidas Ceridian Dayforce HCM integreerimine Azure Active Directory (Azure AD).  
Ceridian Dayforce HCM integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Ceridian Dayforce HCM
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Ceridian Dayforce HCM (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto


Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Ceridian Dayforce HCM, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Ceridian Dayforce HCM ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Ceridian Dayforce HCM lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Ceridian Dayforce HCM lisamine galeriist
Ceridian Dayforce HCM integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Ceridian Dayforce HCM galeriist hallatavate SaaS rakenduste loendisse.

**Ceridian Dayforce HCM galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Ceridian Dayforce HCM**.

    ![Rakenduste](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Tulemuste paanil valige **Ceridian Dayforce HCM**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Ceridian Dayforce HCM nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Ceridian Dayforce HCM töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Ceridian Dayforce HCM tuleb luua.

Azure AD ühekordse sisselogimise koos Ceridian Dayforce HCM testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise Ceridian Dayforce HCM testimiseks](#creating-a-ceridian-dayforce-hcm-test-user)** – olema töölauafunktsioonid Britta Simon Ceridian Dayforce HCM Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Ceridian Dayforce HCM rakenduse.

Teie Ceridian Dayforce HCM rakendus eeldab SAML kinnitused kindlas vormingus. Tehke koostööd Dayforce HCM meeskonnatöö esmalt õige kasutajatunnus tuvastamiseks. Microsoft soovitab **"nimi"** atribuuti mis kasutaja ID abil. Saate hallata **"Atrribute"** dialoogiboksis selle atribuudi väärtust. Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Konfigureerida Azure AD ühekordse sisselogimise Ceridian Dayforce HCM, tehke järgmist.**




1. Azure'i klassikaline portaalis lehel **Ceridian Dayforce HCM** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. **Saml Turbeloa atribuutide** loendist atribuudid, valige atribuudi nimi ja seejärel klõpsake nuppu **Redigeeri**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. Klõpsake dialoogiboksis **Kasutaja atribuutide redigeerimiseks** tehke järgmist.
 
    lisamine. Valige loendist **Väärtus** kasutaja atribuut, mida soovite kasutada oma rakendamiseks.  
    Näiteks, kui soovite kasutada funktsiooni Tabeliseose kordumatu kasutajatunnus ja teil on salvestatud atribuudi väärtus on ExtensionAttribute2, seejärel valige **user.extensionattribute2**. 

    b. Klõpsake nuppu **valmis**.  
    



1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Klõpsake lehel **Ceridian Dayforce HCM** rakenduse integreerimine Azure klassikaline portaalis **konfigureerimine ühekordse sisselogimise**.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Ceridian Dayforce HCM** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse kasutajatele sisselogimise Ceridian Dayforce HCM rakenduse abil. Tootmise keskkonnas, kasutage järgmist URL-i vorming.`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Selgust, asendage DayforcehcmNamespace nimeruum keskkonna või oma ettevõtte ID; näiteks:`https://sso.dayforcehcm.com/contoso`
    
    Test keskkonnas, kasutage järgmist URL-i vorming.`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. Tippige tekstiväljale **Vastus URL-i** URL, mida kasutatakse Azure AD postitamiseks vastus.  
    Tootmise keskkonnas, kasutage:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Test keskkonnas, kasutage:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Lehel **Konfigureeri ühekordse sisselogimise Ceridian Dayforce HCM veebisaidil** , tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO, võtke ühendust oma Ceridian Dayforce HCM klienditoega e-posti ja saatke neile järgmine:

    - Allalaaditud serdifail
    - **Väljaandja URL-i**
    - **SAML SSO URL-i** 
    - **Ühekordse sisselogimise teenuse URL-i välja** 


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Ceridian Dayforce HCM testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Ceridian Dayforce HCM kasutaja loomiseks. 

Tehke koostööd Ceridian Dayforce HCM tugimeeskonna kasutajatel lisatakse teie Ceridian Dayforce HCM rakenduse. 





### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Ceridian Dayforce HCM tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Ceridian Dayforce HCM Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Ceridian Dayforce HCM**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud Ceridian Dayforce HCM paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Ceridian Dayforce HCM rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
