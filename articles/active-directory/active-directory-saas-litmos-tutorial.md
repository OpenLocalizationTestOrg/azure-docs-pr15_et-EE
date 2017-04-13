<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Litmos | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Litmos vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Õpetus: Azure'i Active Directory integreerimine Litmos

Selle õpetuse eesmärk näitavad, kuidas Litmos integreerimine Azure Active Directory (Azure AD).  
Litmos integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs Litmos 
- Saate lubada automaatselt saada allkirjastatud-on Litmos (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata oma ühe keskse koha – Azure Active Directory kontod 

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Konfigureerimiseks integreerimine Azure AD Litmos, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Litmos ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. Litmos lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-litmos-from-the-gallery"></a>Litmos lisamine galeriist
Litmos integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Litmos galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Litmos lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Litmos**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **Litmos**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Litmos nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Litmos töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Litmos tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Litmos väärtus **kasutaja nimi** väärtus.
 
Azure AD ühekordse sisselogimise koos Litmos testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Litmos testida](#creating-a-halogen-software-test-user)** - olema töölauafunktsioonid Britta Simon Litmos Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure AD klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Litmos rakenduse.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

Osana konfiguratsiooni, peate kohandada Litmos rakenduse **SAML Turbeloa atribuute** .  

![Azure'i AD ühekordse sisselogimise][17] 

**Konfigureerida Azure AD ühekordse sisselogimise Litmos, tehke järgmist.**

1. Azure AD klassikaline portaalis lehel **Litmos** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Litmos** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
 
    ![Azure'i AD ühekordse sisselogimise][7] 


1. Sisselogimise Litmos ettevõtte saidil (nt: *https://azureapptest.litmos.com/account/Login*) administraatorina.

    ![Azure'i AD ühekordse sisselogimise][21] 


1. Klõpsake vasakul navigeerimisribal **kontod**.

    ![Azure'i AD ühekordse sisselogimise][22] 


1. Klõpsake vahekaarti **integratsioon** .

    ![Azure'i AD ühekordse sisselogimise][23] 


1. Klõpsake menüü **integratsioon** Kerige allapoole suvandini **3 tootja integratsioon**, ja seejärel vahekaarti **SAML 2.0** .

    ![Azure'i AD ühekordse sisselogimise][24] 

1. Kopeerige jaotises väärtus **on The SAML endoiint litmos jaoks:**.

    ![Azure'i AD ühekordse sisselogimise][26] 


3. Azure'i klassikaline portaalis lehel **Rakenduse sätete konfigureerimine** dialoogiboksis tehke järgmist:

    ![Azure'i AD ühekordse sisselogimise][8] 
 
    lisamine. Tippige tekstiväljale **identifikaator** kasutavad kasutajad sisselogimise Litmos rakenduse URL (nt: *https://azureapptest.litmos.com/account/Login*).
     
    b. Kleepige tekstiväljale **Vastus URL-i** Litmos rakendusest eelmises etapis kopeeritud väärtus.

    c. Klõpsake nuppu **edasi**.
 
4. Lehel **Konfigureeri ühekordse sisselogimise Litmos veebisaidil** , tehke järgmist:

    ![Azure'i AD ühekordse sisselogimise][2] 

    lisamine. Klõpsake nuppu Laadi alla serdi ja seejärel salvestage fail oma arvutis.


1. Oma **Litmos** rakenduse, tehke järgmist.

    ![Azure'i AD ühekordse sisselogimise][25] 

    lisamine. Klõpsake nuppu **Luba SAML**.

    b. Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

    >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    c. Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see siis tekstivälja **SAML x.509 vastav sert** .

    d. Klõpsake nuppu **Salvesta muudatused**.


6. Azure AD klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]


20. Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** . 

    ![Ühekordse sisselogimise konfigureerimine][12]


24. Klõpsake dialoogiboksis **Lisa kasutaja atribuut** tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][14]

  	| Atribuudi nimi | Atribuudi väärtus |
  	| ---            | ---             |
  	| E-posti          | User.mail       |
  	| Eesnimi      | User.givenName  |
  	| Perekonnanimi       | User.surname    |

    Iga andmete rea ülaltoodud tabelis, tehke järgmist.
   
    lisamine. Klõpsake nuppu **Lisa kasutaja atribuut**. 

    ![Ühekordse sisselogimise konfigureerimine][15]


    lisamine. Tippige väljale **Atribuudi nimi** selle rea kuvatud **Atribuudi nimi** .

    b. Valige selle rea näidatud **Atribuudi väärtust** .

    c. Klõpsake nuppu **täielik** **Lisada kasutaja atribuutide** dialoogiboksi sulgemiseks.


25. Klõpsake nuppu **Rakenda muudatused**. 

    ![Ühekordse sisselogimise konfigureerimine][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.  

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i clasic portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Litmos testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Litmos kasutaja loomiseks.  
Litmos rakendus toetab Just ajal ettevalmistamise. See tähendab, et kasutajakonto luuakse automaatselt vajadusel rakenduse kasutamine Accessi juhtpaneeli avamiseks katse ajal.

**Nimega Britta Simon Litmos kasutaja loomiseks tehke järgmist.**


1. Sisselogimise Litmos ettevõtte saidil (nt: *https://azureapptest.litmos.com/account/Login*) administraatorina.

    ![Azure'i AD ühekordse sisselogimise][21] 


1. Klõpsake vasakul navigeerimisribal **kontod**.

    ![Azure'i AD ühekordse sisselogimise][22] 


1. Klõpsake vahekaarti **integratsioon** .

    ![Azure'i AD ühekordse sisselogimise][23] 


1. Klõpsake menüü **integratsioon** Kerige allapoole suvandini **3 tootja integratsioon**, ja seejärel vahekaarti **SAML 2.0** .

    ![Azure'i AD ühekordse sisselogimise][24] 

1. Valige **tabelisse kasutajad:**.

    ![Azure'i AD ühekordse sisselogimise][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Litmos tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Litmos Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Litmos**.

    ![Kasutaja määramine][202] 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud Litmos paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Litmos rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





