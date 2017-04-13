<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Cisco säde | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Cisco säde vahel."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Õpetus: Azure'i Active Directory integreerimine Cisco säde

Selles õppetükis saate teada, kuidas Cisco säde integreerimine Azure Active Directory (Azure AD).

Cisco säde integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Cisco säde
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Cisco säde (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Cisco säde, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne **Cisco säde** ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas. Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Cisco säde lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco säde lisamine galeriist
Cisco säde integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Cisco säde galeriist hallatavate SaaS rakenduste loendisse.

**Cisco säde galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Cisco säde**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. Tulemuste paanil valige **Cisco säde**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Cisco säde nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis Cisco säde töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja Cisco säde seotud kasutaja peab toimuma.
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Cisco säde väärtus **kasutaja nimi** väärtus. Azure AD ühekordse sisselogimise koos Cisco säde testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise Cisco säde testimiseks](#creating-a-cisco-spark-test-user)** – olema töölauafunktsioonid Britta Simon Cisco säde, mis on seotud Azure AD kujutis tema.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubada ja konfigureerida ühekordse sisselogimise Cisco säde rakenduse.

Cisco säde rakendus eeldab SAML kinnitused sisaldama teatud atribuute. Konfigureerige selle rakenduse jaoks järgmisi atribuute. Nende atribuutide väärtused saate hallata rakenduse menüüs **"Atrributes"** . Järgmine pilt kuvatakse järgmine näide.

![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Konfigureerida Azure AD ühekordse sisselogimise Cisco säde, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Cisco säde** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid**.
     
    ![Ühekordse sisselogimise konfigureerimine][5]


2. **SAML Turbeloa atribuutide** dialoogiboksis tehke järgmist.

    lisamine. Klõpsake **kasutaja atribuutide lisamine** **Lisada kasutaja Attribure** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Tippige väljale **Atribuudi nimi** **uid**.
    
    c. Valige loendist **Väärtus** **user.userprincipal**.
    
    d. Klõpsake nuppu **valmis**. Seejärel klõpsake **Muudatuste rakendamiseks** lehe allosas.

3. Klõpsake menüü peal, **Kiirjuhendi**.

    ![Ühekordse sisselogimise konfigureerimine][6]

4. Klassikaline portaalis lehel **Cisco säde** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7] 

5. **Kuidas soovite kasutajad logida Cisco säde** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    lisamine. Tippige tekstiväljale Logi sisse URL-i URL-i abil järgmist mustrit: `https://web.ciscospark.com/#/signin`.

    b. Klõpsake nuppu **edasi**.


7. Klõpsake soovitud **konfigureerimine ühekordse sisselogimise Cisco säde veebisaidil** lehele, klõpsake käsku **metaandmete alla laadida**, ja seejärel salvestage faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Logige sisse [Cisco Cloud koostöö haldamise](https://admin.ciscospark.com/) täieliku administraatori mandaadiga.
9. Valige **sätted** ja klõpsake jaotises **autentimine** nuppu **Muuda**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Valige **integreerida 3-osaline identiteedipakkuja. (Täpsem)** ja valige järgmisel kuval.
11. Klõpsake **Metaandmete faili alla laadida** ja oma arvutis faili salvestada.
12. Lehel **Impordi Idp metaandmete** kas pukseerige lehele Azure AD metaandmete faili või otsige ja Azure AD metaandmete faili üles laadida faili brauseri suvandi abil. Seejärel valige **nõua sertimiskeskus (turvalisem) metaandmete allkirjastatud sert** ja klõpsake nuppu **edasi**. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Valige **Testi SSO ühendust**ja veebibrauseri uuel vahekaardil avamisel autentida Azure AD logides sisse.
14. Naaske brauserivahekaart **Cisco pilve koostöö haldus** . Kui test õnnestus, valige **selle testi õnnestus. Ühekordse sisselogimise suvand Luba** ja klõpsake nuppu **edasi**.

7. Azure AD klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

8. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
    
    ![Azure'i AD ühekordse sisselogimise][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.
    
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist.
 
    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-a-cisco-spark-test-user"></a>Cisco säde testkasutaja loomine

Selles jaotises nimega Britta Simon Cisco säde kasutaja loomine. Selles jaotises nimega Britta Simon Cisco säde kasutaja loomine.

1. Minge [Cisco Cloud koostöö haldamise](https://admin.ciscospark.com/) täieliku administraatori mandaadiga.
2. Klõpsake nuppu **Kasutajad** ja seejärel **kasutajate haldamine**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Aknas **Kasutaja haldamine** valige **käsitsi lisada või muuta kasutajad** ja klõpsake nuppu **edasi**.
4. Valige **nimed ja meiliaadressid**. Seejärel sisestage tekstivälja, kui järgite.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**

    b. Tippige tekstiväljale **Perekonnanimi** **Simon**

    c. Tippige väljale **meiliaadress****britta.simon@contoso.com**

5. Klõpsake plussmärki, et lisada Britta Simon. Klõpsake nuppu **edasi**.
6. **Kasutajate lisamine teenuste** aknas nuppu **Salvesta** ja seejärel **valmis**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon tema juurdepääsuõiguse andmise Cisco säde Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Cisco säde Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Cisco säde**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kõik kasutajad **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.

Kui olete klõpsanud Cisco säde paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Cisco säde rakenduse.

## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
