<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Novatus | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja LearnUpon vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Õpetus: Azure'i Active Directory integreerimine LearnUpon

Selle õpetuse eesmärk näitavad, kuidas LearnUpon integreerimine Azure Active Directory (Azure AD).  
LearnUpon integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs LearnUpon
- Saate lubada automaatselt saada allkirjastatud-on LearnUpon (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata oma kontosid ühes keskses kohas – Azure Active Directory klassikaline 


Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD LearnUpon, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne LearnUpon ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. LearnUpon lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-learnupon-from-the-gallery"></a>LearnUpon lisamine galeriist
LearnUpon integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada LearnUpon galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist LearnUpon lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **LearnUpon**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. Tulemuste paanil valige **LearnUpon**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise LearnUpon nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD LearnUpon töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud LearnUpon tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** LearnUpon väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos LearnUpon testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne LearnUpon testida](#creating-a-learnupon-test-user)** - olema töölauafunktsioonid Britta Simon LearnUpon Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise LearnUpon rakenduse.



**Konfigureerida Azure AD ühekordse sisselogimise LearnUpon, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **LearnUpon** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida LearnUpon** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png) 


    lisamine. Tippige tekstiväljale **Vastus URL-i** kinnituse tarbija teenuse URL, kasutades järgmist mustrit:`https://\<companyname\>.learnupon.com/saml/consumer`

    b. Klõpsake nuppu **edasi**. 


4. Lehel **Konfigureeri ühekordse sisselogimise LearnUpon veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis. Meil vaja seda serti ja metaandmete URL-id (üksuse ID, SSO sisselogimise URL ja Logi välja URL-i) häälestamine SSO LearnUpon küljel.

    b. Klõpsake nuppu **edasi**.




1. Avage teise brauseri eksemplar ja logige LearnUpon sisse administraatorikontoga. 

1. Klõpsake vahekaarti **sätted** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png) 


1. **Ühekordse sisselogimise - SAML**klõpsake ja valige **Üldsätted** SAML sätete konfigureerimiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 


5. Jaotises **Üldsätted** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png) 

    lisamine. Valige **lubatud**.

    b. Kui **versioon**, valige **2.0**.

    c. **Jäta tingimused**, valige **ei**.

    d. **SAML Turbeloa postitada param nimi** tekstiväljale tippige selle kohal märgitud taotluse postituse parameeter SAML tarbija URL-i nimi sisaldab SAML kinnituse kinnitatud ja autenditud – näiteks **SAMLResponse**.

    e. Tippige tekstiväljale **Nimevormingut identifikaator** väärtus, mis näitab, et teie SAML väide on kasutajate identifikaator (meiliaadress) asukoht – näiteks **urn: oasis: nimed: tc: SAML:1.1:nameid-vorming: emailAddress**.

    f. Tippige tekstiväljale **Tuvastamine pakkuja asukoha** väärtus, mis näitab, kuhu kasutajad saadetakse kui nad klõpsavad teie üleslaaditud ikooni oma Azure klassikaline portaali sisselogimine.

    g.in Azure klassikaline portaali, kopeerige **Ühe Sign-Out teenuse URL-i**ja seejärel kleepige **URL-i välja logida** textbos.

    h. Klõpsake nuppu **Halda sõrmega prinditakse**ja laadige allalaaditud serdi sõrmega Prindi. 


1. Klõpsake **Kasutaja sätted**ja seejärel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png) 

    lisamine. **Eesnimi identifikaator Vorminda** tekstivälja, tippige väärtus, mis ütleb, on teie SAML väide kasutaja eesnimi asukoht – näiteks: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ givenname**.

    b. Tippige tekstiväljale **Viimase nimevormingut identifikaator** väärtus, mis ütleb, on teie SAML väide kasutajate perekonnanimi asukoht – näiteks: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ perekonnanimi**.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-learnupon-test-user"></a>LearnUpon testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon LearnUpon kasutaja loomiseks. LearnUpon toetab just in time ettevalmistamise, mis on vaikimisi sisse lülitatud.

On teile selle jaotise pole toimingu toode. LearnUpon juurde, kui see pole veel olemas katse ajal luuakse uus kasutaja. [Azure'i AD ühekordse sisselogimise konfigureerimine](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate pöörduma LearnUpon tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise LearnUpon tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**LearnUpon Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **LearnUpon**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud LearnUpon paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on LearnUpon rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png
