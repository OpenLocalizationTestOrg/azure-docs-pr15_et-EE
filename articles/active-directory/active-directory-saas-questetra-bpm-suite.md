<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Questetra l/min komplekti | Microsoft Aure"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Questetra l/min komplekti vahel."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Õpetus: Azure'i Active Directory integreerimine Questetra l/min komplekti

Selle õpetuse eesmärk näitavad, kuidas Questetra l/min komplekti integreerimine Azure Active Directory (Azure AD).  
Questetra l/min komplekti integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs Questetra l/min komplektile 
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Questetra l/min komplektile (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Questetra l/min Suite Azure AD integreerimise konfigureerimiseks vajate järgmist:

- Tellimuse Azure AD
- Mõne [Questetra l/min komplekti](https://senbon-imadegawa-988.questetra.net/) ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. Questetra l/min komplekti lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Questetra l/min komplekti lisamine galeriist
Questetra l/min komplekti integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Questetra l/min komplekti galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Questetra l/min komplekti lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Questetra l/min komplekti**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **Questetra l/min komplekti**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise Questetra l/min Suite nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Questetra l/min komplektis töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja seotud kasutaja Questetra l/min komplektis tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Questetra l/min komplektis väärtus **kasutaja nimi** väärtus.
 
Azure AD ühekordse sisselogimise Questetra l/min Suite testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise Questetra l/min komplekti testimiseks](#creating-a-questetra-bpm-suite-test-user)** – olema töölauafunktsioonid Britta Simon Questetra l/min komplekt, mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Questetra l/min komplekti rakenduse.

**Azure AD ühekordse sisselogimise Questetra l/min Suite konfigureerimiseks tehke järgmist.**

1. **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **Questetra l/min komplekti** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise konfigureerimine][8]

2. Lehel **Kuidas soovite kasutajad logida Questetra l/min komplekti** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][9]


3. Erinevate web brauseriaknas, logige sisse saidil **Questetra l/min komplekti** ettevõtte administraatorina.

4. Klõpsake menüü peal, **Süsteemisätetest**. 

    ![Azure'i AD ühekordse sisselogimise][10]

5. **SingleSignOnSAML** lehe avamiseks klõpsake **SSO (SAML)**. 

    ![Azure'i AD ühekordse sisselogimise][11]


6. Azure'i klassikaline portaalis lehel **Rakenduse sätete konfigureerimine** dialoogiboksis tehke järgmist: 

    ![Rakenduse sätete konfigureerimine][13]
 
    lisamine. Sisse **Questetra l/min komplekti** ettevõtte saidi jaotises SP teave, mida **ACS URL-i**kopeerida, ja seejärel kleepige **Logi sisse URL-i** tekstiväli.

    b. Sisse **Questetra l/min komplekti** ettevõtte saidil SP teave jaotises kopeerite **Üksuse ID**, ja seejärel kleepige **URL väljaandja** tekstiväli.

    c. Sisse **Questetra l/min komplekti** ettevõtte saidil SP teave jaotises kopeerite **ACS URL-i**, ja seejärel kleepige tekstiväljale **Vastus URL-i** .

    d. Klõpsake nuppu **edasi**.

 
7. Lehel **konfigureerimine ühekordse sisselogimise Questetra l/min komplekti juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine][14]


8. Teile **Questetra l/min komplekti** ettevõtte saidil, tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][15]

    lisamine. Valige **ühekordse sisselogimise lubamiseks**.
     
    b. Azure klassikaline portaalis, **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **Üksuse ID** tekstiväli.

    c. Azure'i klassikaline portaalis, kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige see **sisselogimise lehe URL-i** tekstiväli.

    d. Azure'i klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **väljunud lehe URL-i** tekstiväli.

    e. Tippige tekstiväljale **NameID vorming** **urn: oasis: nimed: tc: SAML:1.1:nameid-vorming: emailAddress**.


    f. Teie allalaaditud serdi base-64-kodeeritud faili loomine. 

    >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

    g. Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige need **valideerimine serdi** tekstiväli. 

    h. Klõpsake nuppu **Salvesta**.


9. Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Mis on Azure AD-ühenduse][17]


10. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Mis on Azure AD-ühenduse][18]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure AD testkasutaja loomine][100] 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure AD testkasutaja loomine][101] 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure AD testkasutaja loomine][102] 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure AD testkasutaja loomine][103]
 
    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.
  
    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu edasi.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure AD testkasutaja loomine][104] 
  
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**. 
 
    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure AD testkasutaja loomine][105]  

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure AD testkasutaja loomine][106]   
  
    lisamine. Kirjutage üles väärtus **Uus parool**.
  
    b. Klõpsake nuppu **valmis**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Questetra l/min komplekti testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Questetra l/min komplektis kasutaja loomiseks.

**Nimega Britta Simon Questetra l/min komplektis kasutaja loomiseks tehke järgmist:**

1.  Sisselogimise saidile Questetra l/min komplekti ettevõtte administraatorina.
2.  Minge **süsteemisätetest > Kasutaja loend > uus kasutaja**. 
3.  Klõpsake dialoogiboksis Uus kasutaja tehke järgmist. 

    ![Testkasutaja loomine][300] 

    lisamine. Tippige väljale **nimi** Azure AD Britta's kasutajanimi.

    b. Tippige tekstiväljale **e-posti** Azure AD Britta oma kasutajanimi.

    c. Tippige **parool** väljale parool.

4.  Klõpsake nuppu **Lisa uus kasutaja**.



### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Questetra l/min komplekti tema juurdepääsuõiguse andmise lubamine.

![Mis on Azure AD-ühenduse][200]

**Questetra l/min komplekti Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Mis on Azure AD-ühenduse][201]

2. Valige rakenduste loendis **Questetra l/min komplekti**.

    ![Mis on Azure AD-ühenduse][205]

1. Klõpsake menüü peal, **Kasutajad**.

    ![Mis on Azure AD-ühenduse][202]

1. Valige loendis kasutajate **Britta Simon**.

    ![Mis on Azure AD-ühenduse][203]

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Mis on Azure AD-ühenduse][204]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.  
Kui klõpsate paanil Accessi paani Questetra l/min komplekti, mida peaks saada automaatselt allkirjastatud-on Questetra l/min komplekti rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 