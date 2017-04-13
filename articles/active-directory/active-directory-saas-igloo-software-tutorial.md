<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Igloo tarkvara | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Igloo tarkvara abil!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Õpetus: Azure'i Active Directory integreerimine Igloo tarkvara
  
Selle õpetuse eesmärk on integreerimine Azure ja Igloo tarkvara kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   [Igloo tarkvara](http://www.igloosoftware.com/) ühekordse sisselogimise lubatud tellimuse
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Igloo tarkvara ühekordse sisselogimise rakendusse Igloo tarkvara ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise Igloo tarkvara lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Rakenduste integreerimise Igloo tarkvara lubamine
  
Selle jaotise eesmärk on liigendamine Igloo tarkvara rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Rakenduste integreerimise Igloo tarkvara lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Igloo tarkvara**.

    ![Rakenduse Galerii] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Igloo tarkvara**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Igloo tarkvara oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma keskse töölaua rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **Igloo tarkvara** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida Igloo tarkvara** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Microsoft Azure'i AD ühekordse sisselogimise] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure'i AD ühekordse sisselogimise")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Igloo tarkvara sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.igloocommunities.com/?signin*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Igloo tarkvara** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Igloo tarkvara ettevõtte administraatorina.

6.  Avage **Juhtpaneel**.

    ![Juhtpaneel] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Juhtpaneel")

7.  Klõpsake vahekaarti **liikmelisus** **Logi sätted**.

    ![Logige sisse sätted] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Logige sisse sätted")

8.  Klõpsake jaotises SAML konfiguratsiooni **SAML autentimise konfigureerimine**.

    ![SAML konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML konfigureerimine")

9.  Jaotises **Üldine konfiguratsiooni** tehke järgmist.

    ![Üldine konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Üldine konfigureerimine")

    1.  Tippige väljale **Ühenduse nimi** konfiguratsioonile kohandatud nimi.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Igloo tarkvara** dialoogi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **IdP sisselogimise URL-i** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Igloo tarkvara** dialoogi **Remote välju URL-i** väärtus kopeerida ja seejärel kleepige **URL-i IdP välju** tekstiväli.
    4.  **Logi välja ja taotleda HTTP tüüp**, valige **POSTITA**.
    5.  Teksti faili allalaaditud serdi loomine.
        
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    6.  Teksti failiversiooni serdi esimene rida ja viimase rea eemaldamine, serdi ülejäänud teksti kopeerida ja kleepida selle **Avaliku serdi** tekstiväli.

10. **Vastuse ja autentimise konfigureerimine**, tehke järgmist.

    ![Vastuse ja autentimise konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Vastuse ja autentimise konfigureerimine")

    1.  Nagu **Identiteedipakkuja**, valige **Microsoft ADFS-i**.
    2.  **Identifikaator tüüp**, valige **Meiliaadress**.
    3.  Tippige tekstiväljale **E-posti atribuudi** **emailaddress**.
    4.  Tippige tekstiväljale **Eesnimi atribuut** **givenname**.
    5.  Tippige tekstiväljale **Viimase nime atribuut** **perekonnanimi**.

11. Preform konfigureerimise lõpuleviimiseks tehke järgmist:

    ![Logige sisse kasutaja loomine] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Logige sisse kasutaja loomine")

    1.  **Kasutaja loomine Logi sisse**, valige **Loo uus kasutaja sisselogimisel saidil**.
    2.  Kui **sisselogimine sätted**, valige **Kasutage SAML nupp kuval "Sisse logida"**.
    3.  Klõpsake nuppu **Salvesta**.

12. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida kasutaja ettevalmistamise Igloo tarkvara toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab Igloo tarkvara juurdepääsu paneeli abil sisse logida, Igloo tarkvara kontrollib, kas kasutajal on olemas.  
Kui kasutajakonto pole saadaval veel, on see automaatselt loodud Igloo tarkvara.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Igloo tarkvara kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Igloo tarkvara **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).