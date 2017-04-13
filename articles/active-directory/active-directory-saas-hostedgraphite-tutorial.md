<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine majutatud grafiit | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja majutatud grafiit vahel."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Õpetus: Azure'i Active Directory integreerimine majutatud grafiit

Selle õpetuse eesmärk näitavad, kuidas majutatud grafiit integreerimine Azure Active Directory (Azure AD).

Majutatud grafiit integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs majutatud grafiit
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on majutatud grafiit (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD majutatud grafiit, peate järgmised üksused:

- Tellimuse Azure AD
- On majutatud grafiit ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Majutatud grafiit lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-hosted-graphite-from-the-gallery"></a>Majutatud grafiit lisamine galeriist
Majutatud grafiit integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada majutatud grafiit galeriist hallatavate SaaS rakenduste loendisse.

**Majutatud grafiit galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .
    
    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
    
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Majutatud grafiit**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_01.png)

7. Tulemuste paanil valige **Majutatud grafiit**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Valige rakenduse Galerii](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise majutatud grafiit nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on majutatud grafiit töölauafunktsioonid kasutaja kasutajale Azure AD. Teisisõnu, link seose Azure AD kasutaja ja majutatud grafiit seotud kasutaja peab toimuma.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** majutatud grafiit väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise koos majutatud grafiit, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Majutatud grafiit loomise katse kasutaja](#creating-a-hosted-graphite-test-user)** - töölauafunktsioonid Britta Simon olema majutatud grafiit Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise majutatud grafiit rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise majutatud grafiit, tehke järgmist.**

1. Klassikaline portaalis lehel **Majutatud grafiit** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajad logida majutatud grafiit** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_03.png)

3. Lehel **Rakenduse sätete konfigureerimine** dialoogiboksi kui soovite konfigureerida rakendus **IDP algatatud režiimi**, tehke järgmist ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_04.png)

    lisamine. Tippige tekstiväljale **identifikaator** URL-i abil järgmist mustrit:`https://www.hostedgraphite.com/metadata/<user id>`

    b. Tippige tekstiväljale **Vastus URL-i** URL-i abil järgmist mustrit:`https://www.hostedgraphite.com/complete/saml/<user id>`

    c. Klõpsake nuppu **Järgmine**

4. Kui soovite konfigureerida rakendus **SP algatatud režiimi** lehel **Rakenduse sätete konfigureerimine** dialoogiboksi, klõpsake **"Kuva täpsemad sätted (valikuline)"** ja sisestage **Logi sisse URL** ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)

    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL-i abil järgmist mustrit:`https://www.hostedgraphite.com/login/saml/<user id>/`

    b. Klõpsake nuppu **Järgmine**

    > [AZURE.NOTE] Pange tähele, et need ei ole tegeliku väärtuse. Teil on need väärtused tegelik Logi sisse URL-i, identifikaator ja vastus URL-i värskendamine. Saada neid väärtusi, saate minna Accessi -> SAML häälestamine oma rakenduse küljel või pöörduge majutatud grafiit.

5. Lehel **Konfigureeri ühekordse sisselogimise grafiit majutatud veebisaidil** järgmiste toimingute ja klõpsake nuppu **Järgmine**:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_05.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.

6. Sisselogimine oma majutatud grafiit rentniku administraatorina.

7. Avage **SAML häälestuslehele** tulp-(**Accessi -> SAML häälestus**).

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

8. Veenduge, et idest vastavad teie konfiguratsiooni sammus 3.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

9. Kopeerige **väljaandja URL** ja **SAML SSO URL-i** Azure AD **üksuse või väljaandja ID** ja majutatud grafiit **SSO sisselogimise URL-i** .

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_003.png)

9. Valige "**kirjutuskaitstud**" nimega **vaikimisi kasutajaroll**.

    ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

10. Allalaaditud serdi faili sisu ja seejärel kleepige **x.509 vastav sert** tekstiväli.

     ![Ühekordse sisselogimise kohta rakenduse side konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

11. Klõpsake nuppu **Salvesta** .

12. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

13. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises eesmärk testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_09.png)

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png)

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png)

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_05.png)

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_06.png)

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_07.png)

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_08.png)

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-hosted-graphite-test-user"></a>Majutatud grafiit testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon majutatud grafiit kasutaja loomiseks. Majutatud grafiit toetab just in time ettevalmistamise, mis on vaikimisi sisse lülitatud.

On teile selle jaotise pole toimingu toode. Ajal püüdke majutatud grafiit juurde, kui see pole veel olemas luuakse uus kasutaja.

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate pöörduma majutatud grafiit tugimeeskonna kaudu <mailto:help@hostedgraphite.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise majutatud grafiit tema juurdepääsuõiguse andmise lubamine.
    
![Kasutaja määramine][200]

**Majutatud grafiit Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
    
    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **Majutatud grafiit**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_50.png)

1. Klõpsake menüü peal, **Kasutajad**.
    
    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.
    
    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.
 
Kui olete klõpsanud grafiit majutatud Accessi paanil, saate tuleks saada automaatselt allkirjastatud-on majutatud grafiit rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_205.png
