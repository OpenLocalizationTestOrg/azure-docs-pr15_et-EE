<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine OpsGenie | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja OpsGenie vahel."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Õpetus: Azure'i Active Directory integreerimine OpsGenie

Selle õpetuse eesmärk näitavad, kuidas OpsGenie integreerimine Azure Active Directory (Azure AD).

OpsGenie integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs OpsGenie
- Saate lubada automaatselt saada allkirjastatud-on OpsGenie (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD OpsGenie, peate järgmised üksused:

- Tellimuse Azure AD
- OpsGenie ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. OpsGenie lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie lisamine galeriist
OpsGenie integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada OpsGenie galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist OpsGenie lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **OpsGenie**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. Tulemuste paanil valige **OpsGenie**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise OpsGenie nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teadma töölauafunktsioonid kasutaja OpsGenie kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud OpsGenie tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** OpsGenie väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos OpsGenie testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne OpsGenie testida](#creating-a-opsgenie-test-user)** - olema töölauafunktsioonid Britta Simon OpsGenie Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure'i klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise OpsGenie rakenduse.



**Konfigureerida Azure AD ühekordse sisselogimise OpsGenie, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **OpsGenie** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida OpsGenie** lehel Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    lisamine. Logige sisse URL väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise OpsGenie rakenduse abil järgmist mustrit: **"https://app.opsgenie.com/auth/login"**.

    > [AZURE.NOTE] Pöörduge oma [tugimeeskonna OpsGenie](mailto:support@opsgenie.com) kui teil on vaja oma URL Logi sisse.

    b. Klõpsake nuppu **edasi**.


4. Lehel **Konfigureeri ühekordse sisselogimise OpsGenie veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    lisamine. Klõpsake **Serdi alla laadida**ja seejärel salvestage fail oma arvutis. Meil vaja seda serti ja metaandmete URL-id (üksuse ID, SSO sisselogimise URL ja Logi välja URL-i) häälestamine SSO OpsGenie küljel.

    b. Klõpsake nuppu **edasi**.


5. Avage brauseris eksemplar ja seejärel sisselogimiseks OpsGenie administraatorina.

6. Klõpsake nuppu **sätted**ja seejärel klõpsake vahekaarti **Ühekordse sisselogimise** .
 
    ![OpsGenie ühekordse sisselogimise](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. SSO lubamiseks valige **lubatud**.

    ![OpsGenie sätted](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. Klõpsake jaotises **pakkuja** **Azure Active Directory** vahekaarti.

    ![OpsGenie sätted](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Azure Active Directory dialoogiboksi lehe, tehke järgmist.
 
    ![OpsGenie sätted](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    lisamine. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil OpsGenie** dialoogiboksi kopeerige **Ühekordse sisselogimise kohta teenuse URL-i** väärtus ja seejärel kleepige **SAML 2.0 lõpp-punkti** tekstiväli.

    b. Teie allalaaditud serdi base-64-kodeeritud faili loomine.      
    
    > [AZURE.NOTE] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be).

    c. Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige need **X.500 serdi** tekstiväli.

    d. Klõpsake nuppu **Salvesta muudatused**.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.



![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-opsgenie-test-user"></a>OpsGenie testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon OpsGenie kasutaja loomiseks. 

1.  Web brauseriaknas, logige sisse oma OpsGenie rentniku administraatorina.

2.  Liikuge kasutajate loend, klõpsates nuppu **kasutaja** vasakul paanil.
   
    ![OpsGenie sätted](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Klõpsake "**Lisa kasutaja**".

3.  Klõpsake dialoogiboksis **Lisa kasutaja,** tehke järgmist.

    ![OpsGenie sätted](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    lisamine. Tippige tekstiväljale **e-posti** Azure Active Directory Britta's e-posti aadressi.

    b. Tippige väljale **Täisnimi** **Britta Simon**.

    c. Klõpsake nuppu **Salvesta**. 

Britta saavad tema profiili häälestamise juhised e-postiga.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise OpsGenie tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**OpsGenie Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
 
    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **OpsGenie**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud OpsGenie paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on OpsGenie rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
