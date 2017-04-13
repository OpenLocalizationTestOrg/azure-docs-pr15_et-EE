<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Kiteworks | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Kiteworks vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a>Õpetus: Azure'i Active Directory integreerimine Kiteworks


Selle õpetuse eesmärk näitavad, kuidas Kiteworks integreerimine Azure Active Directory (Azure AD).  
Kiteworks integreerimine Azure AD annab teile järgmised eelised: 

- Saate määrata Azure AD, kellel on juurdepääs Kiteworks 
- Saate lubada automaatselt saada allkirjastatud-on Kiteworks (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata oma ühe keskse koha – Azure Active Directory kontod 

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused 

Konfigureerimiseks integreerimine Azure AD Kiteworks, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Kiteworks ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selle õpetuse eesmärk on teil testida testimiskeskkonnas Azure AD ühekordse sisselogimise lubamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Kiteworks lisamine galeriist 
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-kiteworks-from-the-gallery"></a>Kiteworks lisamine galeriist
Kiteworks integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Kiteworks galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Kiteworks lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**. 

    ![Active Directory][1]

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .
 
    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Kiteworks**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_01.png)

7. Tulemuste paanil valige **Kiteworks**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises eesmärk näitab teile, kuidas konfigureerida ja testige Azure AD ühekordse sisselogimise Kiteworks nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis on kasutajale Azure AD Kiteworks töölauafunktsioonid kasutaja. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Kiteworks tuleb luua.  
Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Kiteworks väärtus **kasutaja nimi** väärtus.
 
Azure AD ühekordse sisselogimise koos Kiteworks testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
4. **[Kasutaja loomise mõne Kiteworks testida](#creating-a-kiteworks-test-user)** - olema töölauafunktsioonid Britta Simon Kiteworks Azure AD kujutis tema lingitud.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Kiteworks rakenduse. Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine. Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

Ühekordse sisselogimise jaoks Kiteworks konfigureerimiseks peate registreeritud domeeni. Kui teil pole veel registreeritud domeeni, võtke ühendust oma Kiteworks tugimeeskonna poole.  



**Konfigureerida Azure AD ühekordse sisselogimise Kiteworks, tehke järgmist.**

1. Azure'i klassikaline portaalis lehel **Kiteworks** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas soovite kasutajad logida Kiteworks** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehe, tehke järgmist:.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_04.png) 


    lisamine. Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse kasutajatele sisselogimise Kiteworks rakenduse järgi (nt: *https://fabrikam.kiteworks.com/*).

    b. Klõpsake nuppu **edasi**.
 
 
4. Lehel **Konfigureeri ühekordse sisselogimise Kiteworks veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_05.png) 

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


1. Logige saidile Kiteworks ettevõtte administraatorina.

1. Klõpsake tööriistaribal nuppu **sätted**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 


1. Klõpsake jaotises **autentimise ja luba** **SSO häälestus**. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png) 


1. Lehe SSO häälestamine, tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png) 

    lisamine. Valige **SSO kaudu autentida**.

    b. Valige **AuthnRequest algatada**.

    c. Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise veebisaidil Kiteworks** lehel **Üksuse ID** väärtus kopeerida ja seejärel kleepige **IDP üksuse ID** tekstiväli. 

    d. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Kiteworks** dialoogiboksi kopeerige **Ühekordse sisselogimise teenuse URL-i** väärtus ja seejärel kleepige see **Ühekordse sisselogimise teenuse URL-i** tekstiväli.

    e. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Kiteworks** dialoogiboksi kopeerige **Ühe Sign-Out teenuse URL-i** väärtus ja seejärel kleepige **Ühe välju teenuse URL-i** tekstiväli.

    f. Avage oma allalaaditud serdi Notepadis, kopeerige sisu ja seejärel kleepige **RSA avalik võti serdi** tekstiväli. 

    g. Klõpsake nuppu **Salvesta**.


6. Azure'i klassikaline portaalis valige ühekordse sisselogimise konfigureerimine kinnituse, ja seejärel klõpsake nuppu **edasi**. 

    ![Azure'i AD ühekordse sisselogimise][10]

7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  

    ![Azure'i AD ühekordse sisselogimise][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selle jaotise eesmärk on Azure klassikaline portaalis nimega Britta Simon test kasutaja loomiseks.

![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_09.png)  

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 
 
4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_05.png)  

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehe, tehke järgmist. 

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_06.png) 
 
    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.
    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_07.png) 
 
8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_08.png) 
  
    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   

  
 
### <a name="creating-a-kiteworks-test-user"></a>Kiteworks testkasutaja loomine

Selle jaotise eesmärk on nimega Britta Simon Kiteworks kasutaja loomiseks.
Kiteworks toetab just in time ettevalmistamise, mis on vaikimisi sisse lülitatud.

On teile selle jaotise pole toimingu toode.
Kitewors juurde, kui see pole veel olemas katse ajal luuakse uus kasutaja.

> [AZURE.NOTE] Kui teil on vaja rakendust käsitsi loomine, peate pöörduma Kiteworks tugimeeskonna poole.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises eesmärk Britta Simon kasutada Azure ühekordse sisselogimise Kiteworks tema juurdepääsuõiguse andmise lubamine.

![Kasutaja määramine][200] 

**Kiteworks Britta Simon määramiseks tehke järgmist.**

1. Azure'i klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Kiteworks**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_50.png) 

1. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203] 

1. Valige loendis kasutajate **Britta Simon**.

2. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]



### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises eesmärk testida Azure AD ühekordse sisselogimise konfiguratsiooni Accessi juhtpaneeli kaudu.  
Kui olete klõpsanud Kiteworks paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Kiteworks rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_205.png






