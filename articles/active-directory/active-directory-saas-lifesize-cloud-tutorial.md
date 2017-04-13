<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Lifesize Cloud | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Lifesize pilveteenuse vahel."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Õpetus: Azure'i Active Directory integreerimine Lifesize Cloud

Selles õppetükis saate teada, kuidas Lifesize Cloud integreerimine Azure Active Directory (Azure AD).

Integreerimine Azure AD Lifesize Cloud pakub teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Lifesize pilveteenusesse
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Lifesize pilveteenusesse (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Konfigureerimiseks integreerimine Azure AD Lifesize Cloud, peate järgmised üksused:

- Tellimuse Azure AD
- Mõne Lifesize Cloud ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Lifesize Cloud lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize Cloud lisamine galeriist
Lifesize Cloud integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Lifesize Cloud galeriist hallatavate SaaS rakenduste loendisse.

**Galeriist Lifesize Cloud lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]
2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Lifesize pilve**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. Tulemuste paanil valige **Lifesize Cloud**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises saate konfigureerida ja Azure AD ühekordse sisselogimise test Lifesize Cloud põhjal testkasutaja nimega "Britta Simon".

Ühekordse sisselogimise töötada, peab Azure AD teada, mis Lifesize pilveteenuses töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, peab toimuma Azure AD kasutaja ja seotud kasutaja Lifesize Cloud link seos.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Lifesize Cloud väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise koos Lifesize Cloud, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise Lifesize Cloud testida](#creating-a-lifesize-cloud-test-user)** - töölauafunktsioonid Britta Simon on lingitud Azure AD kujutis tema Lifesize pilveteenuses.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Lifesize Cloud rakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise Lifesize Cloud, tehke järgmist.**

1. Klassikaline portaalis lehel **Lifesize Cloud** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajad logida Lifesize Cloud** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    lisamine. **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Lifesize Cloud rakenduse abil järgmist mustrit: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Klõpsake nuppu **Järgmine**
 
4. Lehel **Konfigureeri ühekordse sisselogimise Lifesize Cloud veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    lisamine. Klõpsake **allalaadimine serti**ja seejärel salvestage fail oma arvutis.

    b. Klõpsake nuppu **edasi**.


5. Saada SSO rakenduse sisselogimine rakendusse Lifesize Cloud administraatoriõigused jaoks konfigureeritud.

6. Klõpsake paremas ülanurgas nuppu oma nime ja seejärel klõpsake **Advance sätted**

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. Advance sätted nüüd klõpsake linki **SSO konfigureerimine** . See avatakse teie eksemplari SSO konfiguratsiooni leht.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Nüüd konfigureerida SSO konfiguratsiooni UI järgmised väärtused.    

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Azure AD väljaandja URL-i väärtus kopeerige ja kleepige see **Identiteedi pakkuja väljaandja** tekstiväljale.

    • Azure AD Remote sisselogimise URL-i väärtus kopeerida ja kleepida mis **Sisselogimise URL-i** tekstiväli.

    • Avage allalaaditud serdi Notepadis ja sert, välja arvatud read algavad sert ja Lõpeta serdi sisu ja kleepige see tekstiväljale **x.509 vastav sert** .

    • SAML atribuut kaardistamine **eesnimi** tekstivälja sisestage väärtus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • SAML atribuut kaardistamine **Perekonnanimi** tekstivälja sisestage väärtus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • SAML atribuut kaardistamine **e-posti** tekstivälja sisestage väärtus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Kontrollige konfiguratsiooni saate klõpsake nuppu **testi** .

    > [AZURE.NOTE] Eduka testimiseks peate Azure AD konfiguratsiooni viisardi lõpuleviimine ja ka juurdepääsu kasutajatele või rühmadele, kes saab teha test.
    
10. Luba selle SSO-d, märkides ruudu **Luba SSO** nuppu.

11. Nüüd klõpsake nuppu **Värskenda** nii, et kõik sätted on salvestatud. See loob RelayState väärtus. Kopeerige RelayState väärtus, mis on loodud tekstiväljale. Peame selle väärtuse järgmiste juhiste juurde.

12. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

13. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]

14. Nüüd logige sisse Azure'i haldusportaal **https://portal.azure.com** administraatori identimisteavet kasutades

15. Klõpsake vasakpoolsel navigeerimispaanil linki **Veel teenused**
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Azure Active Directory ja klõpsake linki **Azure Active Directory** otsimine
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. Siit leiate kõik SaaS rakenduste **Enterprise rakenduste** nupu all.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Nüüd klõpsake **Kõik rakendused** linki järgmise tera
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Lifesize rakenduste häälestamine on RelayState soovite otsida. 
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Nüüd klõpsake **ühekordse sisselogimise** link tera

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Kuvatakse ruut **Kuva täpsemad URL-i sätted** . Märkige ruut.
    
    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Nüüd konfigureerida rakenduse, mida näete Lifesize rakenduse SSO konfiguratsiooni lehel RelayState. 

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. Sätted salvestada.

### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist:  ![Azure AD test kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehel tehke järgmist: ![Azure AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Loomise rakendust Lifesize Cloud test

Selles jaotises saate luua kasutaja nimega Britta Simon Lifesize pilveteenusesse. Lifesize cloud ei toeta automaatse kasutaja ettevalmistamise. Pärast eduka autentimisel veebisaidil Azure AD valmistatakse kasutaja automaatselt rakenduse. 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon tema juurdepääsuõiguse andmise Lifesize Cloud Azure ühekordse sisselogimise kasutada.

![Kasutaja määramine][200] 

**Lifesize Cloud Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Lifesize pilve**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


### <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui olete klõpsanud Lifesize pilvepõhise juurdepääsu paanil, te peaksite saada automaatselt allkirjastatud-on Lifesize Cloud rakenduse.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
