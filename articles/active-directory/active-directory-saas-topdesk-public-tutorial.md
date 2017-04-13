<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine TOPdesk – avaliku | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada TOPdesk – avaliku Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Õpetus: Azure'i Directory integreerimine TOPdesk – avaliku

Selle õpetuse eesmärk on integreerimine Azure ja TOPdesk – avaliku kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   A TOPdesk – avaliku ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, Azure AD kasutajate olete määranud TOPdesk – avaliku saab ühekordse sisselogimise rakendusse veebisaidil oma TOPdesk – ettevõtte avaliku saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks TOPdesk – avaliku lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>Rakenduste integreerimise jaoks TOPdesk – avaliku lubamine
  
Selle jaotise eesmärk on liigendamine rakenduste integreerimise jaoks TOPdesk – avaliku lubamise kohta.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>Rakenduste integreerimise jaoks TOPdesk - lubada avaldada, tehke järgmist:

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **TOPdesk - avalik**.

    ![Rakenduse Galerii] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **TOPdesk – avaliku**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![TOPdesk avaliku] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk avaliku")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine lubamine kasutajate autentimiseks TOPdesk – avaliku Azure AD federation SAML protokolli abil oma kontoga.  
Konfigureerimine ühekordse sisselogimise jaoks TOPdesk – avaliku nõuab logo ikoon faili üles laadida. Faili ikooni saamiseks pöörduge tugimeeskonna TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Logige saidile **TOPdesk – avaliku** ettevõtte administraatorina.

2.  **TOPdesk** menüüs nuppu **sätted**.

    ![Sätted] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Sätted")

3.  Klõpsake nuppu **Logi sisse sätted**.

    ![Login sätted] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Login sätted")

4.  Laiendada **Login sätete** menüü ja seejärel käsku **Üldine**.

    ![Üldine] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Üldine")

5.  **SAML sisselogimise** konfigureerimine jaotist **avaliku** jaotises tehke järgmist.

    ![Tehnilised sätted] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Tehnilised sätted")

    1.  Klõpsake nuppu **Laadi alla** avaliku metaandmete faili alla laadida, ja seejärel salvestage see kohalikult teie arvutis.
    2.  Metaandmete faili avada, ja seejärel otsige üles sõlm **AssertionConsumerService** .
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Kopeerige **AssertionConsumerService** väärtus.  

        >[AZURE.NOTE] Peate **Rakenduse URL-i konfigureerimine** jaotises väärtus hiljem sisse selles õpetuses.

6.  Erinevate web brauseriaknas logige oma **Azure klassikaline portaali** sisse administraatorina.

7.  Klõpsake lehel **TOPdesk – avaliku** rakenduse integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Ühekordse sisselogimise konfigureerimine")

8.  Lehel **Kuidas soovite kasutajate TOPdesk – avaliku logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Ühekordse sisselogimise konfigureerimine")

9.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Rakenduse URL-i konfigureerimine")

    1.  **TOPdesk – avaliku Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajate sisselogimiseks oma TOPdesk – avaliku rakendus (nt: "*https://qssolutions.topdesk.net*").
    2.  Kleepige tekstiväljale **TOPdesk – avaliku vastus URL-i** **TOPdesk – avaliku AssertionConsumerService URL-i** (nt: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klõpsake nuppu **edasi**.

10. Lehel **Konfigureeri ühekordse sisselogimise veebisaidil TOPdesk – avaliku** metaandmete faili allalaadimiseks klõpsake nuppu **metaandmete allalaadimine**ja seejärel salvestage fail oma arvutis kohalikult.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Ühekordse sisselogimise konfigureerimine")

11. Serdi faili loomiseks tehke järgmist.

    ![Sert] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Sert")

    1.  Avage allalaaditud metaandmete fail.
    2.  Laiendage **RoleDescriptor** **xsi:type** , mille **anda: ApplicationServiceType**.
    3.  Kopeerige **X509Certificate** sõlm väärtus.
    4.  Faili oma arvutisse salvestada kohalikult kopeeritud **X509Certificate** väärtus.

12. Klõpsake oma TOPdesk – ettevõtte avaliku saidi **TOPdesk** menüüs **sätted**.

    ![Sätted] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Sätted")

13. Klõpsake nuppu **Logi sisse sätted**.

    ![Login sätted] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Login sätted")

14. Laiendada **Login sätete** menüü ja seejärel käsku **Üldine**.

    ![Üldine] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Üldine")

15. **Avaliku** jaotises nuppu **Lisa**.

    ![SAML sisselogimine] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML sisselogimine")

16. **SAML konfiguratsiooni sisselogimisabimehe** dialoogiboksi lehel tehke järgmist:

    ![SAML konfiguratsiooni Assistant] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML konfiguratsiooni abi")

    1.  Metaandmete allalaaditud faili, jaotises **Federation metaandmete**üleslaadimiseks klõpsake nuppu **Sirvi**.
    2.  Laadige oma serdifail jaotises **Sert (RSA)**, klõpsake nuppu **Sirvi**.
    3.  Logo teil tugimeeskonnalt TOPdesk **Logo ikooni**, klõpsake jaotises faili üleslaadimiseks klõpsake nuppu **Sirvi**.
    4.  Tippige tekstiväljale **kasutaja nime atribuut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Tippige väljale **kuvatav nimi** oma konfiguratsioon nimi.
    6.  Klõpsake nuppu **Salvesta**.

17. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate TOPdesk - sse sisse logida avaldada, nad peavad olema ettevalmistatud TOPdesk – avaliku.  
Puhul TOPdesk – avaliku ettevalmistamise on käsitsi tööülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **TOPdesk – avaliku** ettevõtte administraatorina.

2.  Klõpsake menüü peal, **TOPdesk \> uus \> tugi failide \> isiku**.

    ![Isiku] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Isiku")

3.  Klõpsake dialoogiboksis Uus inimene tegema järgmised toimingud:

    ![Uue isiku] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Uue isiku")

    1.  Klõpsake vahekaarti Üldist.
    2.  Tippige tekstiväljale perekonnanimi perekonnanimi lubatud Azure Active Directory konto, mida soovite ette valmistada.
    3.  Valige konto **saidile** .
    4.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud TOPdesk – avaliku kasutaja konto loomise tööriistade või API-de osutavad TOPdesk – avaliku kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Määrata kasutajad TOPdesk - avaldada, tehke järgmist:

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **TOPdesk – avaliku **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).