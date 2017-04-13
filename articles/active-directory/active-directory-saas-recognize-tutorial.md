<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine tuvasta | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja tuvasta vahel."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Õpetus: Azure'i Active Directory integreerimine tuvasta

Selle õpetuse eesmärk näitavad, kuidas tuvasta integreerimine Azure Active Directory (Azure AD).

Tuvasta integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs tuvasta
- Saate lubada kasutajatele automaatselt saada allkirjastatud-on ära tunda (ühekordse sisselogimise) koos oma Azure AD kontod
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD tuvasta, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne tuvasta ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Tuvasta lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-recognize-from-the-gallery"></a>Tuvasta lisamine galeriist
Tuvasta integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada tuvasta galeriist hallatavate SaaS rakenduste loendisse.

**Tuvasta galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
    
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
    
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Tuvasta**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. Tulemuste paanil valige **Tuvasta**ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise tuvasta nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD tuvasta töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud tuvasta tuleb luua.

Selle lingi seos on loodud määramine Azure AD väärtus tuvasta **kasutajanimi** **kasutajanimi** väärtus.

Azure AD ühekordse sisselogimise koos tuvasta testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise on tuvasta testida](#creating-a-recognize-test-user)** - olema töölauafunktsioonid Britta Simon tuvasta Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise tuvasta rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise tuvasta, tehke järgmist.**

1. Klassikaline portaalis lehel **Tuvasta** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida tuvasta** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi tegema järgmised toimingud ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i abil järgmist mustrit: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Tippige tekstiväljale **identifikaator** URL-i abil järgmist mustrit: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Klõpsake nuppu **Järgmine**

    > [AZURE.NOTE] Kui te ei tea kohta need URL-id, tippige valimi URL-ide näide mustriga. Need väärtused saamiseks vaadake lisateavet samm 9 või võtke ühendust klienditoega tuvasta kaudu <mailto:support@recognizeapp.com>.

4. Lehel **Konfigureeri ühekordse sisselogimise tuvasta veebisaidil** klõpsake nuppu **Laadi alla serdi** ja seejärel salvestage fail oma arvutis:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. Erinevate web brauseriaknas sisselogimise oma tuvasta rentniku administraatorina.

6. **Klõpsake paremas ülanurgas menüüd.** Valige **ettevõtte administraator**.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. Klõpsake vasakpoolsel navigeerimispaanil nuppu **sätted**.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. Klõpsake jaotises **SSO sätted** tehke järgmist.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    lisamine. Valige **Luba SSO-d**, nagu **ON**.

    b. Tekstivälja panna **IDP üksuse ID** Azure AD Rakenduse konfiguratsiooniviisardi **Väljaandja URL-i** väärtust.

    c. Tekstivälja panna **Sso sihtkoht URL-i** Azure AD Rakenduse konfiguratsiooniviisardi **Ühekordse sisselogimise teenuse URL-i** väärtus.

    d. Tekstivälja panna **Slo sihtkoht URL-i** Azure AD Rakenduse konfiguratsiooniviisardi **Ühe Sign-Out teenuse URL-i** väärtus.

    e. Avage allalaaditud serdi faili Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **serdi** tekstiväli. 

    f. Klõpsake nuppu **Salvesta sätted** . 

9. Kopeerige kõrval **SSO sätted** jaotises URL-i **teenuse pakkuja metaandmete URL-i**all.
    
    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Avage **metaandmete URL-i link** jaotises tühja brauseri metaandmete dokument alla laadida. Seejärel kasutage EntityDescriptor väärtust tuvasta andnud teile **identifikaatori** dialoogiboksis **Rakenduse sätete konfigureerimine** .

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

12. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tippige tekstiväljale **Perekonnanimi** **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-recognize-test-user"></a>Tuvasta testkasutaja loomine

Selleks, et Azure AD kasutajate tuvasta sisse logida, nad peavad olema ettevalmistatud tuvasta. Tuvasta, ettevalmistamise on käsitsi tööülesande.

See rakendus ei toeta SCIM ettevalmistamise, kuid alternatiivse kasutaja sünkroonimine, et kasutajad sätteid, mis on. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Kasutajakonto sätete, tehke järgmist.

1.  Logige oma saidile tuvasta ettevõtte administraatorina.

2.  **Klõpsake paremas ülanurgas menüüd.** Valige **ettevõtte administraator**.

3.  Klõpsake vasakpoolsel navigeerimispaanil nuppu **sätted**.

4.  **Kasutaja sünkroonimine** jaotises tehke järgmist.
    
    ![Uus kasutaja] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Uus kasutaja")

    lisamine. Valige **Sünkrooni lubatud**, nagu **ON**.

    b. **Valige Sünkrooni pakkuja**, valige **Microsoft / Office 365**.

    c. Klõpsake nuppu **Käivita kasutaja sünkroonimine**

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon kasutada Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise ära tunda.
    
![Kasutaja määramine][200]

**Tuvasta Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
    
    ![Kasutaja määramine][201]

2. Valige **Tuvasta**rakenduste loendis.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.
    
    ![Kasutaja määramine][205]

### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.
 
Kui olete klõpsanud tuvasta paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on tuvasta rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
