<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ehk | Microsoft Azure'i"
    description="Siit saate teada, kuidas konfigureerida ühekordse sisselogimise vahel Azure Active Directory ja täpsemalt."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Õpetus: Azure'i Active Directory integreerimine ehk

Selle õpetuse eesmärk näitavad, kuidas ehk integreerimine Azure Active Directory (Azure AD).

Milleks integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs ehk 
- Saate lubada kasutajate automaatselt saada allkirjastatud-on ehk (ühekordse sisselogimise) oma Azure AD kontode
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Konfigureerida Azure AD integreerimine, mida on vaja järgmisi üksusi:

- Tellimuse Azure AD
- Mõne ehk ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Milleks lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-namely-from-the-gallery"></a>Milleks lisamine galeriist
Integreerimine ehk Azure AD konfigureerimiseks tuleb kõigepealt lisada ehk galeriist hallatavate SaaS rakenduste loendisse.

**Lisada ehk Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Väljale Otsi tippige **Täpsemalt**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. Tulemuste paanil valige **ehk**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise koos nimega "Britta Simon" testkasutaja ehk põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja täpsemalt, et olete kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud ehk tuleb luua.

Selle lingi seos on loodud määramine väärtus **kasutajanimi** **kasutajanimi** väärtusena Azure AD täpsemalt.
 
Konfigureerimine ja testimine Azure AD ühekordse sisselogimise abil, mida on vaja täita järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Loomise on ehk testida kasutaja](#creating-a-namely-test-user)** – olema töölauafunktsioonid Britta Simon ehk mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise ehk rakenduse. 




**Konfigureerida Azure AD ühekordse sisselogimise abil, tehke järgmist:**

1. Azure'i klassikaline portaalis lehel **ehk** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajate logida sisse ehk** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad logida oma ehk rakenduste (nt: *https://fabrikam.Namely.com/*).

    b. Klõpsake nuppu **edasi**.
 
 
4. Lehel **konfigureerimine ühekordse sisselogimise veebisaidil ehk** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


1. Mõnes muus brauseriaknas, logige sisse oma ehk ettevõtte saidi administraator.

1. Klõpsake tööriistaribal ülaosas **ettevõtte**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Klõpsake vahekaarti **sätted** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Klõpsake **SAML**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Klõpsake lehel **SAML sätted** tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    lisamine. Klõpsake nuppu **Luba SAML**. 

    b. Azure'i klassikaline portaalis lehel **konfigureerimine ühekordse sisselogimise veebisaidil ehk** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige **identiteedi pakkuja DDO URL-i** tekstiväli. 

    c. Avage oma allalaaditud serdi Notepadis, kopeerige sisu ja seejärel kleepige **identiteedi pakkuja serdi** tekstiväli.    

    d. Klõpsake nuppu **Salvesta**.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**. 

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]


**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-namely-test-user"></a>Loomise mõne kasutaja ehk testimine

Selle jaotise eesmärk on nimega Britta Simon ehk kasutaja loomiseks.

**Nimega Britta Simon ehk kasutaja loomiseks tehke järgmist.**

1. Sisselogimise oma ehk ettevõtte saidi administraator.

1. Klõpsake tööriistaribal nuppu **inimesed**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Klõpsake **soovitud vahekaarti** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Klõpsake nuppu **Lisa uus**.



1. Klõpsake dialoogiboksis **Uue isiku lisamiseks** tehke järgmist.

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.

    b. Tippige tekstiväljale **perekonnanimi** **Simon**.

    c. Tippige tekstiväljale **e-posti** Azure klassikaline portaalis Britta isiku meiliaadress.

    d. Klõpsake nuppu **Salvesta**.





### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon kasutada Azure ühekordse sisselogimise ehk tema juurdepääsu andmine.

![Kasutaja määramine][200] 

**Määrata Britta Simon, teha järgmist:**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Rakenduste loendis valige **Täpsemalt**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui klõpsate selle ehk paani Access paanil, mida peaks saada automaatselt sisse loginud-on ehk rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






