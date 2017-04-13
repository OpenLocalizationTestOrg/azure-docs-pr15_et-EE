<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine PostBeyond | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja PostBeyond vahel."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-postbeyond"></a>Õpetus: Azure'i Active Directory integreerimine PostBeyond

Selles õppetükis saate teada, kuidas PostBeyond integreerimine Azure Active Directory (Azure AD).

PostBeyond integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs PostBeyond
- Saate lubada automaatselt saada allkirjastatud-on PostBeyond (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD PostBeyond, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne **PostBeyond** ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. PostBeyond lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-postbeyond-from-the-gallery"></a>PostBeyond lisamine galeriist
PostBeyond üheks Azure AD integreerimise konfigureerimiseks tuleb kõigepealt lisada PostBeyond galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist PostBeyond lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **PostBeyond**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_01.png)

7. Tulemuste paanil valige **PostBeyond**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise PostBeyond nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis PostBeyond töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud PostBeyond tuleb luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** PostBeyond väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos PostBeyond testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne PostBeyond testida](#creating-a-PostBeyond-test-user)** - olema töölauafunktsioonid Britta Simon PostBeyond Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise PostBeyond rakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise PostBeyond, tehke järgmist.**

1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine][6]

2. Klassikaline portaalis lehel **PostBeyond** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7] 

3. **Kuidas soovite kasutajad logida PostBeyond** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_06.png)

4. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_07.png)


    lisamine. Tippige tekstiväljale Logi sisse URL-i URL-i abil järgmist mustrit: `https://app.postbeyond.com`. 

    b. Klõpsake nuppu **edasi**.

5. Klõpsake soovitud **konfigureerimine ühekordse sisselogimise PostBeyond veebisaidil** lehele, klõpsake nuppu **Laadige alla serdi**, ja seejärel salvestage faili oma arvutist. Lisaks kopeerida väljaandja URL-i, ühekordse sisselogimise teenuse URL-i ja ühe väljunud teenuse URL-i väärtust. Peate selle teabe jagamine PostBeyond toe saamiseks SSO konfigureeritud.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_08.png)

6. Rakenduse jaoks konfigureeritud SSO saamiseks võtke ühendust PostBeyond klienditoega <sso@postbeyond.com>. Need aitavad proper kanal konfigureerimine SSO-d ja saatke neile järgmine: 

    - Allalaaditud serdi
    - **Väljaandja URL-i**
    - **SAML SSO URL-i**
    - **Ühe väljunud teenuse URL-i**

7. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-postbeyond-test-user"></a>PostBeyond testkasutaja loomine

Selles jaotises nimega Britta Simon PostBeyond kasutaja loomine. Kui te ei tea, kuidas lisada Britta Simon PostBeyond, võtke ühendust lisada testkasutaja ja lubada SSO PostBeyond toe poole. Pöörduge neid <sso@postbeyond.com>.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon PostBeyond tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**PostBeyond Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **PostBeyond**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_09.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kõik kasutajad **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud PostBeyond paanil Accessi peaks saama selle PostBeyond sisselogimisleht. Klõpsake **Office 365 sisse logida**, sisestage mandaat Azure AD. Seejärel saate peaks olema sisse logitud rakendusse PostBeyond.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_205.png
