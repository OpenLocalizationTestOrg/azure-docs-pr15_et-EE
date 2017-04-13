<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine eTouches | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja eTouches vahel."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Õpetus: Azure'i Active Directory integreerimine eTouches

Selles õppetükis saate teada, kuidas eTouches integreerimine Azure Active Directory (Azure AD).

ETouches integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs eTouches
- Saate lubada automaatselt saada allkirjastatud-on eTouches (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD eTouches, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne eTouches ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. ETouches lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-etouches-from-the-gallery"></a>ETouches lisamine galeriist
ETouches integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada eTouches galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist eTouches lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]
2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **eTouches**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_01.png)

7. Tulemuste paanil valige **eTouches**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise

Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise eTouches nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis eTouches töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud eTouches tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** eTouches väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos eTouches testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne eTouches testida](#creating-a-predictix-price-reporting-test-user)** - olema töölauafunktsioonid, Britta Simon eTouches lingitud Azure AD kujutis tema sisse.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise eTouches rakenduse.

eTouches rakendus eeldab SAML kinnitused kindlas vormingus. Konfigureerige järgmised nõuded selle rakenduse jaoks. Nende atribuutide väärtused saate hallata rakenduse menüüs **"Atrribute"** . Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_07.png) 

**Konfigureerida Azure AD ühekordse sisselogimise eTocuhes, tehke järgmist.**


1. Azure'i klassikaline portaalis lehel **eTouches** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_general_80.png) 


2. Klõpsake dialoogiboksis **SAML Turbeloa atribuute** iga rea, mis on näidatud järgmises tabelis, tehke järgmist:

  	| Atribuudi nimi | Atribuudi väärtus |
  	| --- | --- |    
  	| E-posti | User.mail |

    lisamine. Klõpsake **kasutaja atribuutide lisamine** **Lisada kasutaja Attribure** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_general_81.png) 


    b. Tippige tekstiväljale **Attrubute nime** atribuudi nimi, kuvatakse selle rea.

    c. **Atribuudi väärtus** loendis selsect selle rea atribuudi väärtust.

    d. Klõpsake nuppu **valmis**.  
    

3. Klassikaline portaalis lehel **eTouches** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

4. **Kuidas soovite kasutajad logida eTouches** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_03.png) 

5. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_04.png) 

    lisamine. **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise eTouches rakenduse abil järgmist mustrit: **https://www.eiseverywhere.com/saml/accounts/?sso&accountid=\<accountid\>**.
    
    b. Klõpsake nuppu **Järgmine**
 
6. Lehel **Konfigureeri ühekordse sisselogimise eTouches veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_05.png)

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


7. Rakenduse jaoks konfigureeritud SSO saamiseks tehke järgmist eTouches rakenduse:

    lisamine. Logi **eTouches** rakendus Admin õigused.
    
    b. Minge **SAML** konfigureerimine

    c. Klõpsake jaotises **Üldsätted** kleepida allolev Azure AD Federation metaandmete sisu.

    d. Klõpsake nuppu **Salvesta ja püsida**

    e. Klõpsake nuppu **Värskenda metaandmete** SAML metaandmete jaotises. 

    f. See on lehe avamiseks ja täidab SSO-d. Kui soovitud SSO töötab seejärel saate häälestada kasutajanimi

    g. Valige väljal **kasutajanimi** **emailaddress** , nagu on näidatud järgmisel pildil. 

    h. Kopeerige soovitud **SSO URL-i / ACS** väärtus ja asetage Azure AD rakenduse konfigureerimise viisardi Logi sisse URL-i tekstiväli.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png)

8. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

9. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
 
    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist:  ![Azure AD test kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehel tehke järgmist: ![Azure AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-etouches-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-etouches-test-user"></a>Loomise rakendust eTouches test

Selles jaotises nimega Britta Simon eTouches kasutaja loomine. Tehke koostööd eTouches tugimeeskonna eTouches platvormi kasutajate lisamiseks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon kasutada Azure ühekordse sisselogimise tema eTouches juurdepääsu andmine.

![Kasutaja määramine][200] 

**ETouches Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **eTouches**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui olete klõpsanud eTouches paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on eTouches rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_205.png
