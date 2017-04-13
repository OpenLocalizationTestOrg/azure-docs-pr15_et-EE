<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ja ausalt | Microsoft Azure'i"
    description="Siit saate teada, kuidas konfigureerida ühekordse sisselogimise vahel Azure Active Directory ja ja ausalt."
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
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Õpetus: Azure'i Active Directory integreerimine ja ausalt

Selle õpetuse eesmärk on näete, kuidas integreerida & ausalt Azure Active Directory (Azure AD).

Integreerimine ja ausalt Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs & ausalt
- Saate lubada kasutajate saada automaatselt sisse logitud, & ausalt (ühekordse sisselogimise) oma Azure AD kontode
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerida Azure AD integreerimine ja ausalt, peate järgmised üksused:

- Tellimuse Azure AD
- A & ausalt ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Lisamine ja ausalt galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-frankly-from-the-gallery"></a>Lisamine ja ausalt galeriist
Integreerimine & ausalt üheks Azure AD konfigureerimiseks tuleb kõigepealt lisada ja ausalt galeriist hallatavate SaaS rakenduste loendit.

**Lisamine ja ausalt galeriist, tehke järgmist:**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
    
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
    
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **ja ausalt**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. Tulemuste paanil valige **ja ausalt**ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selle jaotise eesmärk on näidata konfigureerimine ja testi Azure AD Ühekordne sisselogimine ja ausalt vastavalt testi kasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja & ausalt rakendust Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud & ausalt tuleb luua.

Selle lingi seos on loodud määramine **kasutaja nimi** väärtus Azure AD **kasutajanimi** väärtusena sisse ja ausalt.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise koos ja ausalt, mida vaja täita järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Loomise lisamine ja ausalt testkasutaja](#creating-a-&frankly-test-user)** – olema töölauafunktsioonid Britta Simon & ausalt mis on seotud Azure AD kujutis tema.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise sisse oma & ausalt rakenduse.

**Konfigureerida Azure AD Ühekordne sisselogimine ja ausalt, tehke järgmist:**

1. Klassikaline portaalis lehel **& ausalt** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajatele & ausalt sisse logida** , valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi kui soovite konfigureerida rakendus **IDP algatatud režiimi**, tehke järgmist ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    lisamine. Tippige tekstiväljale **identifikaator** URL-i abil järgmist mustrit:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. Tippige tekstiväljale **Vastus URL-i** URL-i abil järgmist mustrit:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c. Klõpsake nuppu **Järgmine**

4. Kui soovite konfigureerida rakendus **SP algatatud režiimi** lehel **Rakenduse sätete konfigureerimine** dialoogiboksi, klõpsake **"Kuva täpsemad sätted (valikuline)"** ja sisestage **Logi sisse URL** ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i abil järgmist mustrit:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Klõpsake nuppu **Järgmine**

    > [AZURE.NOTE] Pange tähele, et need ei ole tegeliku väärtuse. Teil on need väärtused tegelik Logi sisse URL-i, identifikaator ja vastus URL-i värskendamine. Kontakti [help@andfrankly.com](emailTo:help@andfrankly.com) saada need väärtused.

5. Klõpsake soovitud **ühekordse sisselogimise veebisaidil konfigureerimine ja ausalt** lehel tegema järgmised toimingud ja klõpsake nuppu **edasi**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.

6. Rakenduse jaoks konfigureeritud SSO saamiseks võtke ühendust oma & ausalt tugimeeskonna kaudu [help@andfrankly.com](emailTo:help@andfrankly.com). Metaandmete allalaaditud faili manustamine ja selle abil ühiskasutusse anda ja häälestada oma küljele SSO ausalt meeskonnatöö.

7. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-frankly-test-user"></a>Loomise lisamine ja ausalt testkasutaja

Selles jaotises nimega Britta Simon & ausalt kasutaja loomine. Tehke koostööd ja ausalt tugimeeskonna kaudu [help@andfrankly.com](emailTo:help@andfrankly.com) kasutajate lisamiseks ausalt & platform.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise & ausalt tema juurdepääsuõiguse andmise lubamine.
    
![Kasutaja määramine][200]

**Britta Simon & ausalt määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
    
    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **ja ausalt**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.
    
    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.
 
Kui klõpsate & ausalt paani paanil juurdepääsu, saate automaatselt sisse loginud-on teie & ausalt rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
