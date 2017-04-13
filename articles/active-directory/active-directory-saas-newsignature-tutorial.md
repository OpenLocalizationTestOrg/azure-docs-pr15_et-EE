<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Microsoft Azure pilveteenuse haldusportaali | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja pilvepõhise haldusportaali Microsoft Azure vahel."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Õpetus: Azure'i Active Directory integreerimine Microsoft Azure pilveteenuse haldusportaal

Selle õpetuse eesmärk näitab, kuidas Microsoft Azure pilveteenuse haldusportaali integreerimine Azure Active Directory (Azure AD).  
Microsoft Azure pilveteenuse haldusportaali integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Microsoft Azure pilveteenuse haldusportaal
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on pilv haldusportaali Microsoft Azure (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto


Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks Azure AD integreerimine Microsoft Azure pilveteenuse haldusportaali, peate järgmised üksused:

- Tellimuse Azure AD
- Pilveteenuse haldusportaali Microsoft Azure'i ühekordse sisselogimise lubatud tellimuse jaoks


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Microsoft Azure pilveteenuse haldusportaali lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Microsoft Azure pilveteenuse haldusportaali lisamine galeriist
Microsoft Azure pilveteenuse haldusportaali integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Cloud haldusportaali Microsoft Azure galeriist hallatavate SaaS rakenduste loendisse.

**Lisada Microsoft Azure pilveteenuse haldusportaali Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Microsoft Azure pilveteenuse haldusportaali**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_01.png)

7. Tulemuste paanil valige **Cloud haldusportaali Microsoft Azure**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise koos Cloud haldusportaali Microsoft Azure nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja Microsoft Azure pilveteenuse haldusportaal Azure AD kasutajale. Teisisõnu, link seose Azure AD kasutaja ja pilveteenuse haldusportaal Microsoft Azure seotud kasutaja peab toimuma.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Cloud haldusportaal Microsoft Azure väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise Microsoft Azure pilveteenuse haldusportaali abil, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Cloud haldusportaali Microsoft Azure loomise test kasutaja](#creating-a-newsignature-test-user)** - on töölauafunktsioonid Britta Simon Cloud haldusportaal Azure AD kujutis tema lingitud Microsoft Azure.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise oma Cloud haldusportaal Microsoft Azure'i rakenduse.



**Microsoft Azure pilveteenuse haldusportaali abil konfigureerida Azure AD ühekordse sisselogimise, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Cloud haldusportaali Microsoft Azure** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajad pilveteenuses haldusportaali Microsoft Azure logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_04.png) 

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad oma Cloud haldusportaali sisselogimise rakendus Microsoft Azure'i järgmist mustrit:`https://portal.igcm.com/<instance name>`

    b. Klõpsake nuppu **edasi**.


4. Tehke lehel **Konfigureeri ühekordse sisselogimise veebisaidil Microsoft Azure pilveteenuse haldusportaal** toimige järgmiselt.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO saamiseks pöörduge oma Cloud haldusportaali Microsoft Azure'i klienditoega [jczernuszka@newsignature.com](mailTo:jczernuszka@newsignature.com) ja e-posti allalaaditud serdi Manusta fail. Palun anda väljaandja URL-i, SAML SSO URL-i ja ühekordse sisselogimise välja teenuse URL-i selleks, et neid saab konfigureerida SSO integreerimiseks.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.  

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-newsignature-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Pilveteenuse haldusportaali Microsoft Azure'i testi kasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Cloud haldusportaal Microsoft Azure kasutaja loomiseks. Tehke koostööd Cloud haldusportaali Microsoft Azure'i tugimeeskonna jaoks lisada kasutajad pilveteenuses haldusportaal Microsoft Azure'i konto jaoks. 


> [AZURE.NOTE]Kui teil on vaja rakendust käsitsi loomine, peate pöörduma Cloud haldusportaali Microsoft Azure'i tugimeeskonna jaoks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon Azure ühekordse sisselogimise kasutada Microsoft Azure pilveteenuse haldusportaali tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Microsoft Azure pilveteenuse haldusportaali Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Microsoft Azure pilveteenuse haldusportaali**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Pilveteenuse haldusportaali Microsoft Azure'i paani Access paanil klõpsamisel saate peaks saada automaatselt allkirjastatud-on oma Cloud haldusportaali Microsoft Azure'i rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_205.png
