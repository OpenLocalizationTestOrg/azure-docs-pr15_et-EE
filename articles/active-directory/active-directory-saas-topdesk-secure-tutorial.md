<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine TOPdesk – turvaline | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada TOPdesk – turvaline Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Õpetus: Azure'i Active Directory integreerimine TOPdesk – turvaline
  
Selle õpetuse eesmärk on integreerimine Azure ja TOPdesk – turvaline kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   A TOPdesk – turvaline ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud TOPdesk – turvaline ühekordse sisselogimise rakendusse veebisaidil oma TOPdesk – turvaline ettevõtte saidil (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks TOPdesk – turvaline lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Rakenduste integreerimise jaoks TOPdesk – turvaline lubamine
  
Selle jaotise eesmärk on liigendamine TOPdesk – turvaline jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Luba rakenduste integreerimise jaoks TOPdesk - turvaline, tehke järgmist:

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **TOPdesk – turvaline**.

    ![Rakenduse Galerii] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **TOPdesk – turvaline**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![TOPdesk – turvaline] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk – turvaline")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine lubamine kasutajate autentimiseks TOPdesk – turvaline Azure AD federation SAML protokolli abil oma kontoga.  
Konfigureerimine ühekordse sisselogimise TOPdesk – turvaline jaoks peab teil logo ikoon faili üles laadida. Faili ikooni saamiseks pöörduge tugimeeskonna TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Logige saidile **TOPdesk – turvaline** ettevõtte administraatorina.

2.  **TOPdesk** menüüs nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Sätted")

3.  Klõpsake nuppu **Logi sisse sätted**.

    ![Login sätted] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login sätted")

4.  Laiendada **Login sätete** menüü ja seejärel käsku **Üldine**.

    ![Üldine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Üldine")

5.  **Secure** osas **SAML sisselogimise** konfigureerimine jaotist, tehke järgmist:

    ![Tehnilised sätted] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Tehnilised sätted")

    1.  Klõpsake nuppu **Laadi alla** avaliku metaandmete faili alla laadida, ja seejärel salvestage see kohalikult teie arvutis.
    2.  Metaandmete faili avada, ja seejärel otsige üles sõlm **AssertionConsumerService** .
        ![Argument tarbija teenus] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Argument tarbija teenus")
    3.  Kopeerige **AssertionConsumerService** väärtus.  

        >[AZURE.NOTE] Peate **Rakenduse URL-i konfigureerimine** jaotises väärtus hiljem sisse selles õpetuses.

6.  Erinevate web brauseriaknas logige oma **Azure klassikaline portaali** sisse administraatorina.

7.  Klõpsake lehel **TOPdesk – turvaline** rakenduse integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Ühekordse sisselogimise konfigureerimine")

8.  Lehel **Kuidas soovite kasutajate TOPdesk – turvaline logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Ühekordse sisselogimise konfigureerimine")

9.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Rakenduse URL-i konfigureerimine")

    1.  **TOPdesk – turvaline Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajate sisselogimiseks oma TOPdesk – turvaline rakendus (nt: "*https://qssolutions.topdesk.net*").
    2.  Kleepige tekstiväljale **TOPdesk – avaliku vastus URL-i** **TOPdesk – turvaline AssertionConsumerService URL-i** (nt: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klõpsake nuppu **edasi**.

10. Lehel **Konfigureeri ühekordse sisselogimise TOPdesk – turvaline juures** metaandmete faili allalaadimiseks klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail oma arvutis kohalikult.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Ühekordse sisselogimise konfigureerimine")

11. Serdi faili loomiseks tehke järgmist.

    ![Sert] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Sert")

    1.  Avage allalaaditud metaandmete fail.
    2.  Laiendage **RoleDescriptor** **xsi:type** , mille **anda: ApplicationServiceType**.
    3.  Kopeerige **X509Certificate** sõlm väärtus.
    4.  Faili oma arvutisse salvestada kohalikult kopeeritud **X509Certificate** väärtus.

12. Klõpsake oma TOPdesk – turvaline ettevõtte saidil **TOPdesk** menüüs **sätted**.

    ![Sätted] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Sätted")

13. Klõpsake nuppu **Logi sisse sätted**.

    ![Login sätted] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login sätted")

14. Laiendada **Login sätete** menüü ja seejärel käsku **Üldine**.

    ![Üldine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Üldine")

15. **Avaliku** jaotises nuppu **Lisa**.

    ![Lisamine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Lisamine")

16. **SAML konfiguratsiooni sisselogimisabimehe** dialoogiboksi lehel tehke järgmist:

    ![SAML konfiguratsiooni abi] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML konfiguratsiooni abi")

    1.  Metaandmete allalaaditud faili, jaotises **Federation metaandmete**üleslaadimiseks klõpsake nuppu **Sirvi**.
    2.  Laadige oma serdifail jaotises **Sert (RSA)**, klõpsake nuppu **Sirvi**.
    3.  Logo teil tugimeeskonnalt TOPdesk **Logo ikooni**, klõpsake jaotises faili üleslaadimiseks klõpsake nuppu **Sirvi**.
    4.  Tippige tekstiväljale **kasutaja nime atribuut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Tippige väljale **kuvatav nimi** oma konfiguratsioon nimi.
    6.  Klõpsake nuppu **Salvesta**.

17. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate TOPdesk - sse sisse logida turvaline, nad peab olema ettevalmistatud TOPdesk – turvaline.  
Puhul TOPdesk – turvaline, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **TOPdesk – turvaline** ettevõtte administraatorina.

2.  Klõpsake menüü peal, **TOPdesk \> uus \> tugi failide \> tehtemärk**.

    ![Tehtemärk] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Tehtemärk")

3.  Klõpsake dialoogiboksis **Uus tehtemärk** tehke järgmist.

    ![Tehtemärgi muutmine.] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Tehtemärgi muutmine.")

    1.  Klõpsake vahekaarti Üldist.
    2.  Tippige jaotises **üldist** tekstiväljale **perekonnanimi** perekonnanimi lubatud Azure Active Directory konto, mida soovite ette valmistada.
    3.  Valige jaotises **asukoht** **saidi** konto.
    4.  Tippige jaotise **TOPdesk Login** tekstiväljale **Kasutajanimi** Kasutajanimi oma kasutaja.
    5.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada muid TOPdesk – turvaline kasutaja konto loomise tööriistade või API-de esitatud TOPdesk – turvaline AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Kasutajate määramine TOPdesk – turvaline, tehke järgmist:

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **TOPdesk – turvaline **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).