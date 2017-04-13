<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ADP GlobalView | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja ADP GlobalView vahel."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a>Õpetus: Azure'i Active Directory integreerimine ADP GlobalView

Selle õpetuse eesmärk näitavad, kuidas ADP GlobalView integreerimine Azure Active Directory (Azure AD).  
ADP GlobalView integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs ADP GlobalView
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on ADP GlobalView (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto


Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD ADP GlobalView, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne ADP GlobalView ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. ADP GlobalView lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-adp-globalview-from-the-gallery"></a>ADP GlobalView lisamine galeriist
ADP GlobalView integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada ADP GlobalView galeriist hallatavate SaaS rakenduste loendisse.

**ADP GlobalView galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **ADP GlobalView**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_01.png)

7. Tulemuste paanil valige **ADP GlobalView**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise koos ADP GlobalView alusel testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD ADP GlobalView töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud ADP GlobalView tuleb luua.  
Selle lingi seos on loodud määramine Azure AD väärtusena ADP GlobalView **kasutajanimi** **kasutajanimi** väärtus.

Azure AD ühekordse sisselogimise koos ADP GlobalView testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[ADP GlobalView loomise katse kasutaja](#creating-a-adp-globalview-test-user)** - olema töölauafunktsioonid Britta Simon ADP GlobalView Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise ADP GlobalView rakenduse.

ADP GlobalView rakenduse loodab SAML kinnitused kindlas vormingus, mis nõuab teilt lisada kohandatud atribuudi vastendused konfiguratsioonile SAML Turbeloa atribuute. Järgmine pilt kuvatakse järgmine näide. Alati on nõude nimi **"PersonImmutableID"** ja mis me vastendada ExtensionAttribute2, mis sisaldab Tabeliseose kasutaja väärtuse. Siin on kasutaja vastenduse Too kõige ette Azure AD-ADP GlobalView tehakse selle töötaja ID, kuid saate seda oma rakenduse sätted ka muu väärtuse vastendada. Palun töö ADP GlobalView meeskond esmalt, et kasutada õige kasutaja ID ja kaardistada taotluste **"PersonImmutableID"** koos selle väärtuse. E-post ja kasutajanimi taotluste saate ka kaart, nagu on näidatud joonisel.
 
![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_02.png) 

Saate konfigureerida SAML kinnitust, peate oma ADP GlobalView tugimeeskonna poole ja taotleda oma rentniku jaoks kordumatut tunnust atribuudi väärtust. Peate seda väärtust Kohandatud taotluste rakenduse konfigureerimiseks.


**ADP GlobalView konfigureerida Azure AD ühekordse sisselogimise, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **ADP GlobalView** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajate ADP GlobalView logida** , valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_04.png) 


    lisamine. Tippige tekstiväljale **identifikaator** URL-i kasutatakse idenify ADP GlobalView rakenduse, kasutades ühte järgmistest mustrite: `https://<server name>.globalview.adp.com/federate2` või`https://<server name>.globalview.adp.com/federate`


    b. Tekstiväljale **Vastus URL-i** tippige URL, mida kasutatakse Azure AD postitamiseks vastuse ADP GlobalView rakendus, kasutades ühte järgmistest mustrid: `https://<server name>.globalview.adp.com/federate2/sp/ACS.saml2` või`https://<server name>.globalview.adp.com/federate/sp/ACS.saml2`

    c. Klõpsake nuppu **edasi**.


4. Lehel **Konfigureeri ühekordse sisselogimise ADP GlobalView veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Rakenduse jaoks konfigureeritud SSO, võtke ühendust oma ADP GlobalView tugimeeskonna ja saatke neile järgmine: 

    - Allalaaditud **serdi**
    - **Üksuse ID**
    - **SAML SSO URL**
    - **Ühe väljunud teenuse URL-i**


    > [AZURE.NOTE] Pärast **ADP GlobalView** meeskond konfigureerimine eksemplari, **RelayState** väärtuse toomine. Järgige selle allpool nimetatud toimingud konfigureerida. Pärast selle konfiguratsiooni saate testida integreerimine. Seega võtke arvesse, et see on oluline konfiguratsiooni selle rakenduse integreerimiseks töötamiseks

6. Azure AD RelayState väärtus konfigureerimiseks tehke järgmist: 
    
    lisamine. Logige sisse administraatorina [Azure'i haldusportaal](https://portal.azure.com) .

    b. Klõpsake vasakpoolsel navigeerimispaanil nuppu **Rohkem teenuseid**. 
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_07.png)

    c. Väljale **Otsi** tippige **Azure Active Directory**ja klõpsake seejärel linki seotud.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_08.png)

    d. Klõpsake **ettevõtte rakendused**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_09.png)

    e. Jaotises **haldamine** nuppu **Kõik rakendused**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_10.png)

    f. Väljale **Otsi** tippige **ADP eTime**ja klõpsake seejärel linki seotud. 
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_11.png)

    g. Klõpsake jaotises **haldamine** **ühekordse sisselogimise**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_12.png)

    h. Valige **Kuva täpsemad URL-i sätted**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_13.png)
    
    Ma. Tippige tekstiväljale **Relay olekus** järgmised mustrite abil väärtus:
    
    `https://<server name>.globalview.adp.com/gvolution/session/<instance name>/sso` 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_14.png)

    j. Sätted salvestada.

7. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon testi kasutaja loomiseks.  
Valige loendis kasutajate **Britta Simon**.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-adpglobalview-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-adp-globalview-test-user"></a>ADP GlobalView testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon ADP GlobalView kasutaja loomiseks. Tehke koostööd ADP GlobalView tugimeeskonna ADP GlobalView konto kasutajate lisamiseks. 


> [AZURE.NOTE]Kui teil on vaja rakendust käsitsi loomine, peate ADP GlobalView tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise ADP GlobalView tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**ADP GlobalView Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **ADP GlobalView**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-adpglobalview-tutorial/tutorial_adpglobalview_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Access juhtpaneeli kaudu.  
Kui olete klõpsanud ADP GlobalView paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on ADP GlobalView rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpglobalview-tutorial/tutorial_general_205.png
