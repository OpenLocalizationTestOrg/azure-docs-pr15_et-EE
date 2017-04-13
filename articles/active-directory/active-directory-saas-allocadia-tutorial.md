<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Allocadia | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Allocadia vahel."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>Õpetus: Azure'i Active Directory integreerimine Allocadia

Selles õppetükis saate teada, kuidas Allocadia integreerimine Azure Active Directory (Azure AD).

Allocadia integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Allocadia
- Saate lubada automaatselt saada allkirjastatud-on Allocadia (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Allocadia, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Allocadia ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Allocadia lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-allocadia-from-the-gallery"></a>Allocadia lisamine galeriist
Allocadia integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Allocadia galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Allocadia lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Allocadia**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. Tulemuste paanil valige **Allocadia**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Allocadia nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis Allocadia töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Allocadia tuleb luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Allocadia väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Allocadia testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Mõne Allocadia loomise katse kasutaja](#creating-an-allocadia-test-user)** - olema töölauafunktsioonid Britta Simon Allocadia Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Allocadia rakenduse.


Allocadia rakendus eeldab SAML kinnitused kindlas vormingus. Konfigureerige järgmised nõuded selle rakenduse jaoks. Nende atribuutide väärtused saate hallata rakenduse menüüs **"Atrribute"** . Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png) 

**Konfigureerida Azure AD ühekordse sisselogimise Hightail, tehke järgmist.**


1. Azure'i klassikaline portaalis lehel **Allocadia** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png) 


2. Klõpsake dialoogiboksis **SAML Turbeloa atribuute** iga rea, mis on näidatud järgmises tabelis, tehke järgmist:

  	| Atribuudi nimi | Atribuudi väärtus |
  	| --- | --- |    
  	| eesnimi | User.givenName |
  	| perekonnanimi  | User.surname |
  	| e-posti | User.mail |
    

    lisamine. Klõpsake **Lisa kasutaja atribuut** **Lisamine kasutaja Attribure** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png) 


    b. Tippige tekstiväljale **Attrubute nime** atribuudi nimi, kuvatakse selle rea.

    c. **Atribuudi väärtus** loendis selsect selle rea atribuudi väärtust.

    d. Klõpsake nuppu **valmis**.  
    

3. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)  


4. **Kuidas soovite kasutajad logida Allocadia** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png) 

5. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png) 

    lisamine. Tippige väljale IDENTIFER järgmise mustriga URL: testi keskkonna jaoks URL-i nimega **"https://na2standby.allocadia.com"** ja tootmiskeskkonda kasutada **"https://na2.allocadia.com"**

    b. Tippige vastus URL-i järgmise mustriga URL: testi keskkonna jaoks kasutada URL-i mustri tootmiskeskkonda nimega **"https://na2standby.allocadia.com/allocadia/saml/SSO"** ja **"https://na2.allocadia.com/allocadia/saml/SSO"**


6. Lehel **Konfigureeri ühekordse sisselogimise Allocadia veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png) 

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


7.  Rakenduse jaoks konfigureeritud SSO saamiseks kontakti [Allocadia](mailTo:support@allocadia.com) tugimeeskonna aitab SSO konfigureerimiseks. Palun pange tähele, et peate meilisõnumeid saata ja manustada faili alla laadida metaandmete konfigureerimiseks SSO Allocadia pool.
 
    > [AZURE.NOTE] Veenduge, et Allocadia meeskonnatöö Määrake identifikaator väärtus on testimiskeskkonnas nimega **"https://na2standby.allocadia.com"** ja tootmiskeskkonda, peaks olema: **"https://na2.allocadia.com"**


8. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

9. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine

Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.
Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-allocadia-test-user"></a>Loomise rakendust Allocadia test

Selles jaotises nimega Britta Simon Allocadia kasutaja loomine. Allocadia rakenduse tugi samal ajal kasutaja ettevalmistamise. Kui olete konfigureerinud nõuete nagu eespool jaotises **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** seejärel see säte rakenduse kasutajad. 


> [AZURE.NOTE] Kui teil on vaja luua kasutaja käsitsi või partii kasutajate, peate pöörduma Allocadia tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon Allocadia tema juurdepääsuõiguse andmise Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Allocadia Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Allocadia**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.
Kui olete klõpsanud Allocadia paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Allocadia rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png
