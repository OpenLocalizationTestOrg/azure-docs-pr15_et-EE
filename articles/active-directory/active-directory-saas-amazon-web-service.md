<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine koos Amazon Web Service (AWS) | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Amazon Web Services (AWS) Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Õpetus: Azure'i Active Directory integreerimine koos Amazon Web Service (AWS)

Selle õpetuse eesmärk näitavad, kuidas Amazon Web Service (AWS) integreerimine Azure Active Directory (Azure AD).  
Amazon Web Service (AWS) integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs Amazon Web Service (AWS) 
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on et Amazon Web Service (AWS) (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Azure AD integreerimise konfigureerimine koos Amazon Web Service (AWS), peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Amazon Web Service (AWS) ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. Amazon Web Service (AWS) lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Amazon Web Service (AWS) lisamine galeriist
Amazon Web Service (AWS) integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Amazon Web Service (AWS) galeriist hallatavate SaaS rakenduste loendisse.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Galeriist Amazon Web Service (AWS) lisamiseks tehke järgmist.

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1] 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** . 
   
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** . 
   
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**. 
   
    ![Rakenduste][4]

6. Tippige otsinguväljale **Amazon Web Service (AWS)**.
   
    ![Rakenduste][5]

7. Tulemuste paanil valige **Amazon Web Service (AWS)**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise koos Amazon Web Service (AWS) põhjal testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD töölauafunktsioonid kasutaja Amazon Web Service (AWS). Teisisõnu, peab toimuma link seose Azure AD kasutaja ja kasutajale seotud Amazon Web Service (AWS).  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Amazon Web Service (AWS) väärtus **kasutaja nimi** väärtus.
 
Konfigureerimine ja testimine Azure AD ühekordse sisselogimise koos Amazon Web Service (AWS), peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühe ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Amazon Web Service (AWS) loomise katse kasutaja](#creating-a-halogen-software-test-user)** - olema töölauafunktsioonid Britta Simon rakenduses Amazon Web Service (AWS) Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure'i AD ühe ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Amazon Web Service (AWS) rakenduse.  
Amazon Web Service (AWS) rakenduse loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine. Järgmine pilt kuvatakse järgmine näide.


![Ühekordse sisselogimise konfigureerimine][27]

**Konfigureerida Azure AD ühekordse sisselogimise koos Amazon Web Service (AWS), tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Amazon Web Service (AWS)** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7]

2. Lehel **Kuidas soovite kasutajad sisse logida Amazon Web Service (AWS)** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine][8]

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, klõpsake nuppu edasi. 

    ![Rakenduse sätete konfigureerimine][9]
 
4. Lehel **konfigureerimine ühekordse sisselogimise Amazon Web Service (AWS)** klõpsake nuppu **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine][10]

5. Erinevate brauseriaknas sisselogimise saidile Amazon Web Service (AWS) ettevõtte administraatorina.

6. Klõpsake **Konsooli Avaleht**.

    ![Ühekordse sisselogimise konfigureerimine][11]

7. Klõpsake **identiteedi ja juurdepääsu juhtimine**. 

    ![Ühekordse sisselogimise konfigureerimine][12]

8. Klõpsake **Identiteedipakkujad**ja seejärel klõpsake nuppu **Loo pakkuja**. 

    ![Ühekordse sisselogimise konfigureerimine][13]

9. Dialoogiboksi **Konfigureerimine pakkuja** lehel tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][14]

     lisamine. Valige **Pakkuja tüüp**, **SAML**.

     b. Tippige tekstiväljale **Pakkuja nimi** pakkuja nimi (nt: *puidu*).

     c. Metaandmete allalaaditud faili üleslaadimiseks klõpsake **Faili valimine**.

     d. Klõpsake **järgmise juhise juurde**.


10. Klõpsake lehel **Kinnitamine pakkuja teave** dialoogiboksi nuppu **Loo**. 

    ![Ühekordse sisselogimise konfigureerimine][15]

11. Klõpsake **rollid**ja seejärel klõpsake nuppu **Loo uus roll**. 

    ![Ühekordse sisselogimise konfigureerimine][16]

12. Klõpsake dialoogiboksis **Määrata rolli nimi** tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][17]

    lisamine. Tippige tekstiväljale **Rolli nimi** rolli nimi (nt: *TestUser*).

    b. Klõpsake **järgmise juhise juurde**.

13. Klõpsake dialoogiboksis **Rolli tüübi valimine** tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][18]

    lisamine. Valige **roll identiteedi pakkuja juurdepääsu**.

    b. Klõpsake jaotises **Anna Web ühekordse sisselogimise (WebSSO) juurdepääsu SAML pakkujad** **Valige**.


14. Klõpsake dialoogiboksis **Luua usaldada** tehke järgmist.  

    ![Ühekordse sisselogimise konfigureerimine][19]
     
     lisamine. SAML pakkuja, valige SAML pakkuja loodud previousley (nt: *puidu*) 

     b. Klõpsake **järgmise juhise juurde**.


15. Klõpsake dialoogiboksis **Kontrollimine usalduskeskuse roll** **Järgmise juhisega**. 

    ![Ühekordse sisselogimise konfigureerimine][32]


16. Klõpsake dialoogiboksis **Manustamine poliitika** **Järgmise juhisega**.  

    ![Ühekordse sisselogimise konfigureerimine][33]


17. Klõpsake dialoogiboksis **Läbivaatus** tehke järgmist.   

    ![Ühekordse sisselogimise konfigureerimine][34]

     lisamine. Kopeerige **Rolli ARN** väärtus.

     b. Kopeerige **Usaldusväärsed üksuste** ARN väärtus.

     c. Klõpsake nuppu **Loo roll**. 

18. Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**.

    ![Mis on Azure AD-ühenduse][20]

19. **Ühekordse sisselogimise kinnitamise** lehel nuppu **Konfigureeri ühekordse sisselogimise** dialoogiboksi sulgemiseks **lõpuleviimine** .

    ![Mis on Azure AD-ühenduse][22]


20. Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** . 

    ![Ühekordse sisselogimise konfigureerimine][21]

21. Klõpsake nuppu **Lisa kasutaja atribuut**. 

    ![Ühekordse sisselogimise konfigureerimine][23]

22. Klõpsake dialoogiboksis Lisa kasutaja atribuut tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][24] 

    lisamine. Tippige väljale **Atribuudi nimi** **https://aws.amazon.com/SAML/Attributes/Role**.

    b. **Atribuudi väärtus** tekstivälja, tippige **[rolli ARN väärtus], [usaldusväärsed üksuse ARN väärtus]**.

    >[AZURE.TIP] Need on läbivaatus dialoogiboksi kopeerimist, kui olete loonud oma rolli väärtused. 

    c. Klõpsake nuppu **täielik** **Lisada kasutaja atribuutide** dialoogiboksi sulgemiseks.

23. Klõpsake nuppu **Lisa kasutaja atribuut**. 

    ![Ühekordse sisselogimise konfigureerimine][23]


24. Klõpsake dialoogiboksis Lisa kasutaja atribuut tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][25]


     lisamine. Tippige väljale **Atribuudi nimi** **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. **Atribuudi väärtus** tekstivälja, tippige või valige rippmenüüst **user.userprincipalname** .
     
    ![Ühekordse sisselogimise konfigureerimine][35]
    

     c. Klõpsake nuppu **täielik** **Lisada kasutaja atribuutide** dialoogiboksi sulgemiseks.


25. Klõpsake nuppu **Rakenda muudatused**. 

    ![Ühekordse sisselogimise konfigureerimine][26]





### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine

Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.

![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Kasutaja tüüp, valige ettevõttes uue kasutaja.
  2. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.
  3. Klõpsake nuppu edasi.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Klõpsake soovitud **Perekonnanimi** txtbox, tüüp, **Simon**.
  
    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
  
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.
  
    b. Klõpsake nuppu **valmis**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Amazon Web Service (AWS) testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Amazon Web Service (AWS) kasutaja loomiseks.

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Nimega Britta Simon Amazon Web Service (AWS) kasutaja loomiseks tehke järgmist.

1. Logige saidile **Amazon Web Service (AWS)** ettevõtte administraatorina.

2. Klõpsake ikooni **Konsooli Avaleht** . 

    ![Ühekordse sisselogimise konfigureerimine][11]

3. Klõpsake nuppu identiteedi ja juurdepääsu juhtimine. 

    ![Ühekordse sisselogimise konfigureerimine][28]

4. Armatuurlaua, klõpsake linki kasutajad ja klõpsake käsku Loo uus kasutajad. 

    ![Ühekordse sisselogimise konfigureerimine][29]

5. Klõpsake dialoogiboksis kasutaja loomiseks tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine][30]

     lisamine. **Sisestage kasutajanimed** tekstiväljade, tippige Azure AD Brita Simon kasutajanimi (userprincipalname).

     b. Klõpsake nuppu **Loo**.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon kasutada Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise Amazon Web Service (AWS).

![Kasutaja määramine][31]

**CloudPassage Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][26]

2. Valige rakenduste loendis **Amazon Web Service (AWS)**.

    ![Kasutaja määramine][27]

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][25]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][29]

### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.  
Kui klõpsate paanil Accessi paani Amazon Web Service (AWS), mida peaks saada automaatselt allkirjastatud-on Amazon Web Service (AWS) rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















