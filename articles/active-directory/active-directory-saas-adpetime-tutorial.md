<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ADP eTime | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja ADP eTime vahel."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Õpetus: Azure'i Active Directory integreerimine ADP eTime

Selle õpetuse eesmärk näitavad, kuidas ADP eTime integreerimine Azure Active Directory (Azure AD).  
ADP eTime integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs ADP eTime
- Saate lubada kasutajatele automaatselt saada allkirjastatud-on Azure AD kontodega tema ADP eTime (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto


Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD ADP eTime, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne ADP eTime ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. ADP eTime lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-adp-etime-from-the-gallery"></a>ADP eTime lisamine galeriist
ADP eTime integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada ADP eTime galeriist hallatavate SaaS rakenduste loendisse.

**ADP eTime galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **ADP eTime**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. Tulemuste paanil valige **ADP eTime**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise ADP eTime nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja ADP eTime Azure AD kasutajale. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud ADP eTime tuleb luua.  
Selle lingi seos on loodud määramine Azure AD väärtusena ADP eTime **kasutajanimi** **kasutajanimi** väärtus.

Azure AD ühekordse sisselogimise koos ADP eTime testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[ADP eTime loomise katse kasutaja](#creating-a-adpetime-test-user)** - olema töölauafunktsioonid, Britta Simon ADP eTime lingitud Azure AD kujutis tema sisse.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise ADP eTime rakenduse.

ADP eTime rakenduse loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma SAML Turbeloa atribuutide konfigureerimine. Järgmine pilt kuvatakse järgmine näide. Alati on nõude nimi **"PersonImmutableID"** ja mis me vastendada ExtensionAttribute2, mis sisaldab Tabeliseose kasutaja väärtuse. Siin on kasutaja vastenduse Too kõige ette Azure AD-ADP eTime tehakse selle töötaja ID, kuid saate seda oma rakenduse sätted ka muu väärtuse vastendada. Palun töö ADP eTime meeskond esmalt, et kasutada õige kasutaja ID ja kaardistada taotluste **"PersonImmutableID"** koos selle väärtuse.  

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

SAML kinnituse saate konfigureerida, peate oma ADP eTime tugimeeskonna poole ja taotleda oma rentniku jaoks kordumatut tunnust atribuudi väärtust. Peate seda väärtust Kohandatud taotluste rakenduse konfigureerimiseks.


**ADP eTime konfigureerida Azure AD ühekordse sisselogimise, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **ADP eTime** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida ADP eTime** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    lisamine. Tippige tekstiväljale **Vastus URL-i** kasutavad kasutajad sisselogimise ADP eTime rakenduse abil järgmise mustriga URL: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Klõpsake nuppu **edasi**.

4. Lehel **Konfigureeri ühekordse sisselogimise ADP eTime veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO saamiseks pöörduge oma tugimeeskonna ADP eTime ja e-posti metaandmete manustamine allalaaditud faili nii, et neid saab konfigureerida SSO integreerimiseks.

    > [AZURE.NOTE] Kui **ADP eTime** meeskonnatöö eksemplari, saada **RelayState** väärtus neid. Järgige selle allpool nimetatud toimingud konfigureerida. Pärast selle konfiguratsiooni saate testida integreerimine. Nii võtke arvesse, et see on oluline konfiguratsiooni selle rakenduse integreerimiseks töötamiseks.

6. Azure AD RelayState väärtus konfigureerimiseks tehke järgmist: 
    
    lisamine. Logige sisse administraatorina [Azure'i haldusportaal](https://portal.azure.com) .

    b. Klõpsake vasakpoolsel navigeerimispaanil nuppu **Rohkem teenuseid**. 
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c. Väljale **Otsi** tippige **Azure Active Directory**ja klõpsake seejärel linki seotud.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Klõpsake **ettevõtte rakendused**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. Jaotises **haldamine** nuppu **Kõik rakendused**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. Väljale **Otsi** tippige **ADP eTime**ja klõpsake seejärel linki seotud. 
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. Klõpsake jaotises **haldamine** **ühekordse sisselogimise**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Valige **Kuva täpsemad URL-i sätted**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    Ma. Tippige tekstiväljale **Relay olekus** järgmised mustrite abil väärtus:
    
    - Tootmiskeskkonda:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Lavastus keskkond:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. Sätted salvestada.

7. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.  
Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.

    b. Tippige väljale **Kasutajanimi** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-adp-etime-test-user"></a>ADP eTime testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon ADP eTime kasutaja loomiseks. Tehke koostööd ADP eTime tugimeeskonna ADP eTime konto kasutajate lisamiseks. 


> [AZURE.NOTE]Kui teil on vaja rakendust käsitsi loomine, peate pöörduma ADP eTime tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon kasutada Azure ühekordse sisselogimise tema ADP eTime juurdepääsu andmine.

![Kasutaja määramine][200] 

**ADP eTime Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **ADP eTime**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.  
Kui klõpsate paanil Accessi eTime paani ADP, teil peaks saada automaatselt allkirjastatud-on ADP eTime rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
