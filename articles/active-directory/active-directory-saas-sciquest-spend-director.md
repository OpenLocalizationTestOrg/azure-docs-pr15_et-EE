<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SciQuest veedavad Director | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja SciQuest veedavad Director vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Õpetus: Azure'i Active Directory integreerimine SciQuest veedavad Director

Selle õpetuse eesmärk näitavad, kuidas SciQuest veedavad Director integreerimine Azure Active Directory (Azure AD).  
SciQuest veedavad Director integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs SciQuest veedavad Director 
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on SciQuest veedavad Director (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Konfigureerimiseks integreerimine Azure AD SciQuest veedavad Director, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne SciQuest veedavad Director ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. SciQuest veedavad Director lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-sciquest-spend-director-from-the-gallery"></a>SciQuest veedavad Director lisamine galeriist
SciQuest veedavad Director integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada SciQuest veedavad Director galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist SciQuest veedavad Director lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **sciQuest veedavad director**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **SciQuest veedavad Director**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste][6]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise SciQuest veedavad Director nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD SciQuest veedavad Director töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja SciQuest veedavad Director seotud kasutaja peab toimuma.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** SciQuest veedavad Director väärtus **kasutaja nimi** väärtus.
 
Azure AD ühekordse sisselogimise koos SciQuest veedavad Director testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühe ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise SciQuest veedavad Director testimiseks](#creating-a-halogen-software-test-user)** – olema töölauafunktsioonid Britta Simon SciQuest veedavad Director Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure'i AD ühe ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise SciQuest veedavad Director rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise SciQuest veedavad Director, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **SciQuest veedavad Director** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][8]

2. **Kuidas soovite kasutajad logida SciQuest veedavad Director** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][9]

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Rakenduse sätete konfigureerimine][10]
 
     3,1. **Logige sisse URL** väljale Tippige oma URL-i kasutavad kasutajad sisse logida, kasutades järgmist mustrit SciQuest veedavad Director rakenduse: *https://.* sciquest.com/.**

     3,2. Tippige tekstiväljale **Vastus URL-i** sama väärtuse, kui olete tippinud **Logi sisse URL-i** tekstiväli. 

     3.3. Klõpsake nuppu **edasi**.
 
4. Lehel **konfigureerimine ühekordse sisselogimise SciQuest veedavad Director juures** **metaandmete allalaadimine**nuppu ja seejärel salvestage fail metaandmete teie arvutile.

    ![Mis on Azure AD-ühenduse][11]

5. Kontakti SciQuest toetavad selle autentimise meetodit kasutades ülaltoodud allalaaditud metaandmete lubamiseks.

6. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks. 

    ![Mis on Azure AD-ühenduse][15]

10. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Mis on Azure AD-ühenduse][100] 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.
3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Mis on Azure AD-ühenduse][101] 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Mis on Azure AD-ühenduse][102] 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Mis on Azure AD-ühenduse][103] 

    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.
  
    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.
  
    c. Klõpsake nuppu edasi.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Mis on Azure AD-ühenduse][104] 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  
  
    b. Klõpsake soovitud **Perekonnanimi** txtbox, tüüp, **Simon**.
  
    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.
  
    d. Valige loendis **rolliga** **kasutaja**.
  
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Mis on Azure AD-ühenduse][105]  

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Mis on Azure AD-ühenduse][106]   

    lisamine. Kirjutage üles väärtus **Uus parool**.
  
    b. Klõpsake nuppu **valmis**.   
  
 
### <a name="creating-a-sciquest-spend-director-test-user"></a>SciQuest veedavad Director testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon SciQuest veedavad Director kasutaja loomiseks.

Peate SciQuest veedavad Director tugimeeskonna poole ja saatke neile hankimine loodud testi konto üksikasjad.

Teise võimalusena saate kasutada ka lihtsalt õigel ajal ettevalmistamise, ühekordse sisselogimise funktsioon, mis toetab SciQuest veedavad Director.  
Kui just in time ettevalmistamise on lubatud, kasutajad on automaatselt loodud SciQuest veedavad Director ajal ühekordse sisselogimise katsel kui nad pole olemas. See funktsioon välistab ühekordse sisselogimise töölauafunktsioonid kasutajate käsitsi loomiseks.

Saada just in time ettevalmistamise lubatud, peate pöörduma olete SciQuest veedavad Director tugimeeskonnale.
  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise SciQuest veedavad Director tema juurdepääsuõiguse andmise lubamine.

![Mis on Azure AD-ühenduse][200]

**SciQuest veedavad Director Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Mis on Azure AD-ühenduse][201]

2. Valige rakenduste loendis **SciQuest veedavad Director**.

    ![Mis on Azure AD-ühenduse][202]

1. Klõpsake menüü peal, **Kasutajad**.

    ![Mis on Azure AD-ühenduse][203]

1. Valige loendis kasutajate **Britta Simon**.

    ![Mis on Azure AD-ühenduse][204]

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Mis on Azure AD-ühenduse][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud SciQuest veedavad Director paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on SciQuest veedavad Director rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png

