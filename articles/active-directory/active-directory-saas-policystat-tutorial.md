<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine PolicyStat | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory PolicyStat abil!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Õpetus: Azure'i Active Directory integreerimine PolicyStat
  
Selle õpetuse eesmärk on integreerimine Azure ja PolicyStat kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   PolicyStat rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud PolicyStat ühekordse sisselogimise rakendusse PolicyStat ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks PolicyStat lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-policystat"></a>Rakenduste integreerimise jaoks PolicyStat lubamine
  
Selle jaotise eesmärk on liigendamine PolicyStat jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Rakenduste integreerimise jaoks PolicyStat lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **PolicyStat**.

    ![Rakenduse Galerii] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **PolicyStat**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks PolicyStat oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Rakenduse PolicyStat eeldab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![Atribuudid] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Atribuudid")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **PolicyStat** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate PolicyStat logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Ühekordse sisselogimise konfigureerimine")

3.  **Rakenduse sätete konfigureerimine** , **Logige sisse URL** väljale Tippige lehe URL, mida kasutatakse teie kasutajad sisselogimise URL-i PolicyStat rakenduse (nt: *"https://demo-azure.policystat.com"*), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse sätete konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Rakenduse sätete konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise PolicyStat juures** **metaandmete allalaadimine**nuppu ja salvestage metaandmete faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil PolicyStat ettevõtte administraatorina.

6.  Klõpsake **vahekaarti** ja klõpsake vasakpoolsel navigeerimispaanil **Ühekordse sisselogimise konfigureerimine** .

    ![Menüü administraator] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Menüü administraator")

7.  Valige jaotises **häälestus** **lubada ühekordse sisselogimise integreerimine**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Ühekordse sisselogimise konfigureerimine")

8.  **Konfigureerimine atribuudid**nuppu ja seejärel klõpsake jaotises **Atribuudid konfigureerida** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Ühekordse sisselogimise konfigureerimine")

    1.  Tippige väljale **Kasutajanimi atribuut** **uid**.
    2.  Tippige tekstiväljale **Eesnimi atribuut** **eesnimi**.
    3.  Tippige tekstiväljale **Viimase nime atribuut** **perekonnanimi**.
    4.  Tippige tekstiväljale **E-posti atribuudi** **emailaddress**.
    5.  Klõpsake nuppu **Salvesta muudatused**.

9.  Klõpsake **Oma IDP metaandmete**ja seejärel klõpsake jaotises **Teie IDP metaandmete** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Ühekordse sisselogimise konfigureerimine")

    1.  Metaandmete allalaaditud faili avamiseks, kopeerige sisu ja seejärel kleepige see **Oma identiteedi pakkuja metaandmete** tekstiväli
    2.  Klõpsake nuppu **Salvesta muudatused**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Ühekordse sisselogimise konfigureerimine")

11. 12. Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** .

    ![Atribuudid] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Atribuudid")

13. Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ![Atribuudid] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Atribuudid")

    1.  Klõpsake nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** **uid**.
    3.  **Atribuudi väärtus** tekstiväli, valige **ExtractMailPrefix()**.
    4.  Valige loendist **e-posti** **User.mail**.
    5.  Klõpsake nuppu **valmis**.
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate PolicyStat sisse logida, nad peavad olema ettevalmistatud PolicyStat.  
PolicyStat toetab ainult kellaaja kasutaja ettevalmistamise. See tähendab, et teil pole vaja käsitsi kasutajate lisamiseks PolicyStat.  
Kasutajad saavad lisatud automaatselt sisse oma esimene sisselogimine ühekordse sisselogimise kaudu.

>[AZURE.NOTE]Saate kasutada mis tahes muud PolicyStat kasutaja konto loomise tööriistade või API-de esitatud PolicyStat AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>PolicyStat kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **PolicyStat **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).