<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine lamedam failide | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise lamedam failide ja Azure Active Directory vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Õpetus: Azure'i Active Directory integreerimine lamedam failid

Selle õpetuse eesmärk näitavad, kuidas lamedam failide integreerimine Azure Active Directory (Azure AD).  
Lamedam failide integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs lamedam failid 
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on lamedam failide (ühekordse sisselogimise)
- Saate hallata oma ühe keskse koha – klassikaline portaali Azure Active Directory kontod

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Azure AD integreerimise konfigureerimiseks lamedam failidega, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne lamedam failide ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Lamedam failide lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-flatter-files-from-the-gallery"></a>Lamedam failide lisamine galeriist
Lamedam failide integreerimine Azure AD konfigureerimiseks peate Galerii lamedam faile lisada hallatavate SaaS rakenduste loendit.

**Galeriist lamedam failide lisamiseks tehke järgmist:**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Lamedam failid**.


    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. Tulemuste paanil **Lamedam failide**valimine, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise põhjal nimega "Britta Simon" testkasutaja lamedam failidega.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD lamedam failides töölauafunktsioonid kasutaja. Teisisõnu, peab toimuma link seose Azure AD kasutaja ja seotud kasutaja lamedam failid.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** lamedam failide väärtus **kasutaja nimi** väärtus.
 
Konfigureerimine ja testimine Azure AD ühekordse sisselogimise lamedam failidega, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Lamedam failide loomine testida kasutaja](#creating-a-halogen-software-test-user)** - olema töölauafunktsioonid Britta Simon lamedam faile, mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure AD klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise lamedam failide rakenduse. Selle toimingu käigus saate vajalike base – 64 encoded serdi faili loomine. Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

Ühekordse sisselogimise lamedam failide jaoks konfigureerida, peate registreeritud domeeni. Kui teil pole registreeritud domeeni veel kontakti lamedam failide tugimeeskonna kaudu [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Konfigureerida Azure AD ühekordse sisselogimise lamedam failidega, tehke järgmist.**

1. Azure AD klassikaline portaalis lehel **Lamedam failide** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajate lamedam failide logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Lamedam failide kasutab sama SSO sisselogimise URL-i kõik kliendid: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Klõpsake lehel **Konfigureeri ühekordse sisselogimise lamedam faile** tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


1. Sisselogimise lamedam failide rakenduse administraatorina.

2. Valige armatuurlaud. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Klõpsake nuppu **sätted**ja seejärel menüü **ettevõte** tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    lisamine. Valige suvand **Kasuta SAML 2.0 autentimist**.

    b. Klõpsake **SAML konfigureerimine**.



2. Klõpsake dialoogiboksis **SAML konfiguratsiooni** tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    lisamine. Tippige tekstiväljale domeeni registreeritud domeeni.

    > [AZURE.NOTE] Kui teil pole registreeritud domeeni veel kontakti lamedam failide tugimeeskonna kaudu [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. Funktsiooni Azure'i klassikaline portaali sisse ühe konfigureerimine sisselogimine veebisaidil lamedam failide dialoogiboksis copt ühekordse sisselogimise teenuse URL-i ja seejärel kleepige URL-i identiteedi pakkuja tekstiväli.

    c.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

    >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    d.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **FlatterFiles identiteedi pakkuja serdi** tekstiväli.

    e. Klõpsake nuppu **Värskenda**.

6. Azure AD klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Testkasutaja lamedam failide loomine

Selle jaotise eesmärk on nimega Britta Simon lamedam failid kasutaja loomiseks.

**Nimega Britta Simon lamedam failid kasutaja loomiseks tehke järgmist:**

1. Logige saidile **Lamedam failide** ettevõtte administraatorina.

2. Klõpsake vasakpoolsel navigeerimispaanil nuppu **sätted**ja klõpsake **vahekaarti**kasutajad.

    ![Cfreate lamedam failide kasutaja](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Klõpsake nuppu **Lisa kasutaja**. 

4. Klõpsake dialoogiboksis **Lisa kasutaja,** tehke järgmist.

    ![Cfreate lamedam failide kasutaja](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.

    b. Tippige tekstiväljale **Perekonnanimi** **Simon**. 

    c. Tippige väljale **Meiliaadress** Azure klassikaline portaalis Britta isiku meiliaadress.

    d. Klõpsake **esitada**.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise lamedam failide tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Lamedam failide Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Lamedam failid**.

    ![Kasutaja määramine](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud lamedam failide paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on lamedam failide rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






