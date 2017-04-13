<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine @Task| Microsoft Azure"
    description="Siit saate teada, kuidas konfigureerida ühekordse sisselogimise vahel Azure Active Directory ja @Task."
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


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Õpetus: Azure'i Active Directory integreerimine@Task

Selle õpetuse eesmärk on näete, kuidas integreerida @Task Azure Active Directory (Azure AD).  
Integreerimine @Task Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs@Task
- Saate lubada kasutajate automaatselt saada allkirjastatud-on @Task (ühekordse sisselogimise) oma Azure AD kontode
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Konfigureerida integreerimine Azure AD @Task, teil vaja järgmist:

- Tellimuse Azure AD
- Mõne @Task ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. Lisades @Task galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-task-from-the-gallery"></a>Lisades @Task galeriist
Integreerimise konfigureerimiseks @Task üheks Azure AD, peate lisama @Task galeriist hallatavate SaaS rakenduste loendisse.

**Lisada @Task galeriist, tehke järgmist:**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1] 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2] 

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3] 

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4] 

6. Tippige väljale Otsi **@Task**.

    ![Rakenduste][5] 

7. Valige paanil tulemuste **@Task**, ja seejärel klõpsake nuppu **täielik** rakenduse lisamiseks.

    ![Rakenduste][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise

Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise koos @Task nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, millised töölauafunktsioonid kasutaja @Task Azure AD kasutajale on. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud @Task tuleb luua.   
Selle lingi seos on loodud määramine Azure AD väärtusena **kasutajanimi** **kasutajanimi** väärtus @Task.
 
Azure AD ühekordse sisselogimise koos testimiseks ja konfigureerimiseks @Task, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Loomise on @Tasktest kasutaja](#creating-a-halogen-software-test-user) ** – olema töölauafunktsioonid Britta Simon sisse @Taskthat Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise sisse oma @Task rakendus.

**Konfigureerida Azure AD ühekordse sisselogimise koos @Task, tehke järgmist:**

1. Azure'i klassikaline portaalis, klõpsake selle **@Task** rakenduste integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Klõpsake soovitud **Kuidas soovite kasutajad sisse logida @Task ** lehel, valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][7] 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Rakenduse sätete konfigureerimine][8] 
 
     lisamine. Tippige tekstiväljale **Logi sisse URL-i** kasutavad kasutajatele sisselogimise URL-i oma @Task rakendus (nt:*https://<Tenant name>.attask-ondemand.com*).

     b. Klõpsake nuppu **edasi**.

4. Klõpsake soovitud **konfigureerimine ühekordse sisselogimise veebisaidil @Task ** lehel nuppu **alla laadida metaandmete**, metaandmete faili kohapeal arvutisse salvestada ja seejärel klõpsake nuppu **edasi**.

    ![Mis on Azure AD-ühenduse][9] 



1. Sisselogimise oma @Task ettevõtte saidile administraatorina.

2. Minge **ühekordse sisselogimise konfigureerimine**.


1. **Ühekordse sisselogimise** dialoogiboksi, tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine][23]

    lisamine. **Tüüp**, valige **SAML 2.0**.

    b. Valige **teenusepakkuja ID-d**.

    c. Azure'i klassikaline portaalis, kopeerige **Remote sisselogimise URL-i**ja seejärel kleepige **URL-i Logi sisse portaali** tekstiväli.

    d. Azure'i klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i**ja seejärel kleepige **Sign-Out URL** tekstiväli.

    e. Azure'i klassikaline portaalis, kopeerige **URL-i Muuda parooli**ja seejärel kleepige **URL-i Muuda parooli** tekstiväli.

    f. Klõpsake nuppu **Salvesta**.

6. Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Mis on Azure AD-ühenduse][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Mis on Azure AD-ühenduse][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.  

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-an-task-test-user"></a>Luua mõne @Task testkasutaja

Selle jaotise eesmärk on nimega Britta Simon kasutaja loomiseks @Task.


**Nimega Britta Simon kasutaja loomiseks @Task, tehke järgmist:**

1. Logige sisse oma @Task ettevõtte saidile administraatorina.

2. Klõpsake menüü peal, **inimesed**.

3. Klõpsake nuppu **Uus**. 

4. Klõpsake dialoogiboksis Uus inimene tegema järgmised toimingud:

    ![Saate luua ka @Task testkasutaja][21] 

    lisamine. Tippige tekstiväljale **eesnimi** "Britta".

    b. Tippige tekstiväljale **Perekonnanimi** "Simon".

    c. Tippige väljale **Meiliaadress** Britta Simon e-posti aadressi Azure Active Directory.

    d. Klõpsake **isiku lisamine**.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise andes tema juurdepääsu lubamine @Task.

![Kasutaja määramine][200] 

**Britta Simon, et määrata @Task, tehke järgmist:**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **@Task**.

    ![Kasutaja määramine][202] 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui klõpsate selle @Task paani paanil Accessi saate automaatselt sisse loginud-on teie @Task rakendus.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






