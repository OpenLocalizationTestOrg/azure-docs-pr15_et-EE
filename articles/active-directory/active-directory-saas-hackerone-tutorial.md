<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine HackerOne | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja HackerOne vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Õpetus: Azure'i Active Directory integreerimine HackerOne

Selles õppetükis saate integreerida HackerOne Azure Active Directory (Azure AD).

HackerOne integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs HackerOne
- Saate lubada automaatselt saada allkirjastatud-on HackerOne (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD HackerOne, peate järgmised üksused:

- Azure'i tellimuse
- Mõne HackerOne ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õpetuses konfigureerimine ja Azure AD ühekordse sisselogimise testimiskeskkonnas testida.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. HackerOne lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-hackerone-from-the-gallery"></a>HackerOne lisamine galeriist
Azure AD HackerOne integreerida, tuleb kõigepealt lisada HackerOne galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist HackerOne lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

![Rakenduste][4]

6. Tippige otsinguväljale **HackerOne**.

![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. Tulemuste paanil valige **HackerOne**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Järgmiseks konfigureerimine ja testige Azure AD ühekordse sisselogimise HackerOne nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD HackerOne töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud HackerOne tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** HackerOne väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos HackerOne testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise mõne HackerOne testida](#creating-a-hackerone-test-user)** - olema töölauafunktsioonid Britta Simon sertifitseeri Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Järgmiseks lubate Azure AD ühekordse sisselogimise klassikaline portaali ja konfigureerida ühekordse sisselogimise HackerOne rakenduse.

Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

**Konfigureerida Azure AD ühekordse sisselogimise HackerOne, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **HackerOne** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida HackerOne** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    lisamine. **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise HackerOne rakenduse abil järgmist mustrit: **"https://hackerone.com/\<ettevõtte nime\>/authentication"**. 

    b. Ühendust klienditoega HackerOne kaudu [support@hackerone.com](mailto:support@hackerone.com) saada oma rentniku URL, kui te ei tea seda.

    c. Tippige tekstiväljale **identifikaator** rentniku URL. 

    d. Klõpsake nuppu **edasi**.


4. Lehel **Konfigureeri ühekordse sisselogimise veebisaidil HackerOne** järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


1. Sisselogimine oma HackerOne rentniku administraatorina.

1. Menüü ülaosas nuppu **sätted**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Liikuge "**autentimine**" ja "**lisada SAML sätted**".

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. Klõpsake dialoogiboksis **SAML sätted** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    lisamine. Tippige tekstiväljale **Meilidomeeni** registreeritud domeeni.

    b. Azure'i klassikaline portaalis, kopeerige **Ühekordse sisselogimise teenuse URL**ja seejärel kleepige ühekordse sisselogimise kohta URL tekstiväli.

    c. Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

       >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)
    
    d. Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see soovitud **X509 serdi** tekstiväli.

    e. Klõpsake nuppu **Salvesta**


1. Klõpsake dialoogiboksis autentimissätted tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    lisamine. Klõpsake käsku **testi**.

    b. Kui **olek** väärtus väljale võrdub **viimati testida olek: loodud**, pöörduge oma tugimeeskonna HackerOne kaudu [support@hackerone.com](mailto:support@hackerone.com) taotleda ülevaadet oma konfiguratsioon.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine

Järgmiseks luua test kasutajal nimega Britta Simon klassikaline portaalis.  

![Azure'i AD kasutaja loomine][20]

**Azure AD SECURE esitamisel testi kasutaja loomiseks tehke järgmist:**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   


### <a name="creating-a-hackerone-test-user"></a>HackerOne testkasutaja loomine

Järgmiseks kasutaja nimega Britta Simon HackerOne loomine. HackerOne toetab just in time ettevalmistamise, mis on vaikimisi sisse lülitatud.

On teile selle jaotise pole toimingu toode. Kui kasutate HackerOne, luuakse uus kasutaja kui see pole veel olemas. [Azure'i AD ühekordse sisselogimise konfigureerimine](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Kui teil on vaja luua kasutaja käsitsi, peate sertifitseeri tugimeeskonna poole.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Järgmiseks lubate Britta Simon HackerOne tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**HackerOne Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **HackerOne**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Lõpuks testida Azure AD ühekordse sisselogimise konfiguratsioonist Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud HackerOne paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on HackerOne rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png