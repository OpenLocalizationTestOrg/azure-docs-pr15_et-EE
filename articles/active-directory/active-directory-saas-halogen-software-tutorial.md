<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine halogeen tarkvara"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja halogeen tarkvara vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Õpetus: Azure'i Active Directory integreerimine halogeen tarkvara

Selle õpetuse eesmärk näitavad, kuidas halogeen tarkvara integreerimine Azure Active Directory (Azure AD).

Halogeen tarkvara integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs halogeen tarkvara 
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on halogeen tarkvara (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Azure AD integreerimise konfigureerimiseks halogeen tarkvara vajate järgmist:

- Tellimuse Azure AD
- Mõne halogeen tarkvara ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Halogeen tarkvara lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-halogen-software-from-the-gallery"></a>Halogeen tarkvara lisamine galeriist
Halogeen tarkvara integreerimine Azure AD konfigureerimiseks peate halogeen tarkvara Galerii lisamiseks loendisse hallatavad SaaS rakenduste.

**Galeriist halogeen tarkvara lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** . 

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **halogeen tarkvara**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **Halogeen tarkvara**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise halogeen tarkvara nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD halogeen tarkvara töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja halogeen tarkvara seotud kasutaja peab toimuma.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** halogeen tarkvara väärtus **kasutaja nimi** väärtus.
 
Konfigureerimine ja Azure AD ühekordse sisselogimise halogeen tarkvara katsetada, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühe ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise halogeen tarkvara katsetada](#creating-a-halogen-software-test-user)** - olema töölauafunktsioonid Britta Simon halogeen tarkvara, mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure'i AD ühe ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise halogeen tarkvara rakenduse.


**Azure AD ühekordse sisselogimise halogeen tarkvara konfigureerimiseks tehke järgmist.**

1. **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **Halogeen tarkvara** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise konfigureerimine][8]

2. **Kuidas soovite kasutajad logida halogeen tarkvara** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][9]

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:  ![rakenduse sätete konfigureerimine][10]
 
     lisamine. **Logige sisse URL** väljale Tippige oma URL-i kasutavad kasutajad sisse logida, kasutades järgmist mustrit halogeen tarkvara rakenduse: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Klõpsake nuppu **edasi**.
 
4. Lehel **Konfigureeri ühekordse sisselogimise halogeen tarkvara** **metaandmete allalaadimine**nuppu ja seejärel salvestage fail metaandmete teie arvutile.
    
    ![Mis on Azure AD-ühenduse][11]

5. Erinevate brauseriaknas sisselogimise **Halogeen tarkvara** rakenduse administraatorina.

6. Klõpsake vahekaarti **Suvandid** . 

    ![Mis on Azure AD-ühenduse][12]


7. Klõpsake vasakpoolsel navigeerimispaanil **SAML konfigureerimine**. 

    ![Mis on Azure AD-ühenduse][13]

8. **SAML konfiguratsiooni** lehele, tehke järgmist:  ![mis on Azure AD-ühenduse][14]

    lisamine. Kui **Kordumatut tunnust**, valige **NameID**.

    b. Valige **Kordumatu identifikaator kaartide abil**, nagu **kasutajanimi**.

    c. Metaandmete allalaaditud faili üleslaadimiseks klõpsake **sirvige** soovitud faili ja seejärel **Faili üles laadida**.

    d. Klõpsake konfiguratsiooni testimiseks **Käivitage Test**. 

    > [AZURE.NOTE] Peate ootama teates, et "*SAML test on lõpule viidud. Sulgege see aken*". Sulgege avatud brauseriaknas. Märkeruut **Luba SAML** on lubatud ainult siis, kui test on lõpule viidud.

    e. Valige **Luba SAML**.
    
    f. Klõpsake nuppu **Salvesta muudatused**. 


9. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks. 

    ![Mis on Azure AD-ühenduse][15]

10. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Mis on Azure AD-ühenduse][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Mis on Azure AD-ühenduse][100] 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Mis on Azure AD-ühenduse][101] 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Mis on Azure AD-ühenduse][102] 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Mis on Azure AD-ühenduse][103] 
 
    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu edasi.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Mis on Azure AD-ühenduse][104] 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Klõpsake soovitud **Perekonnanimi** txtbox, tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Mis on Azure AD-ühenduse][105]  

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Mis on Azure AD-ühenduse][106]   

    lisamine. Kirjutage üles väärtus **Uus parool**.
    b. Klõpsake nuppu **valmis**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Halogeen tarkvara testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon halogeen tarkvara kasutaja loomiseks.

**Nimega Britta Simon halogeen tarkvara kasutaja loomiseks tehke järgmist.**

1. **Halogeen tarkvara** rakenduse administraatorina sisse logida.

2. **Kasutaja keskele** vahekaarti ja seejärel klõpsake nuppu **Loo kasutaja**.

    ![Mis on Azure AD-ühenduse][300]  

3. Tehke lehel **Uus kasutaja** dialoogiboksi järgmist:

    ![Mis on Azure AD-ühenduse][301]

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**. 
  
    b. Tippige tekstiväljale **Perekonnanimi** **Simon**.
  
    c. Tippige väljale **kasutajanimi** **Brita Simon kasutajanimi Azure klassikaline portaalis**.
  
    d. Tippige väljale **parool** parool Britta.
  
    e. Klõpsake nuppu **Salvesta**.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise halogeen tarkvara kasutada.

![Mis on Azure AD-ühenduse][200]

**Halogeen tarkvara Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Mis on Azure AD-ühenduse][201]

2. Valige rakenduste loendis **Halogeen tarkvara**.

    ![Mis on Azure AD-ühenduse][202]

1. Klõpsake menüü peal, **Kasutajad**.

    ![Mis on Azure AD-ühenduse][203]

1. Valige loendis kasutajate **Britta Simon**.

    ![Mis on Azure AD-ühenduse][204]

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Mis on Azure AD-ühenduse][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui klõpsate paanil Accessi paani halogeen tarkvara, mida peaks saada automaatselt allkirjastatud-on halogeen tarkvara rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png