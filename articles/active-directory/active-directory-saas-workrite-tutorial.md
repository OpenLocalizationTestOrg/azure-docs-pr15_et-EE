<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Workrite | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Workrite vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-workrite"></a>Õpetus: Azure'i Active Directory integreerimine Workrite

Selle õpetuse eesmärk näitavad, kuidas Workrite integreerimine Azure Active Directory (Azure AD).  
Workrite integreerimine Azure AD annab teile järgmised eelised: 


- Saate määrata Azure AD, kellel on juurdepääs Workrite 
- Saate lubada automaatselt saada allkirjastatud-on Workrite (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Konfigureerimiseks integreerimine Azure AD Workrite, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Workrite ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. Workrite lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-workrite-from-the-gallery"></a>Workrite lisamine galeriist
Workrite integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Workrite galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Workrite lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 
 
    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.
 
    ![Rakenduste][4]

6. Tippige otsinguväljale **Workrite**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **Workrite**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Workrite nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Workrite töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Workrite tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Workrite väärtus **kasutaja nimi** väärtus.
 
Azure AD ühekordse sisselogimise koos Workrite testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Workrite testida](#creating-a-halogen-software-test-user)** - olema töölauafunktsioonid Britta Simon Workrite Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Workrite rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise Workrite, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Workrite** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Workrite** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][7] 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.
    
    ![Azure'i AD ühekordse sisselogimise][8] 
 
     lisamine. Tippige tekstiväljale **Logi sisse URL-i** kasutavad kasutajatele Workrite saidile sisselogimise URL-i (nt: *https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=1a82b5aa-4dd6-4472-9721-7d0193f59e22*).

     > [AZURE.NOTE] Pöörduge oma tugimeeskonna Workrite [support@workrite.co.uk](mailto:support@workrite.co.uk) kui te ei tea väärtus URL Logi sisse.

     b. Klõpsake nuppu **edasi**.
 
4. Lehel **Konfigureeri ühekordse sisselogimise Workrite veebisaidil** , tehke järgmist:

    ![Azure'i AD ühekordse sisselogimise][9] 

    lisamine. Klõpsake nuppu Laadi alla serdi ja seejärel salvestage fail oma arvutis.

    b. Pöörduge oma tugimeeskonna Workrite [support@workrite.co.uk](mailto:support@workrite.co.uk), peovide neile selle allalaaditud serdi **Väljaandja URL-i** (üksuse ID), **Ühekordse sisselogimise teenuse URL-i**, **Ühe Sign-Out URL-i**, ja paluge neil oma Workrite rakenduse häälestamine SSO-d. 

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

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_09.png)  

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_05.png)  

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-workrite-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-workrite-test-user"></a>Workrite testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Workrite kasutaja loomiseks.

**Nimega Britta Simon Workrite kasutaja loomiseks tehke järgmist.**

1. Logige saidile workrite ettevõtte administraatorina.

2. Klõpsake navigeerimispaanil **administraator**.

    ![Kasutaja määramine][400]

3. Minge Kiirlingid ja seejärel klõpsake nuppu **Loo kasutaja**. 

    ![Kasutaja määramine][401]

4. Klõpsake dialoogiboksis **Kasutaja loomiseks** tehke järgmist.

    ![Kasutaja määramine][402]

    lisamine. Tippige **e-posti**, **eesnimi** ja **perekonnanimi** lubatud Azure AD kasutaja, mida soovite ette valmistada.

    b. **Valige roll**valige **Kliendi administraator** . 

    c. Klõpsake nuppu **Salvesta**.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Workrite tema juurdepääsuõiguse andmise lubamine.

    ![Assign User][200] 

**Workrite Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Workrite**.

    ![Kasutaja määramine][202] 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 
1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud Workrite paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Workrite rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_01.png
[500]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_05.png

[6]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_02.png
[8]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_03.png
[9]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_04.png
[10]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_07.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png




