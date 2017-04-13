<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Cezanne HR tarkvara | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Cezanne HR tarkvara vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Õpetus: Azure'i Active Directory integreerimine Cezanne HR tarkvara

Selle õpetuse eesmärk näitavad, kuidas Cezanne HR tarkvara integreerimine Azure Active Directory (Azure AD).

Integreerimine Azure AD Cezanne HR tarkvara annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Cezanne HR tarkvara
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Cezanne HR tarkvara (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks Cezanne HR tarkvara vajate järgmist:

- Tellimuse Azure AD
- Mõne Cezanne HR tarkvara ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Cezanne HR tarkvara lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR tarkvara lisamine galeriist
Cezanne HR tarkvara integreerimine Azure AD konfigureerimiseks peate lisama Cezanne HR tarkvara galeriist hallatavate SaaS rakenduste loendit.

**Galeriist Cezanne HR tarkvara lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
    
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
    
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Cezanne HR tarkvara**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. Tulemuste paanil valige **Cezanne HR tarkvara**ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise Cezanne HR tarkvara nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Cezanne HR tarkvara töölauafunktsioonid kasutaja. Teisisõnu, peab toimuma link seose Azure AD kasutaja ja kasutajale seotud Cezanne HR tarkvara.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Cezanne HR tarkvara väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja Azure AD ühekordse sisselogimise Cezanne HR tarkvara katsetada, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise Cezanne HR tarkvara katsetada](#creating-a-cezanne-hr-software-test-user)** - olema töölauafunktsioonid Britta Simon Cezanne HR tarkvara, mis on seotud Azure AD kujutis tema.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Cezanne HR tarkvararakendus.

**Azure AD ühekordse sisselogimise Cezanne HR tarkvara konfigureerimiseks tehke järgmist.**

1. Klassikaline portaalis lehel **Cezanne HR tarkvara** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Cezanne HR tarkvara** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi tegema järgmised toimingud ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i abil järgmist mustrit: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Tippige tekstiväljale **Vastus URL-i** URL-i abil järgmist mustrit: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Klõpsake nuppu **Järgmine**

    > [AZURE.NOTE] Pange tähele, et teil on need väärtused tegelik Logi sisse URL ja vastus URL-i värskendamine. Saada need väärtused, võtke ühendust klienditoega Cezanne HR tarkvara kaudu <mailto:info@cezannehr.com>.

4. Lehel **Konfigureeri ühekordse sisselogimise Cezanne HR tarkvara** järgmiste toimingute ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.

5. Erinevate web brauseriaknas sisselogimise oma Cezanne HR tarkvara rentniku administraatorina.

6. Klõpsake vasakpoolsel navigeerimispaanil nuppu **Süsteemi Setup**. Avage **turbesätted**. Liikuge **Ühekordse sisselogimise konfigureerimine**.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. **Luba kasutajatel järgmised ühekordse sisselogimise (SSO) teenuse sisse logima** paneeli ruut **SAML 2.0** ja **Täpsem konfiguratsiooni** suvand kõrval.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Klõpsake nuppu **Lisa uus** .

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. **SAML 2.0 IDENTITEEDIPAKKUJAD** jaotises tehke järgmist.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    lisamine. Sisestage oma identiteedi pakkuja nimi **Kuvatav nimi**.

    b. **Üksuse identifikaator** panna tekstivälja Azure AD Rakenduse konfiguratsiooniviisardi **Üksuse ID** väärtus.

    c. Saate muuta **SAML Köitmine** "POSTITA".

    d. **Turvalisus Turbeloa teenuse lõpp-punkti** panna tekstivälja Azure AD Rakenduse konfiguratsiooniviisardi **Ühekordse sisselogimise teenuse URL-i** väärtust.

    e. Sisestage **Kasutaja ID atribuudi nimi**"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name".

    f. Klõpsake ikooni **üles** laadida allalaaditud serdi: Azure'i AD.

    g. Klõpsake nuppu **Ok** . 

10. Klõpsake nuppu **Salvesta** .

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

12. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tippige tekstiväljale **Perekonnanimi** **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Testkasutaja Cezanne HR tarkvara loomine

Selleks, et Azure AD kasutajate Cezanne HR tarkvara sisse logida, nad peab olema ettevalmistatud Cezanne HR tarkvara. Cezanne HR tarkvara ettevalmistamise on käsitsi ülesanne.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Kasutajakonto sätete, tehke järgmist.

1.  Logige oma saidile Cezanne HR tarkvara ettevõtte administraatorina.

2.  Klõpsake vasakpoolsel navigeerimispaanil nuppu **Süsteemi Setup**. Minge **kasutajate haldamine**. Liikuge seejärel **Lisa uus kasutaja**.

    ![Uus kasutaja] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Uus kasutaja")

3.  Tehke **Isiku üksikasjad** jaotises all juhiseid.

    ![Uus kasutaja] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Uus kasutaja")

    lisamine. Määrake **sisemine kasutaja** välja lülitada.

    b. Tippige tekstiväljale **eesnimi** **Britta**.  

    c. Tippige tekstiväljale **Perekonnanimi** **Simon**.

    d. Tippige tekstiväljale **e-posti** Britta Simon konto meiliaadress.

4.  Tehke jaotises **Kontoteave** all juhiseid.

    ![Uus kasutaja] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Uus kasutaja")

    lisamine. Tippige tekstiväljale **kasutajanimi** selle meilikonto aadress, Britta Simon.

    b. Sisestage tekstiväljale **parool** Britta Simon konto parooli.

    c. Valige **HR Professional** **Turvalisus roll**.

    d. Klõpsake nuppu **OK**.

5. Liikuge **Ühekordse sisselogimise** menüü ja valige käsk **Lisa uus** **SAML 2.0 identifikaatorite** ala.

    ![Kasutaja] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Kasutaja")

6. Valige oma identiteedipakkuja **Identiteedipakkuja** ja **Kasutajale identifikaator**tekstivälja, sisestage Britta Simon konto meiliaadress.

    ![Kasutaja] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Kasutaja")
    
7. Klõpsake nuppu **Salvesta** .

    ![Kasutaja] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Kasutaja")


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise Cezanne HR tarkvara kasutada.
    
![Kasutaja määramine][200]

**Cezanne HR tarkvara Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
    
    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **Cezanne HR tarkvara**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.
    
    ![Kasutaja määramine][205]

### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.
 
Kui klõpsate paanil Accessi paani Cezanne HR tarkvara, mida peaks saada automaatselt allkirjastatud-on Cezanne HR tarkvara rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
