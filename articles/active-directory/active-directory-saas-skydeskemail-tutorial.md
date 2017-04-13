<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Skydesk e-posti | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Skydesk e-posti vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Õpetus: Azure'i Active Directory integreerimine Skydesk e-posti

Selle õpetuse eesmärk näitavad, kuidas Skydesk e-posti integreerimine Azure Active Directory (Azure AD).

Skydesk e-posti integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Skydesk e-posti
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Skydesk e-postiga (ühekordse sisselogimise)
- Saate hallata oma ühe keskse koha – klassikaline portaali Azure Active Directory kontod

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks Skydesk e-posti, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Skydesk e-posti ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks. 

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Skydesk e-posti lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-skydesk-email-from-the-gallery"></a>Skydesk e-posti lisamine galeriist
Skydesk e-posti integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Skydesk e-posti galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Skydesk e-posti lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Skydesk e-posti**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. Tulemuste paanil valige **Skydesk e-posti**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selle jaotise eesmärk on, näete, kuidas konfigureerida ja testida Azure AD ühekordse sisselogimise Skydesk e-posti põhjal testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on Azure AD kasutajale Skydesk e-posti töölauafunktsioonid kasutaja. Teisisõnu, peab toimuma Azure AD kasutaja ja seotud kasutaja Skydesk e-posti lingi seos.

Selle lingi seos on loodud määramine Azure AD väärtus Skydesk meili **kasutajanimi** **kasutajanimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise Skydesk e-posti, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise Skydesk e-posti testimiseks](#creating-a-Skydesk-Email-test-user)** – olema töölauafunktsioonid Britta Simon Skydesk meili, mis on seotud Azure AD kujutis tema.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise Skydesk e-posti rakenduse.



**Konfigureerida Azure AD ühekordse sisselogimise Skydesk e-posti, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Skydesk e-posti** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajate Skydesk e-posti logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.
 
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    lisamine. Logige sisse URL väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Skydesk e-posti rakenduse abil järgmist mustrit: **"https://mail.skydesk.jp/portal/\<ettevõtte nimi\>"**.

    b. Klõpsake nuppu **edasi**.


4. Lehel **Konfigureeri ühekordse sisselogimise Skydesk e-posti** , tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. SSO **Skydesk**e-posti lubamiseks tehke järgmist.
 
    lisamine. Sisselogimise Skydesk meilikontoga administraatorina.

    b. Menüü peal, häälestamine ja valige Org. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. Klõpsake vasakul paanil domeenid.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klõpsake domeeni lisamine.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Sisestage oma domeeni nimi ja seejärel kontrollige domeeni.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klõpsake vasakul paanil kaudu **SAML autentimine**

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Klõpsake lehel **SAML autentimine** dialoogiboksi tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] SAML vastavalt autentimist kasutama kas peate **Domeen kinnitatud** või **portaali URL-i** häälestus. Saate määrata portaali URL-i kordumatu nimi.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    lisamine. Azure AD klassikaline portaalis, kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.

    b. Azure AD klassikaline portaalis, kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **välju** URL-i tekstiväli.

    c. **Muuda parooli URL** on valikuline nii jätke tühjaks.

    d. **Saada võti failist** oma Skydesk e-posti allalaaditud serdi valimiseks klõpsake nuppu ja klõpsake **avatud** laadige üles sert.

    e. Kui **algoritmi**, valige **RSA**.

    f. Klõpsake nuppu **Ok** muudatuste salvestamiseks.


7. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  
    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-skydesk-email-test-user"></a>E-posti Skydesk testkasutaja loomine

Selles jaotises kasutaja nimega Britta Simon Skydesk meilisõnumi loomine.

lisamine. Klõpsake vasakul paanil Skydesk meili kaudu **Kasutajate juurdepääsu** ja seejärel sisestage oma kasutajanimi. 

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Kui vajate hulgi kasutajate loomiseks, peate pöörduma Skydesk e-posti toe poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk lubamine Britta Simon kasutada Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise Skydesk e-posti teel.

![Kasutaja määramine][200] 

**E-posti Skydesk Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.
 
    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Skydesk e-posti**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui klõpsate paanil Accessi paani Skydesk e-posti, mida tuleks saada automaatselt allkirjastatud-on Skydesk e-posti rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
