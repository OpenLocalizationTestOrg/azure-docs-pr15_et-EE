<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine HR2day, Merces | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja HR2day, Merces vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Õpetus: Azure'i Active Directory integreerimine HR2day Merces järgi

Selle õpetuse eesmärk näitavad, kuidas HR2day, Merces integreerimine Azure Active Directory (Azure AD).  
HR2day, Merces integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs HR2day Merces järgi
- Saate lubada automaatselt saada allkirjastatud-on HR2day Merces (ühekordse sisselogimise), Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD HR2day, Merces, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne HR2day Merces ühekordse sisselogimise lubatud tellimuse järgi


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. HR2day, Merces lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>HR2day, Merces lisamine galeriist
HR2day järgi Merces integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada HR2day, Merces galeriist hallatavate SaaS rakenduste loendisse.

**Lisada HR2day, Merces Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
 
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **HR2day Merces järgi**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. Tulemuste paanil valige **HR2day, Merces**, ja klõpsake **lõpuleviimine** rakenduse lisamiseks.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selle jaotise eesmärk on konfigureerimine ja Azure AD ühekordse sisselogimise HR2day Merces nimega "Britta Simon" testkasutaja põhjal, mille testimiseks kuvamiseks.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja poolt Merces HR2day kasutajale Azure AD. Teisisõnu, link seose Azure AD kasutaja ja seotud kasutaja poolt Merces HR2day tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanime** järgi Merces HR2day väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise HR2day Merces, mille testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise, Merces on HR2day testida](#creating-a-hr2day-by-merces-test-user)** - olema töölauafunktsioonid Britta Simon HR2day alusel lingitud Azure AD kujutis tema Merces.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks HR2day Merces, oma konto abil SAML protokolli federation Azure AD lubamise kohta.

Teie HR2day Merces rakendus eeldab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma SAML Turbeloa atribuutide konfigureerimine. Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Enne kui saate konfigureerida SAML kinnituse, peate pöörduma tugimeeskonnale HR2day kaudu [servicedesk@merces.nl](mailto:servicedesk@merces.nl) ja paluge oma rentniku jaoks kordumatut tunnust atribuudi väärtust. Peate selle väärtuseks järgmise jaotise juhiseid.


**Konfigureerida Azure AD ühekordse sisselogimise HR2day, Merces, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **HR2day, Merces** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid** **SAML Turbeloa atribuutide** dialoogiboksi avamiseks. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Nõutavate atribuutide vastendused lisamiseks tehke järgmist, tehke järgmist: 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    lisamine. Klõpsake nuppu **Lisa kasutaja atribuut**.

    b. Tippige väljale **Atribuudi nimi** **"ATTR_LOGINCLAIM"**.

    c. Valige loendist **Väärtus** **liituda**. 

    d. Valige loendist **String1** **User.mail**. 

    e. Tippige tekstiväljale **String2** esitatud **kordumatut tunnust** HR2day meeskonnatöö. 

    f. Tippige tekstiväljale **eraldaja** **@**.

    g. Klõpsake nuppu **valmis**.

  
3. Klõpsake nuppu **Rakenda muudatused**.


1. Klõpsake menüü peal, **Kiirkäivituse** **Kiirkäivituse** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Klõpsake nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajate poolt Merces HR2day logida** , valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    lisamine. Logige sisse URL väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise oma HR2day Merces rakendus, kasutades järgmist mustrit: **"https://\<rentniku nime\>.force.com/\<eksemplari nimi\>"**.

    b. Klõpsake nuppu **edasi**.

4. Lehel **Konfigureeri ühekordse sisselogimise veebisaidil HR2day Merces,** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO saamiseks pöörduge oma HR2day Merces tugimeeskonna kaudu [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) ja allalaaditud serdi faili manustamiseks postkasti. Samuti sisestage URL-i SAML SSO-d, Logi välja URL-i ja väljaandja URL-i nii, et neid saab konfigureerida SSO integreerimiseks.


> [AZURE.NOTE] Palun nimetada Merces meeskonnatöö, et see integreerimine on vaja üksuse ID, et määrata selle mustri **https://hr2day.force.com/INSTANCENAME** abil



6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.  

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>Mõne HR2day Merces testi kasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon HR2day, Merces kasutaja loomiseks. Tehke koostööd HR2day Merces tugitöötajad HR2day konto kasutajate lisamiseks. 


> [AZURE.NOTE]Kui teil on vaja rakendust käsitsi loomine, peate pöörduma selle HR2day Merces tugimeeskonnalt.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise HR2day Merces, mis võimaldab.

![Kasutaja määramine][200] 

**HR2day, Merces Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **HR2day Merces järgi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.  
Funktsiooni HR2day klõpsamisel Merces paani paanil juurdepääsu, saate peaks saada automaatselt sisse loginud-on oma HR2day Merces rakendus.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png
