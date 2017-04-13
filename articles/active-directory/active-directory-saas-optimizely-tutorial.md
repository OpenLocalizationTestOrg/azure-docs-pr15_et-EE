<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Optimizely | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Optimizely vahel."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Õpetus: Azure'i Active Directory integreerimine Optimizely

Selles õppetükis saate teada, kuidas Optimizely integreerimine Azure Active Directory (Azure AD).

Optimizely integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Optimizely
- Saate lubada automaatselt saada allkirjastatud-on Optimizely (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Optimizely, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne **Optimizely** ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Optimizely lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-optimizely-from-the-gallery"></a>Optimizely lisamine galeriist
Optimizely integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Optimizely galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Optimizely lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Optimizely**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. Tulemuste paanil valige **Optimizely**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Optimizely nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis Optimizely töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Optimizely tuleb luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Optimizely väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Optimizely testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Mõne Optimizely loomise katse kasutaja](#creating-an-optimizely-test-user)** - olema töölauafunktsioonid Britta Simon Optimizely Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Optimizely rakenduse.

Optimizely rakendus eeldab SAML kinnitused sisaldama atribuut nimega "e-posti". "E-posti" väärtus peab olema Optimizely tuvastas meilisõnum, mille saate hankida autenditud Azure AD. Konfigureerige see rakendus "e-posti" nõue. Nende atribuutide väärtused saate hallata rakenduse menüüs **"Atrributes"** . Järgmine pilt kuvatakse järgmine näide. 


![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Konfigureerida Azure AD ühekordse sisselogimise Optimizely, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Optimizely** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.
     
    ![Ühekordse sisselogimise konfigureerimine][5]

2. Klõpsake dialoogiboksis SAML Turbeloa atribuute lisada "e-posti" atribuuti.

    lisamine. Klõpsake **kasutaja atribuutide lisamine** **Lisada kasutaja atribuutide** dialoogiboksi avamiseks. 
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. Tippige väljale **Atribuudi nimi** atribuudi nimi "meilisõnum".

    c. Valige loendist **Väärtus** atribuudi väärtust "userprincipalname" või mis tahes väärtus, mis sisaldab e-posti tuvasta Azure AD ja Optimizely.

    d. Klõpsake nuppu **valmis**.
3. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine][6]
4. Klassikaline portaalis lehel **Optimizely** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7] 

5. **Kuidas soovite kasutajad logida Optimizely** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    lisamine. Tippige tekstiväljale **Logi sisse URL** :`https://app.optimizely.net/contoso`

    b. Tippige tekstiväljale **identifikaator** :`urn:auth0:optimizely:contoso`

    c. Klõpsake nuppu **edasi**. 


    > [AZURE.NOTE] **Logige sisse URL-i** ja **ID** väärtused on ainult tegelike väärtuste kohatäited. Leiate juhised kaks tegelike väärtuste põhjal Optimizely hiljem selles õpetuses.

7. Lehel **Konfigureeri ühekordse sisselogimise Optimizely veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. **Ühekordse sisselogimise teenuse URL-i**kopeerimine

8. Rakenduse jaoks konfigureeritud SSO, võtke ühendust oma Optimizely kontohaldur ja sisestage järgmine teave:

    - Teie allalaaditud serdi 
    - Ühekordse sisselogimise teenuse URL-i
 
    Oma e-posti vastuseks Optimizely annab teile Logi sisse URL (SP algatatud SSO) ja väärtused identifikaator (teenuse pakkuja üksuse ID).

9. Minge tagasi lehele **Rakenduse sätete konfigureerimine** dialoogiboksi ja seejärel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** **SP algatatud SSO URL-i** Optimizely järgi.

    b. Tippige tekstiväljale **identifikaator** **teenuse pakkuja üksuse ID** Optimizely järgi.

    c. Klõpsake nuppu **edasi**.

10. Lehel **Konfigureeri ühekordse sisselogimise Optimizely veebisaidil** , tehke järgmist:
    
    ![Azure'i AD ühekordse sisselogimise][10]

    lisamine. Valige kinnituse ühekordse sisselogimise konfigureerimine.

    b. Klõpsake nuppu **edasi**.

11. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]

12. Erinevate brauseriaknas sisselogimise Optimizely rakenduse.
13. Teie konto nimi **Konto sätted**paremas ülanurgas ja seejärel klõpsake nuppu.

    ![Azure'i AD ühekordse sisselogimise](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Märkige ruut **Luba SSO** ühekordse sisselogimise jaotises **Ülevaade** jaotises vahekaarti konto.

    ![Azure'i AD ühekordse sisselogimise](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.
Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-optimizely-test-user"></a>Loomise rakendust Optimizely test

Selles jaotises nimega Britta Simon Optimizely kasutaja loomine.

1. Valige avalehel **kaastöötajate** menüü
2. Klõpsake nuppu **Uus kaastööline** lisada uue kaastööline projekt.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Sisestage meiliaadress ja rolli määrata. Klõpsake nuppu **Kutsu**.


    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Nad saavad e-posti kutsuda. E-posti aadressiga. neil Optimizely sisse logida.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon Optimizely tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Optimizely Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Optimizely**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kõik kasutajad **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud Optimizely paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Optimizely rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
