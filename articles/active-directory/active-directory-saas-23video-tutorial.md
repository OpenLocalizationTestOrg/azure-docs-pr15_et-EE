<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine 23 Video | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja 23 Video vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Õpetus: Azure'i Active Directory integreerimine 23 Video

Selle õpetuse eesmärk näitavad, kuidas 23 Video integreerimine Azure Active Directory (Azure AD).  
23 integreerimine Azure AD Video annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs 23 Video 
- Saate lubada automaatselt saada allkirjastatud-on 23 video (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Azure AD integreerimise konfigureerimiseks 23 video vajate järgmist:

- Tellimuse Azure AD
- Mõne 23 Video ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. 23 Video lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-23-video-from-the-gallery"></a>23 Video lisamine galeriist
23 Video integreerimine Azure AD konfigureerimiseks peate 23 Video lisamine galeriist hallatavate SaaS rakenduste loendit.

**Galeriist 23 Video lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **23 Video**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **23 Video**, ja klõpsake rakenduse lisamiseks **valmis** .

    ![Rakenduste][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise 23 video nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja 23 video Azure AD kasutajale. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud 23 Video tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** 23 Video väärtus **kasutaja nimi** väärtus.
 
Konfigureerimine ja testimine Azure AD ühekordse sisselogimise 23 video, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[23 Video loomise katse kasutaja](#creating-a-23-video-test-user)** – 23 Video, mis on seotud Azure AD kujutis tema töölauafunktsioonid Britta Simon olema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise 23 Video rakenduse.

**Azure AD ühekordse sisselogimise 23 video konfigureerimiseks tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **23 Video** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajate 23 Video sisse logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][7] 

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksis tehke järgmist.

    ![Azure'i AD ühekordse sisselogimise][8] 
 
     lisamine. Tippige tekstiväljale **Vastus URL-i** kasutavad kasutajad 23 Video saidile sisselogimise URL-i (nt: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] SAML 2.0 abil Active Directory integreerimine on saadaval kõikide 23 Video kasutajate jaoks. Võtke ühendust toega veebisaidil [support@23company.com](mailto:support@23company.com) kui teil on vaja seotud metaandmed.

     b. Klõpsake nuppu **edasi**.
 
4. Klõpsake lehel **Konfigureeri ühekordse sisselogimise 23 video** tehke järgmist:

    ![Azure'i AD ühekordse sisselogimise][9] 

    lisamine. Klõpsake nuppu Laadi alla serdi ja seejärel salvestage fail oma arvutis.

    b. Pöörduge kaudu 23 Video tugimeeskonna poole [support@23company.com](mailto:support@23company.com), osutaksid allalaaditud serdi, **Väljaandja URL-i**, **Ühekordse sisselogimise teenuse URL-i**, **Ühe Sign-Out URL-i**ja seejärel paluge tal SSO rakenduse 23 Video häälestamine. 

    c. Klõpsake nuppu **edasi**.


6. Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboks lehel tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksi lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-23-video-test-user"></a>23 Video testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon 23 Video kasutaja loomiseks.

**Nimega Britta Simon 23 Video kasutaja loomiseks tehke järgmist:**

1. Logige administraatorina 23 Video ettevõtte saidil.

1. Valige **sätted**.


2. Klõpsake jaotises **Kasutajad** **konfigureerimine**.

    ![Kasutaja määramine][400]

3. Klõpsake nuppu **Lisa uus kasutaja**. 

    ![Kasutaja määramine][401]

4. Jaotises **kutsuda kedagi liitumine sellel saidil** tehke järgmist:

    ![Kasutaja määramine][402]

    lisamine. Tippige tekstiväljale **meiliaadressid** Azure AD Britta Simon e-posti aadress.

    b. Klõpsake nuppu **Lisa kasutaja**.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise 23 Video tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**23 Video Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **23 Video**.

    ![Kasutaja määramine][202] 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui klõpsate Video 23 paani paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on 23 Video rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




