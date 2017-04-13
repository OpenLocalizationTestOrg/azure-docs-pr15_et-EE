<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine O.C. Tanner - AppreciateHub | Microsoft Azure'i"
    description="Siit saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja O.C. vahel Tanner - AppreciateHub."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Õpetus: Azure'i Active Directory integreerimine O.C. Tanner - AppreciateHub

Selle õpetuse eesmärk on näete, kuidas integreerida O.C. Tanner - AppreciateHub Azure Active Directory (Azure AD).  
O.C. integreerimine Tanner - AppreciateHub Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs O.C. Tanner - AppreciateHub 
- Saate lubada automaatselt saada allkirjastatud-on O.C. kasutajatele Tanner - AppreciateHub (ühekordse sisselogimise) oma Azure AD kontode
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Koos O.C. Azure AD integreerimise konfigureerimiseks Tanner - AppreciateHub, on teil vaja järgmist:

- Tellimuse Azure AD
- MÕNE O.C. Tanner - AppreciateHub ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. O.C. lisamine Tanner - AppreciateHub galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. lisamine Tanner - AppreciateHub galeriist
O.C. integreerimise konfigureerimiseks Tanner - AppreciateHub Azure AD sisse, peate lisama O.C. Tanner - AppreciateHub galeriist hallatavate SaaS rakenduste loendisse.

**Lisage O.C. Tanner - AppreciateHub galeriist, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1] 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2] 

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3] 

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4] 

6. Tippige otsinguväljale **O.C. Tanner - AppreciateHub**.

    ![Rakenduste][5] 

7. Tulemuste paanil valige **O.C. Tanner - AppreciateHub**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise

Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja Azure AD ühekordse sisselogimise koos O.C. testimine Tanner - AppreciateHub põhjal testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, millised töölauafunktsioonid kasutaja O.C. Tanner - AppreciateHub Azure AD kasutajale on. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud O.C. Tanner - AppreciateHub tuleb luua.  
Selle lingi seos on loodud määramine väärtus **kasutajanimi** **kasutajanimi** O.C. väärtusena Azure AD Tanner - AppreciateHub.
 
Konfigureerimine ja Azure AD ühekordse sisselogimise koos O.C. testimine Tanner - AppreciateHub, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Loomise O.C. Tanner - AppreciateHub testkasutaja](#creating-a-halogen-software-test-user)** - olema töölauafunktsioonid Britta Simon O.C. Tanner - AppreciateHub Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise oma O.C. Tanner - AppreciateHub rakendus.


**Konfigureerimiseks Azure AD ühekordse sisselogimise O.C. Tanner - AppreciateHub, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **O.C. Tanner - AppreciateHub** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6]

2. **Kuidas soovite kasutajad logida O.C. Tanner - AppreciateHub** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][7]

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Rakenduse sätete konfigureerimine][8]
 
     lisamine. Avage metaandmete faili, kasutades järgmist linki: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).

     b. Otsige üles sõlm **md:AssertionConsumerService** . 

     c. Kopeerige **asukoha** atribuudi väärtust. 

     ![Rakenduse sätete konfigureerimine][12]
     
     d. **Logige sisse URL** väljale viimase väärtuse olete saanud eelmises etapis.

     > [AZURE.NOTE] Kui olete expiriencing probleeme vastus URL-i metaandmete faili, pöörduge selle O.C. Tanner - AppreciateHub tugimeeskonna kaudu [sso@octanner.com](mailto:sso@octanner.com).

     e. Klõpsake nuppu **edasi**.
 
4. Lehel **Konfigureeri ühekordse sisselogimise O.C. Tanner - AppreciateHub juures** **metaandmete allalaadimine**nuppu ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Mis on Azure AD-ühenduse][9]

5. Pöörduge selle O.C. Tanner - AppreciateHub tugimeeskonna kaudu xyz, saatke neile metaandmete faili ja need saadame talle, peaks võimaldavad SSO teie eest.


6. Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Mis on Azure AD-ühenduse][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Mis on Azure AD-ühenduse][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.  

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Mõne O.C. loomine Tanner - AppreciateHub testkasutaja

Selle jaotise eesmärk on nimega Britta Simon O.C. kasutaja loomiseks Tanner - AppreciateHub.

**Nimega Britta Simon O.C. Tanner - AppreciateHub, kasutaja loomiseks tehke järgmist.**

1. Paluge tugimeeskonnale OC Tanner kui nameID atribuut sama väärtuse, mis kasutajanimi Britta Simon Azure AD kasutaja loomiseks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise O.C. tema juurdepääsuõiguse andmise lubamine Tanner - AppreciateHub.

![Kasutaja määramine][200]

**Määrake Britta Simon O.C. Tanner - AppreciateHub, tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **O.C. Tanner - AppreciateHub**.

    ![Kasutaja määramine][202]

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui klõpsate selle O.C. Tanner - AppreciateHub paani paanil Accessi te peaksite saada automaatselt sisse loginud-on teie O.C. Tanner - AppreciateHub rakendus.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






