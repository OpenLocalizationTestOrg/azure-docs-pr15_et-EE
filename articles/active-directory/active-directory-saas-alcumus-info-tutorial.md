<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Exchange'i Alcumus teave | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Alcumus teave."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Õpetus: Azure'i Active Directory integreerimine Exchange'i Alcumus teave

Selle õpetuse eesmärk näitavad, kuidas Alcumus teave Exchange'i integreerimine Azure Active Directory (Azure AD).  
Integreerimine Azure AD Alcumus teave Exchange annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs Exchange'i Alcumus teave 
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Alcumus teave Exchange (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Alcumus teave Exchange'iga integreerimine Azure AD konfigureerimiseks vajate järgmist:

- Tellimuse [Azure AD](https://azure.microsoft.com/)
- Mõne [Alcumus teave Exchange'i](http://www.alcumusgroup.com/) ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kolmest põhikomponendist.

1. Alcumus teave Exchange'i lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Alcumus teave Exchange'i lisamine galeriist
Alcumus teave Exchange'i integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Alcumus teave Exchange'i galeriist hallatavate SaaS rakenduste loendisse.

**Alcumus teave Exchange'i galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Alcumus teave Exchange**.

    ![Rakenduste][5]

7. Tulemuste paanil valige **Alcumus teave Exchange**ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rakenduste][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Alcumus teave Exchange'i nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on töölauafunktsioonid kasutaja Exchange'i Alcumus teave kasutajale Azure AD. Teisisõnu, peab link seose Azure AD kasutaja ja seotud kasutaja Exchange'i Alcumus teave kindlaks.  
Selle lingi seos on loodud määramine Azure AD väärtus Alcumus teave Exchange'i **kasutajanimi** **kasutajanimi** väärtus.
 
Azure AD ühekordse sisselogimise Alcumus teave Exchange'iga testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise Alcumus teave Exchange'i testimiseks](#creating-a-alcumus-info-exchange-test-user)** – olema töölauafunktsioonid Britta Simon Alcumus teave Exchange'i Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamine ja konfigureerimine rakenduses Exchange Alcumus teave ühekordse sisselogimise.

**Azure AD ühekordse sisselogimise Alcumus teave Exchange'iga konfigureerimiseks tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Alcumus teave Exchange** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6]

2. Lehel **Kuidas soovite kasutajate Alcumus teave Exchange'i logida** , valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise][7]

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist. 

    ![Azure'i AD ühekordse sisselogimise][8]
 
    lisamine. Tippige tekstiväljale **Vastus URL-i** setup teile Alcumus teave Exchange'i tugimeeskonnalt tarbija URL-i.

    > [AZURE.NOTE] Kui te ei tea, mis on õige väärtuse, ühendust klienditoega Alcumus teave Exchange'i kaudu [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Klõpsake nuppu **edasi**.
 
4. Lehel **Konfigureeri ühekordse sisselogimise veebisaidil Alcumus teave Exchange** klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete teie arvutile.

    ![Mis on Azure AD-ühenduse][9]

5. Ühendust klienditoega Alcumus teave Exchange'i kaudu [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), anda neile metaandmete faili ja need saadame talle, peaks võimaldavad SSO teie eest.


6. Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**. 

    ![Mis on Azure AD-ühenduse][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Mis on Azure AD-ühenduse][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.  

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.
  
    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.
  
    c. Klõpsake nuppu edasi.



6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  
  
    b. Klõpsake soovitud **Perekonnanimi** txtbox, tüüp, **Simon**.
  
    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.
  
    d. Valige loendis **rolliga** **kasutaja**.
  
    e. Klõpsake nuppu **edasi**.


7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.
  
    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Alcumus teave Exchange'i testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Alcumus teave Exchange'i kasutaja loomiseks.

**Nimega Britta Simon Alcumus teave Exchange'i kasutaja loomiseks tehke järgmist:**

1. Ühendust klienditoega Alcumus teave Exchange'i kaudu [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Alcumus teave Exchange'i tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200]

**Alcumus teave Exchange'i Britta Simon määramiseks tehke järgmist.**

1. Azure'i portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201]

2. Valige rakenduste loendis **Alcumus teave Exchange**.

    ![Kasutaja määramine][202]

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui klõpsate paanil Accessi paani Alcumus teave Exchange'i, saate tuleks saada automaatselt allkirjastatud-on rakenduse Alcumus teave Exchange.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png