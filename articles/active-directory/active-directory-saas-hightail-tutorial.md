<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Hightail | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Hightail vahel."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Õpetus: Azure'i Active Directory integreerimine Hightail

Selle õpetuse eesmärk näitavad, kuidas Hightail integreerimine Azure Active Directory (Azure AD).

Hightail integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Hightail
- Saate lubada automaatselt saada allkirjastatud-on Hightail (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Hightail, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Hightail ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Hightail lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-hightail-from-the-gallery"></a>Hightail lisamine galeriist
Hightail integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Hightail galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Hightail lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 
 
    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Hightail**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_01.png)

7. Tulemuste paanil valige **Hightail**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Hightail nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Hightail töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Hightail tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Hightail väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Hightail testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Hightail testida](#creating-a-hightail-test-user)** - olema töölauafunktsioonid Britta Simon Hightail Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Hightail rakenduse.

Hightail rakendus eeldab SAML kinnitused kindlas vormingus. Konfigureerige järgmised nõuded selle rakenduse jaoks. Nende atribuutide väärtused saate hallata rakenduse menüüs **"Atrribute"** . Järgmine pilt kuvatakse järgmine näide. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_51.png) 

**Konfigureerida Azure AD ühekordse sisselogimise Hightail, tehke järgmist.**


1. Azure'i klassikaline portaalis lehel **Hightail** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_general_81.png) 


1. Klõpsake dialoogiboksis **SAML Turbeloa atribuute** iga rea, mis on näidatud järgmises tabelis, tehke järgmist:

  	| Atribuudi nimi | Atribuudi väärtus |
  	| --- | --- |    
  	| Eesnimi | User.givenName |
  	| Perekonnanimi  | User.surname |
  	| E-posti | User.mail |
  	| UserIdentityle | User.mail |

    lisamine. Klõpsake **kasutaja atribuutide lisamine** **Lisada kasutaja Attribure** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_general_82.png) 


    b. Tippige tekstiväljale **Attrubute nime** atribuudi nimi, kuvatakse selle rea.

    c. **Atribuudi väärtus** loendis selsect selle rea atribuudi väärtust.

    d. Klõpsake nuppu **valmis**.  
    



1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  



2. **Kuidas soovite kasutajad logida Hightail** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_03.png) 


3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi kui soovite konfigureerida rakendus **IDP algatatud režiimi**, tehke järgmist ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_04.png) 



    lisamine. Tippige tekstiväljale **Vastus URL-i** järgmise mustriga URL: **"https://www.hightail.com/samlLogin?phi_action=app/samlLogin ja subAction = handleSamlResponse"**

    b. Klõpsake nuppu **Järgmine**

4. Kui soovite konfigureerida rakendus **SP algatatud režiimi** lehel **Rakenduse sätete konfigureerimine** dialoogiboksi, klõpsake **"Kuva täpsemad sätted (valikuline)"** ja sisestage **Logi sisse URL** ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_06.png) 

    lisamine. Logige sisse URL väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Hightail rakenduse abil järgmist mustrit: **https://www.hightail.com/loginSSO**. See on mõeldud klientidele, kes soovivad SSO levinud sisselogimislehe.

    b. Klõpsake nuppu **Järgmine**

5. Lehel **konfigureerimine ühekordse sisselogimise veebisaidil Hightail** järgmiste toimingute ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_05.png) 


    lisamine. Klõpsake nuppu **serdi allalaadimine**ja seejärel salvestage base – 64 encoded serdifail teie arvutis.

    b. Klõpsake nuppu **edasi**.

    > [AZURE.NOTE] Enne veebisaidil Hightail rakenduse konfigureerimine on ühekordse sisselogimise, palun Hightail domeeni e-posti, meeskonnatöö nii, et kõik kasutajad, kes kasutavad seda domeeni, saate kasutada ühekordse sisselogimise funktsioonide valge loend.

6. Saada SSO rakenduse jaoks konfigureeritud, peate oma Hightail rentniku administraatorina sisselogimise.

    lisamine. Menüü peal, klõpsake vahekaarti **konto** ja valige **SAML konfigureerimine**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 


    b. Märkige ruut **Luba SAML**autentimist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige see **SAML Turbeloa allkirjasert** tekstiväli.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 


    d. Kopeeri Hightail Azure AD Remote sisselogimise URL-i **SAML sertimiskeskuse (identiteedipakkuja)** .

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_005.png)
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Kui soovite konfigureerida rakendus **IDP algatatud režiimis** valige **"Identiteedipakkuja (IdP) algatatud sisselogimine"**. Kui **SP algatatud režiimis** valige **"Teenuse pakkuja (SP) algatatud sisselogimine"**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. SAML tarbija URL-i oma eksemplari kopeerige ja kleepige tekstiväljale **Vastus URL-i** , nagu on näidatud etappi 4. 

    g. Klõpsake nuppu **Salvesta**.



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

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 


4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_05.png) 

    lisamine. **Tüüp ja kasutajale**, valige **oma ettevõttes uue kasutaja**.

    b. Tippige väljale **Kasutajanimi** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_07.png) 


8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hightail-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-hightail-test-user"></a>Hightail testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Hightail kasutaja loomiseks. 

On teile selle jaotise pole toimingu toode. Hightail just in time kasutaja ettevalmistamise põhjal kohandatud nõuded toetab. Kui teil on konfigureeritud kohandatud nõuded, nagu on näidatud jaotise **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** ülaltoodud, luuakse automaatselt kasutaja seda pole veel rakenduse. 

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate ühendust klienditoega Hightail kaudu [support@hightail.com](mailto:support@hightail.com).


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Hightail tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Hightail Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
 
    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Hightail**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_50.png) 


1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud Hightail paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Hightail rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_205.png
