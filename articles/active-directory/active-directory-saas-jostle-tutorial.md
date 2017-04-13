<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine rüselemine | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja rüselemine vahel."
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
    ms.date="10/14/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-jostle"></a>Õpetus: Azure'i Active Directory integreerimine rüselemine

Selles õppetükis saate teada, kuidas integreerida rüselemine Azure Active Directory (Azure AD).

Integreerimise rüselemine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs rüselemine
- Saate lubada automaatselt saada allkirjastatud-on rüselemine (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks koos rüselemine vajate järgmist:

- Tellimuse Azure AD
- Mõne **rüselemine** ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Rüselemine lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-jostle-from-the-gallery"></a>Rüselemine lisamine galeriist
Integreerimine rüselemine Azure AD konfigureerimiseks tuleb kõigepealt lisada rüselemine galeriist hallatavate SaaS rakenduste loendisse.

**Lisada rüselemine Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **rüselemine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_01.png)

7. Tulemuste paanil valige **rüselemine**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise rüselemine nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis rüselemine töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud rüselemine tuleb luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** rüselemine väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise koos rüselemine, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise on rüselemine testimiseks](#creating-a-jostle-test-user)** – olema töölauafunktsioonid Britta Simon rüselemine Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise rüselemine rakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise rüselemine, tehke järgmist.**

1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine][6]

2. Klassikaline portaalis lehel **rüselemine** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7] 

3. **Kuidas kas soovite kasutajad logida rüselemine** lehel Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_06.png)

4. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_07.png)


    lisamine. Tippige tekstiväljale Logi sisse URL-i URL-i abil järgmist mustrit:`https://<subdomain>.jostle.us/jostle-prod/`

    b. Klõpsake nuppu **edasi**.

5. Klõpsake soovitud **konfigureerimine ühekordse sisselogimise veebisaidil rüselemine** lehel, klõpsake käsku **metaandmete alla laadida**, ja seejärel salvestage fail oma arvutis.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_08.png)

6. Rakenduse jaoks konfigureeritud SSO saamiseks võtke ühendust teie haldur rüselemine või tugiteenuste rüselemine. Need aitavad konfigureerida SSO õige kanal. Palun pange tähele, et peate meilisõnumeid saata ja manustage allalaaditud faili metaandmete <support@jostle.me>.

7. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jostle-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-jostle-test-user"></a>Rüselemine testkasutaja loomine

Selles jaotises nimega Britta Simon rüselemine kasutaja loomine. Kui te ei tea, kuidas lisada Britta Simon rüselemine, võtke ühendust lisada testkasutaja ja lubada SSO rüselemine toe poole. Pöörduge neid <support@jostle.me>.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon tema juurdepääsuõiguse andmise rüselemine Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Rüselemine Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **rüselemine**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kõik kasutajad **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui klõpsate paanil Accessi rüselemine paani, mida peaks saada automaatselt allkirjastatud-on rüselemine rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-jostle-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_205.png
