<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine sellele Online | Microsoft Azure'i"
    description="Siit saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja sellele Online."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Õpetus: Azure'i Active Directory integreerimine sellele Online

Selles õppetükis saate teada, kuidas sellele Online'i integreerimine Azure Active Directory (Azure AD).

Sellele Online'i integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs sellele veebisaidile
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on sellele Online'i (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks sellele Online'iga peate järgmised üksused:

- Tellimuse Azure AD
- **Sellele Online'i** ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Lisada sellele Online'i galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-tableau-online-from-the-gallery"></a>Lisada sellele Online'i galeriist
Sellele online integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada sellele Online galeriist hallatavate SaaS rakenduste loendisse.

**Lisada sellele Online Galerii, tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige väljale Otsi **Sellele Online**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. Tulemuste paanil valige **Sellele võrgus**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise sellele Online nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis sellele Online'is töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, peab link seose Azure AD kasutaja ja seotud kasutaja sellele Online'is luua.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** sellele Online'is väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise sellele online, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise sellele Online testimine](#creating-a-Tableau-Online-test-user)** - olema töölauafunktsioonid Britta Simon sellele Online'is, mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise sellele Online-rakendus.

**Sellele Online'i konfigureerimine Azure AD ühekordse sisselogimise, tehke järgmist.**

1. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine][6]
2. Klassikaline portaalis **Sellele Online'i** integreerimine lehel nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7] 

3. Lehel **Kuidas soovite kasutajatele sellele online sisse logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    lisamine. Tippige tekstiväljale Logi sisse URL-i URL-i abil järgmist mustrit:`https://sso.online.tableau.com`

    c. Klõpsake nuppu **edasi**.

5. Klõpsake soovitud **konfigureerimine ühekordse sisselogimise sellele online** lehel, klõpsake käsku **metaandmete alla laadida**, ja seejärel salvestage fail oma arvutis.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]
8. Erinevate brauseriaknas sisselogimise sellele Online rakenduse. Avage **sätted** ja seejärel **autentimine**

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. Jaotises **Autentimistüüpidest** . Märkige ruut **ühekordse sisselogimise koos SAML** lubamiseks märkige ruut SAML.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Liikuge kerides allapoole, kuni jaotises **Import metaandmete faili sellele online** .  Klõpsake nuppu Sirvi ja allalaaditud Azure AD metaandmete faili importimine. Klõpsake nuppu **Rakenda**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. **Match kinnitused** jaotises Lisa identiteedipakkuja vastava kinnituse nime e-posti aadressi, eesnimi ja perekonnanimi. Azure AD saada see teave:

    lisamine. Minge tagasi Azure AD. Azure'i klassikaline portaalis lehel **Sellele Online** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**. Kopeerige väärtused nimi: userprincipalname, givenname ees- ja perekonnanimi.
     
    ![Azure'i AD ühekordse sisselogimise](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Aktiveerige sellele Online rakendus ja määrake **Sellele Online atribuudid** jaotises järgmiselt:
    
    -  E-post: **e-posti** või **userprincipalname**
    -  Eesnimi: **givenname**
    -  Perekonnanimi: **perekonnanimi**

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-tableau-online-test-user"></a>Sellele Online testkasutaja loomine

Selles jaotises saate luua nimega Britta Simon Online'is sellele kasutajale.

1. **Sellele võrgus**, klõpsake nuppu **sätted** ja seejärel jaotises **autentimine** . Liikuge kerides jaotiseni **Valige kasutajad** . Klõpsake **kasutajate lisamiseks** ja seejärel **Sisestage meiliaadressid**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Valige **Lisage kasutajaid ühekordse sisselogimise (SSO) autentimine**. **Sisestage e-posti aadressid** väljale lisaminebritta.simon@contoso.com

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Klõpsake nuppu **Loo**.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises lubate Britta Simon Azure ühekordse sisselogimise talle juurdepääsu andmine sellele Online kasutada.

![Kasutaja määramine][200] 

**Sellele Online'i Britta Simon määramiseks tehke järgmist:**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

3. Valige rakenduste loendis **Sellele Online**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

5. Valige loendis kõik kasutajad **Britta Simon**.

6. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui klõpsate paanil juurdepääs sellele Online'i paani, mida peaks saada automaatselt allkirjastatud-on sellele Online rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
