<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Showpad | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Showpad vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-showpad"></a>Õpetus: Azure'i Active Directory integreerimine Showpad

Selle õpetuse eesmärk näitavad, kuidas Showpad integreerimine Azure Active Directory (Azure AD).

Showpad integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Showpad
- Saate lubada automaatselt saada allkirjastatud-on Showpad (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – Azure Active Directory portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Showpad, peate järgmised üksused:

- Tellimuse Azure AD
- Tellimuse Showpad


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Showpad lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-showpad-from-the-gallery"></a>Showpad lisamine galeriist
Showpad integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Showpad galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Showpad lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Rakenduste][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.
 
    ![Rakenduste][4]

6. Tippige otsinguväljale **Showpad**.

    ![Rakenduste](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. Tulemuste paanil valige **Showpad**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Showpad nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Showpad töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Showpad tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Showpad väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Showpad testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne Showpad testida](#creating-a-showpad-test-user)** - olema töölauafunktsioonid Britta Simon Showpad Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Showpad rakenduse.



**Konfigureerida Azure AD ühekordse sisselogimise Showpad, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Showpad** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Showpad** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png) 


    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad sisselogimise Showpad rakenduse abil järgmist mustrit:`https://<company name>.showpad.biz/login`

    b. Tippige tekstiväljale **identifikaator** URL, kasutades järgmist mustrit:`https://<company name>.showpad.biz`

    c. Klõpsake nuppu **Järgmine**


4. Lehel **Konfigureeri ühekordse sisselogimise veebisaidil Showpad** järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Sisselogimine oma Showpad rentniku administraatorina.

6. Menüü ülaosas nuppu **sätted**.

    ![Ühekordse sisselogimise App pool konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

7. Liikuge "**Ühekordse sisselogimise**" ja valige "**Luba**".
 
    ![Ühekordse sisselogimise App küljel konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. Klõpsake dialoogiboksis **Lisa teenus SAML 2.0** tehke järgmist.

    ![Ühekordse sisselogimise App küljel konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 

    lisamine. Tippige väljale **nimi** identifikaator pakkuja nimi (nt: teie ettevõtte nime).

    b. **Metaandmete allikas**, valige **XML-i**.

    c. Allalaaditud metaandmete XML-faili sisu ja kleepige need **Metaandmete XML-i** tekstiväli.

    d. Valige **Automaatne-sätte kontod uutel kasutajatel, kui nad sisse logima**.

    e. Klõpsake **esitada**.


10. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]


11. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]






### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD Showpad testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png) 

    lisamine. Tippige väljale **Kasutajanimi** **BrittaSimon**.

    b. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige **roll**, kui **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.


### <a name="creating-a-showpad-test-user"></a>Showpad testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Showpad kasutaja loomiseks. 

Showpad toetab just in time ettevalmistamise. Teil on lubatud ettevalmistamine on **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)**. 

On teile selle jaotise pole toimingu toode. 




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Showpad tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200]

**Showpad Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis menüü ülaosas nuppu **rakendused**.

    ![Kasutaja määramine][201] 

2. Klõpsake loendis rakenduste **Showpad**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]




### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud **Showpad** paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Showpad rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png
