<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine sellele Server | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja sellele serveri vahel."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Õpetus: Azure'i Active Directory sellele serveri integreerimine

Selle õpetuse eesmärk näitavad, kuidas sellele serveri integreerimine Azure Active Directory (Azure AD).

Sellele serveri integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs sellele Server
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on sellele server (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks sellele serveris peate järgmised üksused:

- Tellimuse Azure AD
- On sellele serveri ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Sellele serveri lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-tableau-server-from-the-gallery"></a>Sellele serveri lisamine galeriist
Sellele serveri integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada sellele serveri galeriist hallatavate SaaS rakenduste loendisse.

**Lisada sellele serveri Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 
 
    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Sellele Server**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. Tulemuste paanil valige **Sellele Server**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise nimega "Britta Simon" testkasutaja vastavalt sellele serveriga.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD sellele serveri töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja sellele serveri seotud kasutaja peab toimuma.

Selle lingi seos on loodud määramine Azure AD väärtus sellele serveri **kasutajanimi** **kasutajanimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise sellele serveriga, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise sellele Server testimiseks](#creating-a-tableauserver-test-user)** – olema töölauafunktsioonid Britta Simon sellele Server, mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise sellele Server rakenduse.

Sellele serverirakenduse loodab SAML kinnitused kindlas vormingus. Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Konfigureerida Azure AD ühekordse sisselogimise sellele serveriga, tehke järgmist.**


1. Azure'i klassikaline portaalis lehel **Sellele Server** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. **SAML Turbeloa atribuutide** dialoogiboksis tehke järgmist.

    

    lisamine. Klõpsake **kasutaja atribuutide lisamine** **Lisada kasutaja Attribure** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. Tippige tekstiväljale **Attrubute nimi nimi** , **kasutajanimi**.

    c. **Atribuudi väärtus** loendis selsect **user.displayname**.

    d. Klõpsake nuppu **valmis**.  
    



1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Klõpsake nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 



2. Lehel **Kuidas soovite kasutajatele sellele serverisse sisse logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi tegema järgmised toimingud ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    lisamine. Tippige väljale **Sisselogimise URL-i** sellele serveri URL. 

    b. Identifikaator väljale Kopeeri soovitud 

    c. Klõpsake nuppu **Järgmine**


4. Lehel **Konfigureeri ühekordse sisselogimise sellele server** järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


6. Saada SSO rakenduse jaoks konfigureeritud, peate oma sellele serveri rentniku administraatorina sisselogimise.

    lisamine. Sellele serveri konfiguratsiooni, klõpsake vahekaarti **SAML** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Valige märkeruut **Kasuta SAML ühekordse sisselogimise jaoks**.

    c. Leidke Federation metaandmete Azure klassikaline portaalist allalaaditud faili ja selle **SAML Idp metaandmete faili**üleslaadimiseks.

    d. Sellele Server tagasi URL-i – mis sellele serveri kasutaja saab juurdepääsu, nt http://tableau_server URL. Kasutades http://localhost ei ole soovitatav. URL-i abil lõpunullid kaldkriips (nt http://tableau_server/) ei toetata. Kopeerige **sellele Server tagasi URL** ja kleepige see Azure AD **Logi sisse URL-i** tekstiväli, nagu on näidatud sammus 3

    e. SAML üksuse ID – üksuse ID tuvastab kordumatult teie soovitud IdP sellele Serveri install. Saate sisestada sellele serveri URL-i uuesti, kui soovite, kuid see ei pea olema sellele serveri URL-i. **SAML üksuse ID** kopeerige ja kleepige see Azure AD **IDENTIFER** tekstiväli, nagu on näidatud sammus 3.

    f. Klõpsake **Metaandmete eksportimine faili** ja avage see rakenduses teksti editor. Leidke kinnituse tarbija teenuse URL-i Http postitus ja Index 0 ja kopeerige URL. Nüüd kleepige see Azure AD **Vastus URL-i** tekstiväli nagu on näidatud sammus 3. 

    g. Klõpsake lehel sellele Server Configiuration nuppu **OK** .

    > [AZURE.NOTE] Kui vajate abi SAML sellele serveris seejärel vaadake artiklit [SAML konfigureerimine](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**. 
 
    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.

    b. Tippige väljale **Kasutajanimi** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-tableau-server-test-user"></a>Sellele serveri testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon sellele serveri kasutaja loomiseks. Peate sellele server kõigi kasutajate ettevalmistamine. Samuti võtke arvesse, et kasutaja kasutajanime peaksid olema samad väärtus, mille olete konfigureerinud Azure AD kohandatud atribuut **kasutajanimi**. Õige vastenduse peaks integreerimine [Azure AD seadistamine ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)töötada.

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate ettevõtte sellele serveriadministraatori poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon talle juurdepääsu andmine sellele Server Azure'i ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Sellele serveri Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
 
    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Sellele Server**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud sellele serveri juurdepääsu paanil, saate tuleks saada automaatselt allkirjastatud-on sellele Server rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
