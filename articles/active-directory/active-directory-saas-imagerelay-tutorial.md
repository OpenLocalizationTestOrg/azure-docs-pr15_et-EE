<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ImageRelay | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja ImageRelay vahel."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-imagerelay"></a>Õpetus: Azure'i Active Directory integreerimine ImageRelay

Selle õpetuse eesmärk näitavad, kuidas ImageRelay integreerimine Azure Active Directory (Azure AD).  
ImageRelay integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs ImageRelay
- Saate lubada automaatselt saada allkirjastatud-on ImageRelay (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD ImageRelay, peate järgmised üksused:

- Tellimuse Azure AD
- ImageRelay ühekordse sisselogimise lubatud tellimus


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. ImageRelay lisamine galeriist

2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-imagerelay-from-the-gallery"></a>ImageRelay lisamine galeriist
ImageRelay integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada ImageRelay galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist ImageRelay lisamiseks tehke järgmist.**

1. Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **ImageRelay**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. Tulemuste paanil valige **ImageRelay**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise ImageRelay nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD kasutajakonto, mis tähistab ImageRelay seotud kasutaja.  Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud ImageRelay tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** ImageRelay väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos ImageRelay testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne ImageRelay testida](#creating-a-userecho-test-user)** - olema töölauafunktsioonid Britta Simon ImageRelay Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise ImageRelay rakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise ImageRelay, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **ImageRelay** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

     ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida ImageRelay** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

     ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) 

    lisamine. **Logige sisse URL-i** tekstivälja, tippige URL, mida kasutatakse teie kasutajad logida ImageRelay rakenduse (nt: *https://fabrikam.ImageRelay.com/*).

    b. Klõpsake nuppu **edasi**.

4. Lehel **Konfigureeri ühekordse sisselogimise ImageRelay veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.

5. Mõnes muus brauseriaknas, logige sisse saidil ImageRelay ettevõtte administraatorina.

1. Klõpsake tööriistaribal ülaosas töökoormus **kasutajad ja õigused** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Klõpsake **uue õigusetaseme loomine**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) 

1. **Ühekordse sisselogimise kohta sätted** töökoormus, märkige ruut **selle rühma saate ainult sisselogimine ühekordse sisselogimise kaudu** ja klõpsake siis nuppu **Salvesta**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Valige **konto sätted**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Minge **Ühekordse sisselogimise kohta sätted** töökoormus.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Klõpsake dialoogiboksis **SAML sätted** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

    lisamine. Azure'i klassikaline portaalis, kopeerige **Ühekordse sisselogimise teenuse URL-i**ja seejärel kleepige **Sisselogimise URL-i** tekstiväli.


    b. Azure'i klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i**ja seejärel kleepige **URL välju** tekstiväli.

    c. **Nimevormingut Id**, valige **urn: oasis: nimed: tc: SAML:1.1:nameid-vorming: emailAddress**.

    
    d. **Köitmine suvandite taotlusi teenuse pakkuja (pilt Relay)**, valige **postituse sidumine**.
   

    e. Jaotises **x.509 vastav sert**, klõpsake nuppu **Värskenda sert**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Avage allalaaditud serdi Notepadis, kopeerige sisu ja seejärel kleepige x.509 vastav sert tekstiväli.
  
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. Valige jaotises **Just kasutaja ettevalmistamise** **Lubamine just kasutaja ettevalmistamise**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Mis on lubatud ainult kuni ühekordse sisselogimise sisse logida (näiteks **Lihtsa SSO**) õiguste rühma valimine

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    Ma. Klõpsake nuppu **Salvesta**.

6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.

    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-imagerelay-test-user"></a>ImageRelay testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon ImageRelay kasutaja loomiseks.

**Nimega Britta Simon ImageRelay kasutaja loomiseks tehke järgmist.**

1. Sisselogimise saidile ImageRelay ettevõtte administraatorina.

1. Valige **kasutajad ja õigused** ja valige **SSO kasutaja loomine**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Sisestage **e-posti**, **eesnimi**, **Perekonnanimi** ja **ettevõtte** kasutaja, mida soovite ettevalmistamine ja valige õiguste rühm (nt SSO Basic), mis on rühma, kus saab sisse logida ainult kaudu ühekordse sisselogimise.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Klõpsake nuppu **Loo**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise ImageRelay tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**ImageRelay Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **ImageRelay**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud ImageRelay paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on ImageRelay rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png
