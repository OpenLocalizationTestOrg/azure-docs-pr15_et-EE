<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Lessonly | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Lessonly vahel."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a>Õpetus: Azure'i Active Directory integreerimine Lessonly

Selle õpetuse eesmärk näitavad, kuidas Lessonly integreerimine Azure Active Directory (Azure AD).

Lessonly integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Lessonly
- Saate lubada automaatselt saada allkirjastatud-on Lessonly (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Lessonly, peate järgmised üksused:

- Tellimuse Azure AD
- Lessonly ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Lessonly lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-lessonly-from-the-gallery"></a>Lessonly lisamine galeriist
Lessonly integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Lessonly galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Lessonly lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.
 
    ![Rakenduste][4]

6. Tippige otsinguväljale **Lessonly**.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_01.png)

7. Tulemuste paanil valige **Lessonly**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .


![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Lessonly nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Lessonly töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Lessonly tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Lessonly väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Lessonly testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Lessonly testida](#creating-a-Lessonly-test-user)** - olema töölauafunktsioonid Britta Simon Lessonly Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Lessonly oma konto abil SAML protokolli federation Azure AD lubamise kohta.

Lessonly rakenduse loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.
Järgmine pilt kuvatakse järgmine näide.

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png) 

**Konfigureerida Azure AD ühekordse sisselogimise Lessonly, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel Lessonly rakenduse integreerimise menüü ülaosas nuppu **Atribuudid** **SAML Turbeloa atribuutide** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_07.png) 

2. Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_08.png) 

    lisamine. Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.

    b. Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.

    c. Valige loendist **Atribuudi väärtus** atribuudi selle rea kuvatud väärtus.

    d. Klõpsake nuppu **valmis**

3. Klõpsake nuppu **Rakenda muudatused**.

4. Brauseri nuppu **tagasi** Kiirkäivituse dialoogiboksi avamiseks uuesti

5. Klõpsake nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

6. **Kuidas soovite kasutajad logida Lessonly** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_03.png) 

7. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_04.png) 


    lisamine. Logige sisse URL väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Lessonly rakenduse abil järgmist mustrit: **"https://companyname.lesson.ly/signin"**. Kui viitate üldise nimi selle **ettevõtte nimi** peab asendada ka tegelikku nime.


8. Lehel **Konfigureeri ühekordse sisselogimise Lessonly veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


9. Rakenduse jaoks konfigureeritud SSO saamiseks pöörduge kaudu Lessonly tugimeeskonna poole dev@lessonly.com. Allalaaditud serdi faili manustamiseks oma e-posti ja jagada Lessonly meeskonnatöö häälestamiseks SSO temapoolne metaandmete URL-id (üksuse ID, SSO sisselogimise URL-i ja Logi välja URL-i).


10. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

11. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lessonly-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-lessonly-test-user"></a>Lessonly testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Lessonly kasutaja loomiseks. Lessonly toetab just in time ettevalmistamise, mis on vaikimisi sisse lülitatud.

On teile selle jaotise pole toimingu toode. Lessonly juurde, kui see pole veel olemas katse ajal luuakse uus kasutaja. [Azure'i AD ühekordse sisselogimise konfigureerimine](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate pöörduma Lessonly tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Lessonly tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Lessonly Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Lessonly**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud Lessonly paanil Accessi te peaksite saada automaatselt allkirjastatud-on Lessonly rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_205.png
