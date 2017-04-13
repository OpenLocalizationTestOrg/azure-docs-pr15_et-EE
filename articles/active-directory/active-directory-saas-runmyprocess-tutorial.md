<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine RunMyProcess | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja RunMyProcess vahel."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Õpetus: Azure'i Active Directory integreerimine RunMyProcess

Selle õpetuse eesmärk näitavad, kuidas RunMyProcess integreerimine Azure Active Directory (Azure AD).

RunMyProcess integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs RunMyProcess
- Saate lubada automaatselt saada allkirjastatud-on RunMyProcess (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD RunMyProcess, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne RunMyProcess ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. RunMyProcess lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess lisamine galeriist
RunMyProcess integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada RunMyProcess galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist RunMyProcess lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **RunMyProcess**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. Tulemuste paanil valige **RunMyProcess**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise RunMyProcess nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötamiseks, Azure AD on vaja teada, mis RunMyProcess töölauafunktsioonid kasutaja on kasutaja Azure AD on. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud RunMyProcess tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** RunMyProcess väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos RunMyProcess testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne RunMyProcess testida](#creating-a-runmyprocess-test-user)** - olema töölauafunktsioonid Britta Simon RunMyProcess Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise RunMyProcess rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise RunMyProcess, tehke järgmist.**

1. Klassikaline portaalis lehel **RunMyProcess** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida RunMyProcess** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i abil järgmist mustrit: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Klõpsake nuppu **Järgmine**

    > [AZURE.NOTE] Pange tähele, et peate värskendama väärtuse tegeliku URL Logi sisse. See väärtus saamiseks võtke ühendust klienditoega RunMyProcess e <mailto:support@runmyprocess.com>.
 
4. Lehel **Konfigureeri ühekordse sisselogimise RunMyProcess veebisaidil** klõpsake nuppu **Laadi alla serdi** ja seejärel salvestage fail oma arvutis:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. Erinevate web brauseriaknas sisselogimise oma RunMyProcess rentniku administraatorina.

6. Vasakpoolsel navigeerimisribal paanil, klõpsake nuppu **konto** ja valige **konfigureerimine**.

    ![Ühekordse sisselogimise App pool konfigureerimine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Liikuge jaotisse **autentimise meetodit** ja allapoole juhiseid.

    ![Ühekordse sisselogimise App küljel konfigureerimine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    lisamine. Valige **meetod** **Samlv2 koos SSO**.

    b. **SSO ümber suunata** tekstivälja panna **SAML SSO URL-i** väärtus konfiguratsiooniviisardi Azure AD Rakenduse kaudu.

    c. **Logi välja ümber suunata** tekstivälja panna **Ühe Sign-Out teenuse URL-i** väärtus konfiguratsiooniviisardi Azure AD Rakenduse kaudu.

    d. Tekstivälja panna **Nimevormingut Id** Azure AD Rakenduse konfiguratsiooniviisardi **Nimevormingut identifikaator** väärtus.

    e. Allalaaditud serdi faili sisu kopeerida ja kleepida selle **serdi** tekstiväli. 

    f. Klõpsake ikooni **salvestada** .
    
8. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

9. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist:  ![Azure AD test kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehel tehke järgmist: ![Azure AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

### <a name="creating-a-runmyprocess-test-user"></a>RunMyProcess testkasutaja loomine
  
Selleks, et Azure AD kasutajate RunMyProcess sisse logida, nad peavad olema ettevalmistatud RunMyProcess. Puhul RunMyProcess, ettevalmistamise on käsitsi ülesanne.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma RunMyProcess ettevõtte saidi administraator.

2.  Klõpsake nuppu **konto** ja valige vasakpoolsel navigeerimisribal **Kasutajad** ja seejärel klõpsake nuppu **Uus kasutaja**.

    ![Uus kasutaja] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Uus kasutaja")

3.  Valige jaotises **Kasutaja sätted** tehke järgmist:

    ![Profiil] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profiil")

    lisamine. Tippige **nimi** ja **e-posti** kehtiv AAD konto soovite seotud tekstiväljad säte.

    b. Valige mõni **IDE keel**, **keel** ja **profiili**.

    c. Valige **konto loomise meili mulle**.

    d. Klõpsake nuppu **Salvesta**.

    >[AZURE.NOTE] Saate kasutada mis tahes muud RunMyProcess kasutaja konto loomise tööriistade või API-de esitatud RunMyProcess ettevalmistamine Azure Active Directory Kasutajakontod.
    

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise RunMyProcess tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**RunMyProcess Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **RunMyProcess**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. riba all klõpsake **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.
 
Kui olete klõpsanud RunMyProcess paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on RunMyProcess rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
