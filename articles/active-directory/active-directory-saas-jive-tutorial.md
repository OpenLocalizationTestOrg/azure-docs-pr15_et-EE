<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Jive | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Jive vahel."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Õpetus: Azure'i Active Directory integreerimine Jive

Selles õppetükis saate teada, kuidas Jive integreerimine Azure Active Directory (Azure AD).

Jive integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Jive
- Saate lubada automaatselt saada allkirjastatud-on Jive (ühekordse sisselogimise) Azure AD kontodega tema kasutajatele
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Jive, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Jive ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Jive lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-jive-from-the-gallery"></a>Jive lisamine galeriist
Jive üheks Azure AD integreerimise konfigureerimiseks tuleb kõigepealt lisada Jive galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Jive lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]
2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Jive**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. Tulemuste paanil valige **Jive**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Jive nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis Jive töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, link seose Azure AD kasutaja ja kasutajale seotud Jive tuleb luua.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Jive väärtus **kasutaja nimi** väärtus.

Azure AD ühekordse sisselogimise koos Jive testimiseks ja konfigureerimiseks, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise on Jive testida](#creating-a-jive-test-user)** - olema töölauafunktsioonid Britta Simon Jive Azure AD kujutis tema lingitud.
4. **[Konfigureerimise kasutaja ettevalmistamise](#configuring-user-provisioning)** - liigendamine lubamine kasutaja ettevalmistamise kasutaja Active Directory kontod Jive.
5. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
6. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure'i AD ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on Azure AD ühekordse sisselogimise Azure klassikaline portaalis lubamiseks ja konfigureerimiseks ühekordse sisselogimise Jive rakenduse.

**Konfigureerida Azure AD ühekordse sisselogimise Jive, tehke järgmist.**

1. Klassikaline portaalis lehel **Jive** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. **Kuidas kas soovite kasutajad logida Jive** lehel Valige **Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    lisamine. **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Jive rakenduse abil järgmist mustrit: **https://\<kliendi nimi\>. jivecustom.com**.
    
    b. Klõpsake nuppu **Järgmine**
 
4. Lehel **Konfigureeri ühekordse sisselogimise Jive veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Sisselogimine oma Jive rentniku administraatorina.

6. Menüü ülaosas nuppu "**Saml**".

    ![Ühekordse sisselogimise App pool konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    lisamine. Valige vahekaardil **Genaral** **lubatud** .

    b. Klõpsake nuppu "**Kõik saml sätted salvestada**".

7. Liikuge vahekaardile "**Idp metaandmete**".

    ![Ühekordse sisselogimise App pool konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    lisamine. Allalaaditud metaandmete XML-faili sisu ja seejärel kleepige **identiteedi pakkuja (IDP) metaandmete** tekstiväli.

    b. Klõpsake nuppu "**Kõik saml sätted salvestada**". 

8. Minge menüüsse "**Kasutaja atribuudi vastendamine**".

    ![Ühekordse sisselogimise App pool konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    lisamine. Väljale **e-posti** , kopeerige ja kleepige **meil** väärtus atribuudi nimi.

    b. Tekstiväljale **eesnimi** , kopeerige ja kleepige **givenname** väärtus atribuudi nimi.

    c. Tekstiväljale **Perekonnanimi** , kopeerige ja kleepige **perekonnanimi** väärtus atribuudi nimi.
    
9. Azure AD portaalis valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**.
![Azure'i AD ühekordse sisselogimise][10]

10. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
  ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist:  ![Azure AD test kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehel tehke järgmist: ![Azure AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



###<a name="creating-a-jive-test-user"></a>Jive testkasutaja loomine

Selles jaotises nimega Britta Simon Jive kasutaja loomine. Tehke koostööd Jive tugimeeskonna Jive platvormi kasutajate lisamiseks.


###<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutaja ettevalmistamine Active Directory kasutajakontosid, et Jive lubamise kohta.  
Selle toimingu käigus olete kasutaja Turbeloa, paluge Jive.com peate esitama.
  
Järgmine pilt kujutab näidet seotud dialoogiboks Azure AD:

![Kasutaja ettevalmistamise konfigureerimine] (./media/active-directory-saas-jive-tutorial/IC698794.png "Kasutaja ettevalmistamise konfigureerimine")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Klõpsake haldusportaali Azure'i lehel **Jive** rakenduse integreerimise **konfigureerimine kasutaja ettevalmistamise** **Konfigureerimine kasutaja ettevalmistamise** dialoogiboksi avamiseks.

2.  Sisestage lehel **Sisestage mandaat Jive lubamiseks automaatse kasutaja ettevalmistamise** konfiguratsiooni järgmised sätted.

    1.  Tippige tekstiväljale **Jive administraatori kasutajanime** Jive konto nimi, mis on määratud Jive.com profiili **Süsteemiadministraatori poole** .

    2.  Tippige tekstiväljale **Jive administraatori parooli** selle konto parool.

    3.  Tippige väljale **URL-i Jive rentniku** Jive rentniku URL.

        >[AZURE.NOTE]Jive rentniku URL on URL, mida kasutatakse teie asutuse Jive sisse logida.  
        Tavaliselt URL on järgmises vormingus: **www.\< ettevõtte\>. jive.com**.

    4.  Klõpsake kinnitamaks, et teie konfiguratsiooni **valideerimiseks** .

    5.  Klõpsake **kinnituslehel** avamiseks nuppu **edasi** .

3.  Klõpsake lehel **Confirmation** konfiguratsioonile salvestamiseks märke.
  
Nüüd saate luua testkontoga, oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon tema juurdepääsuõiguse andmise Jive Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Jive Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Jive**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui olete klõpsanud Jive paanil juurdepääsu, saate tuleks saada automaatselt allkirjastatud-on Jive rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
