<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Novatus | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja SECURE esitamisel vahel."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Õpetus: Azure'i Active Directory integreerimine SECURE esitamine

Selle õpetuse eesmärk näitab, kuidas integreerida SECURE esitamisel Azure Active Directory (Azure AD).  
Integreerimise SECURE esitamisel Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs SECURE esitamisel
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on turvaline esitamisel (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Integreerimine Azure AD konfigureerida SECURE esitamisel, peate järgmised üksused:

- Azure'i tellimuse
- On turvaline esitamisel ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. TURVALINE esitamisel lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-secure-deliver-from-the-gallery"></a>TURVALINE esitamisel lisamine galeriist
Integreerimine SECURE esitamisel Azure AD konfigureerimiseks tuleb kõigepealt lisada SECURE esitamisel galeriist hallatavate SaaS rakenduste loendisse.

**Lisada SECURE esitamisel Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **SECURE esitamisel**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. Tulemuste paanil valige **Turvaline esitamisel**, ja klõpsake **lõpuleviimine** rakenduse lisamiseks.
    
    ![Rakenduse logo ja nime galeriis](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise SECURE esitamisel nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja SECURE esitamisel Azure AD kasutajale. Teisisõnu, link seose Azure AD kasutaja ja seotud kasutaja SECURE esitamisel tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** SECURE esitamisel väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise koos SECURE esitamisel, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Turvaline esitamisel loomise katse kasutaja](#creating-a-secure-deliver-test-user)** - töölauafunktsioonid Britta Simon on lingitud Azure AD kujutis tema inimesed.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise rakenduse SECURE esitamisel.



**Konfigureerida SECURE esitamisel Azure AD ühekordse sisselogimise, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **SECURE esitamisel** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida turvaline esitamisel** lehel Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png) 

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi järgmiste toimingute ja klõpsake nuppu **edasi**.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png) 

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i kasutavad sisselogimise abil järgmise mustriga turvaline pakkuda rakenduse kasutajatele: **"https://i-securedeliver.jp/sd/\<ettevõtte nime\>/jsf/login/sso"**.

    b. Võtke ühendust klienditoega SECURE esitamisel kaudu [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) saada oma rentniku URL, kui te ei tea väärtus.

    c. Tippige tekstiväljale **identifikaator** rentniku URL. 

    d. Klõpsake nuppu **Järgmine**


4. Lehel **Konfigureeri ühekordse sisselogimise veebisaidil SECURE esitamisel** järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO saamiseks pöörduge tugimeeskonna SECURE esitamisel kaudu [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) ja saatke neile järgmine:
    
    • Allalaaditud serdifail

    • **Üksuse ID**

    • **Ühekordse sisselogimise teenuse URL-i**

    • **Ühe väljunud teenuse URL-i**



6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD SECURE esitamisel testi kasutaja loomiseks tehke järgmist:**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
  
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-secure-deliver-test-user"></a>TURVALINE esitamisel testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon SECURE esitamisel kasutaja loomiseks. Tehke koostööd SECURE esitamisel tugimeeskonna SECURE esitamisel konto kasutajate lisamiseks.

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate ühendust klienditoega SECURE esitamisel.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon kasutada Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise SECURE esitamisel.

![Kasutaja määramine][200] 

**TURVALINE esitamisel Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **SECURE esitamisel**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui klõpsate paanil Accessi SECURE esitamisel paani, teil peaks saada automaatselt allkirjastatud-on rakenduse SECURE esitamisel.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png
