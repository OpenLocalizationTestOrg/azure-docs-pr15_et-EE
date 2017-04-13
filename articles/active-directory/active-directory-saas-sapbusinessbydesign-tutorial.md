<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SAP-i Business ByDesign | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja SAP-i Business ByDesign vahel."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Õpetus: Azure'i Active Directory ja SAP-i Business ByDesign integreerimine

Selles õppetükis saate teada, kuidas SAP-i Business ByDesign integreerimine Azure Active Directory (Azure AD).

SAP-i Business ByDesign integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs SAP-i Business ByDesign
- Saate lubada kasutajatele automaatselt saada allkirjastatud-on SAP-i Business ByDesign (ühekordse sisselogimise) oma Azure AD kontod
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD SAP-i Business ByDesign, peate järgmised üksused:

- Tellimuse Azure AD
- On SAP-i Business ByDesign ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. SAP-i Business ByDesign lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP-i Business ByDesign lisamine galeriist
SAP-i Business ByDesign integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada SAP-i Business ByDesign galeriist hallatavate SaaS rakenduste loendisse.

**SAP-i Business ByDesign galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.


3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **SAP-i Business ByDesign**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. Tulemuste paanil valige **SAP-i Business ByDesign**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja SAP-i Business ByDesign põhjal nimega "Britta Simon" testkasutaja Azure AD ühekordse sisselogimise test.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis SAP-i Business ByDesign töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja SAP-i Business ByDesign seotud kasutaja peab toimuma.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** SAP-i Business ByDesign väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise ja SAP-i Business ByDesign testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[SAP-i Business ByDesign loomise katse kasutaja](#creating-an-sap-business-bydesign-test-user)** - olema töölauafunktsioonid Britta Simon SAP-i Business ByDesign Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise SAP-i Business ByDesign rakenduse.

SAP-i Business ByDesign rakendus eeldab SAML kinnitused kindlas vormingus. Konfigureerige järgmised nõuded selle rakenduse jaoks. Nende atribuutide väärtused saate hallata rakenduse menüüs **"Atrribute"** . Järgmine pilt kuvatakse järgmine näide. 


**SAP-i Business ByDesign konfigureerida Azure AD ühekordse sisselogimise, tehke järgmist.**


1. Azure'i klassikaline portaalis lehel **SAP-i Business ByDesign** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Valige nameidentifier atribuut atribuutide SAML Turbeloa atribuutide loendis ja seejärel klõpsake nuppu **Redigeeri**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. Klõpsake dialoogiboksis kasutaja atribuutide redigeerimiseks tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    lisamine. Valige loendist väärtus **ExtractMailPrefix()** fuction

    b. Valige loendist e-posti kasutaja atribuut, mida soovite kasutada oma rakendamiseks. 
    Näiteks, kui soovite kasutada funktsiooni Tabeliseose kordumatu kasutajatunnus ja teil on salvestatud atribuudi väärtus on ExtensionAttribute2, seejärel valige **user.extensionattribute2**. 

    c. Klõpsake nuppu **valmis**. 
    

4. Klassikaline portaalis lehel **SAP-i Business ByDesign** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

5. **Kuidas soovite kasutajad logida SAP-i Business ByDesign** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad sisselogimise SAP-i Business ByDesign rakenduse abil järgmist mustrit:`https://<servername>.sapbydesign.com`
    
    b. Klõpsake nuppu **Järgmine**
 
7. Lehel **Konfigureeri ühekordse sisselogimise SAP-i Business ByDesign veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


8. Rakenduse jaoks konfigureeritud SSO saamiseks tehke järgmist.

    lisamine. Logige oma SAP-i Business ByDesign portaali administraatori õigused.

    b. Avage **rakendus ja kasutajale halduse levinud tööülesande** ja klõpsake vahekaarti **Identiteedipakkuja** .

    c. Klõpsake **Uue identiteedipakkuja** ja valige Azure klassikaline portaalist allalaaditud metaandmete XML-fail. Metaandmete importimisega süsteemi automaatselt lisatud nõutav allkirjasert ja krüptimise serti.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. SAML taotluse **Kinnituse tarbija teenuse URL-i** lisada, valige **Kaasa kinnituse tarbija teenuse URL**.

    e. Klõpsake nuppu **Aktiveeri ühekordse sisselogimise**.

    f. Salvestage muudatused.

    g. Klõpsake vahekaardil **Minu süsteemi** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. **SSO URL-i** kopeerimine ja kleepimine **Azure AD sisselogimise URL-i** tekstiväli.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    Ma. Määrake, kas töötaja valida käsitsi, valides **Käsitsi identiteedi pakkuja valiku**kasutaja ID ja parool või SSO sisselogimist.

    j. Määrake jaotises **SSO URL-i** URL, mis tuleks kasutada töötaja süsteemi sisselogimiseks. 
    Selle URL-i saadetud töötaja ripploendist, saate valida järgmiste võimaluste vahel:
    
    **URL d**
 
    Süsteem saadab töötaja ainult tavaline süsteemi URL. Töötaja ei saa sisse logida SSO kasutamist, ja peab parooli kasutamine või serdi hoopis.

    **SSO URL-I** 

    Süsteem saadab töötaja ainult SSO URL. Töötaja saate sisse logida SSO kasutamist. Autentimine taotluse on ümber ka IdP kaudu.

    **Automaatne valimine**
 
    Kui SSO-d ei ole aktiveeritud, saadab süsteem töötaja tavapärase süsteemi URL-i. Kui SSO-d on aktiivne, süsteem kontrollib, kas töötaja on parooliga kaitstud. Kui parool on saadaval, saadetakse töötaja nii SSO URL-i ja mitte-SSO URL. Siiski juhul, kui töötaja parooli, saadetakse ainult SSO URL-i töötaja.

    k. Salvestage muudatused.

9. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

10. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>SAP-i Business ByDesign testi kasutaja loomine

Selles jaotises nimega Britta Simon SAP-i Business ByDesign kasutaja loomine. Tehke koostööd SAP-i Business ByDesign tugimeeskonna SAP-i Business ByDesign platvormi kasutajate lisamiseks. 

> [AZURE.NOTE] Veenduge, et NameID väärtus peaksid olema samad SAP-i Business ByDesign platvormi kasutajanimi väljaga.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon kasutama Azure'i ühekordse sisselogimise, andes talle juurdepääsu SAP-i Business ByDesign.

![Kasutaja määramine][200] 

**SAP-i Business ByDesign Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **SAP-i Business ByDesign**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui klõpsate paanil juurdepääsu SAP-i Business ByDesign paani, mida peaks saada automaatselt allkirjastatud-on SAP-i Business ByDesign rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
