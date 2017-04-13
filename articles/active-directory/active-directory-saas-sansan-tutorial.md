<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SanSan | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja SanSan vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Õpetus: Azure'i Active Directory integreerimine SanSan

Selles õppetükis saate teada, kuidas SanSan integreerimine Azure Active Directory (Azure AD).

SanSan integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs SanSan
- Saate lubada automaatselt saada allkirjastatud-on SanSan (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD SanSan, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne SanSan ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Microsoft Azure AD Microsofti ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. SanSan lisamine galeriist
2. Konfigureerimine ja Microsoft Azure AD ühekordse sisselogimise testimine


## <a name="adding-sansan-from-the-gallery"></a>SanSan lisamine galeriist
SanSan integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada SanSan galeriist hallatavate SaaS rakenduste loendisse.

**SanSan galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **SanSan**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_01.png)

7. Tulemuste paanil valige **SanSan**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_06.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigureerimine ja Microsoft Azure AD ühekordse sisselogimise testimine
Selles jaotises konfigureerimine ja Microsoft Azure AD ühekordse sisselogimise koos SanSan põhjal nimega "Britta Simon" testkasutaja testida.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis SanSan töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud SanSan tuleb luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** SanSan väärtus **kasutaja nimi** väärtus.

Microsoft Azure AD ühekordse sisselogimise koos SanSan testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Microsoft Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Microsoft Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Mõne SanSan loomise katse kasutaja](#creating-an-sansan-test-user)** - olema töölauafunktsioonid Britta Simon SanSan Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Microsoft Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Microsoft Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise SanSan rakenduse.


**Konfigureerida Microsoft Azure AD ühekordse sisselogimise SanSan, tehke järgmist.**


1. Klõpsake lehel **SanSan** rakenduse integreerimine Azure klassikaline portaalis konfigureerimine ühekordse sisselogimise konfigureerimine ühekordse sisselogimise dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png) 

2. **Kuidas soovite kasutajad logida SanSan** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_04.png) 

    lisamine. Tippige tekstiväljale **URL Logi sisse** järgmise mustriga URL:

  	| Keskkonnas             | URL-I |
  	| :--                     | :-- |
  	| Arvuti web                  | `https://ap.sansan.com/v/saml2/<company name>/acs`|
  	| Kohalikke mobiilirakenduse kaudu       | `https://internal.api.sansan.com/saml2/<company name>/acs` |
  	| Mobiilse veebibrauseri sätted | `https://ap.sansan.com/s/saml2/<company name>/acs` |


    b. Tippige tekstiväljale **identifikaator** järgmise mustriga URL: 

  	| Keskkonnas             | URL-I |
  	| :--                     | :-- |
  	| Arvuti web                  | `https://ap.sansan.com/v/saml2/<company name>`|
  	| Kohalikke mobiilirakenduse kaudu       | `https://internal.api.sansan.com/saml2/<company name>` |
  	| Mobiilse veebibrauseri sätted | `https://ap.sansan.com/s/saml2/<company name>` |


    c. Klõpsake nuppu **edasi**.

4. Lehel **Konfigureeri ühekordse sisselogimise SanSan veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5.  Rakenduse jaoks konfigureeritud SSO saamiseks kontakti SanSan tugimeeskonna aitab SSO konfigureerimiseks. Palun saatke neile järgmine teave:

    • Allalaaditud **serdi**

    • **Identiteedi pakkuja ID**

    • **SAML SSO URL-i**

    • **Ühekordse sisselogimise teenuse URL-i välja**

> [AZURE.NOTE] PC-veebibrauseri sätted töötada ka mobiilirakenduse ja mobiilibrauseri koos PC web. 


6. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine

Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.
Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-sansan-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-sansan-test-user"></a>Loomise rakendust SanSan test

Selles jaotises nimega Britta Simon SanSan kasutaja loomine. SanSan rakenduse peate olema ette valmistatud enne SSO rakenduse kasutaja. 


> [AZURE.NOTE] Kui teil on vaja luua kasutaja käsitsi või partii kasutajate, peate pöörduma SanSan tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon tema juurdepääsuõiguse andmise SanSan Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**SanSan Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **SanSan**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate testida konfiguratsiooni Microsoft Azure AD ühekordse sisselogimise juurdepääsu juhtpaneeli kaudu.
Kui olete klõpsanud SanSan paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on SanSan rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_205.png
