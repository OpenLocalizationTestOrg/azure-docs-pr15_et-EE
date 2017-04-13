<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Pluralsight | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Pluralsight vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Õpetus: Azure'i Active Directory integreerimine Pluralsight

Selle õpetuse eesmärk näitavad, kuidas Pluralsight integreerimine Azure Active Directory (Azure AD).

Pluralsight integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Pluralsight
- Saate lubada automaatselt saada allkirjastatud-on Pluralsight (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto


Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Pluralsight, peate järgmised üksused:

- Azure'i tellimuse
- Mõne Pluralsight ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Pluralsight lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-pluralsight-from-the-gallery"></a>Pluralsight lisamine galeriist
Pluralsight integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Pluralsight galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Pluralsight lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
 
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Pluralsight**.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_01.png)

7. Tulemuste paanil valige **Pluralsight**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Pluralsight nimega "Britta Simon" testkasutaja põhjal.

Azure AD ühekordse sisselogimise koos Pluralsight testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Pluralsight testida](#creating-a-pluralsight-test-user)** - olema töölauafunktsioonid Britta Simon Pluralsight Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Pluralsight rakenduse.

Rakenduse Pluralsight eeldab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma SAML Turbeloa atribuutide konfigureerimine. Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_02.png) 

**"Kordumatu ID"** atribuuti vastav väärtus, nt töötaja ID või midagi muud, mis sobib abil saate lisada ka oma ettevõtte jaoks. Samuti võtke arvesse, et see ei ole vajalik atribuut; Siiski saate lisada selle kordumatu kasutaja tuvastamiseks. 

**Konfigureerida Azure AD ühekordse sisselogimise Pluralsight, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Pluralsight** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_81.png) 



1. Liigsete **SAML Turbeloa atribuutide**eemaldamiseks tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/2829.png) 


    lisamine. Iga kasutaja atribuudi ülaltoodud tabelis punane väljal libistage kursoriga üle soovitud atribuuti ja klõpsake nuppu Kustuta. 




1. Nõutav **SAML Turbeloa atribuute**, iga rea, mis on näidatud järgmises tabelis, lisamiseks tehke järgmist:

  	| Atribuudi nimi | Atribuudi väärtus |
  	| --- | --- |    
  	| Eesnimi| User.givenName |
  	| Perekonnanimi  | User.surname |
  	| E-posti | User.mail |

    lisamine. Klõpsake **kasutaja atribuutide lisamine** **Lisada kasutaja Attribure** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_82.png) 


    b. Tippige tekstiväljale **Attrubute nime** atribuudi nimi, kuvatakse selle rea.

    c. **Atribuudi väärtus** loendis selsect selle rea atribuudi väärtust.

    d. Klõpsake nuppu **valmis**.  
    


1. Klõpsake nuppu **Rakenda muudatused**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/3232.png)  



1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_83.png)  









1. Azure'i klassikaline portaalis lehel **Pluralsight** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Pluralsight** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_04.png) 


    lisamine. Tippige tekstiväljale Logi sisse URL-i URL, mida kasutatakse teie kasutajad sisselogimise Pluralsight rakenduse abil järgmist mustrit:`https://<instance name>.pluralsight.com/sso/<comapny name>`

    b. Klõpsake nuppu **edasi**.


4. Lehel **Konfigureeri ühekordse sisselogimise Pluralsight veebisaidil** , tehke järgmist:  ![konfigureerimine Ühekordne sisselogimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_05.png) 

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO-d, võtke ühendust Pluralsight [Professionaalsete teenuste](mailTo:professionalservices@pluralsight.com) meeskonna ja sisestage metaandmete allalaaditud faili.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.
  
    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-pluralsight-test-user"></a>Pluralsight testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Pluralsight kasutaja loomiseks. Tehke koostööd Pluralsight tugimeeskonna Pluralsight konto kasutajate lisamiseks. 


> [AZURE.NOTE]Kui teil on vaja rakendust käsitsi loomine, peate pöörduma Pluralsight tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Pluralsight tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Pluralsight Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Pluralsight**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud Pluralsight paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Pluralsight rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_205.png
