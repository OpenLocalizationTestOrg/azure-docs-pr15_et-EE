<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Salesforce'i Liivakasti | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Salesforce'i Liivakasti kasutamine!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Õpetus: Azure'i Active Directory integreerimine Salesforce'i Liivakasti
>[AZURE.TIP]Tagasiside, klõpsake [siin](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Selle õpetuse eesmärk on integreerimine Azure ja Salesforce'i Liivakasti kuvamiseks.  
Liivakastid teile võimalus luua mitme eksemplari ettevõtte mitmel otstarbel, nagu näiteks arengu, testimine ja koolitus, ilma andmete ja Salesforce'i tootmise ettevõtte rakenduste kompromisse eraldi keskkonnas.  
Lisateavet leiate teemast [Liivakasti ülevaade](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Klõpsake Salesforce.com Liivakasti
  
Kui teil pole kehtiv Liivakasti Salesforce.com veel, peate pöörduma Salesforce'i.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Salesforce'i Liivakasti jaoks rakenduse integreerimise lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Oma domeeni lubamine
4.  Kasutaja ettevalmistamise konfigureerimine
5.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Salesforce'i Liivakasti jaoks rakenduse integreerimise lubamine
  
Selle jaotise eesmärk on liigendamine Salesforce'i Liivakasti jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Salesforce'i Liivakasti lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Rakenduste")

4.  **Rakenduse Galerii**avamiseks klõpsake nuppu **Lisa An rakendus**ja seejärel klõpsake nuppu **Lisa rakenduse kasutamise oma asutuse jaoks**.

    ![Mida te soovite teha?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Mida te soovite teha?")

5.  Tippige **väljale Otsing**, **Salesforce Liivakasti**.

    ![Rakenduse Galerii] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Rakenduse Galerii")

6.  Tulemuste paanil valige **Salesforce'i Liivakasti**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Salesforce'i Liivakasti] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce'i Liivakasti")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Salesforce'i oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Salesforce'i Liivakasti** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida Salesforce'i Liivakasti** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Salesforce'i Liivakasti] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce'i Liivakasti")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Logi sisse URL** väljale Tippige oma URL-i abil järgmise mustriga `http://company.my.salesforce.com`, ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Rakenduse URL-i konfigureerimine")

4. Kui teil on juba loodud ühekordse sisselogimise jaoks mõne muu Salesforce'i Liivakasti eksemplari kataloogis, siis peate ka konfigureerima **identifikaator** olema sama väärtuse, mis **URL-i sisse logida**. Välja **identifikaator** leiate, märkides ruudu dialoogiboksi lehel **Rakenduse URL-i konfigureerimine** ruut **Kuva täpsemad sätted** .

4.  Lehel **Konfigureeri ühekordse sisselogimise Salesforce'i Liivakasti juures** nuppu **Laadi alla serdi**ja salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse oma Salesforce'i Liivakasti administraatorina.

6.  Klõpsake menüü peal, **häälestus**.

    ![Häälestamine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Häälestamine")

7.  Klõpsake vasakpoolsel navigeerimispaanil nuppu **Turbemeetmed**ja valige **Ühekordse sisselogimise sätted**.

    ![Ühekordse sisselogimise sätted] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Ühekordse sisselogimise sätted")

8.  Klõpsake jaotise ühekordse sisselogimise sätted tehke järgmist.

    ![Ühekordse sisselogimise sätted] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Ühekordse sisselogimise sätted")

    lisamine.  Valige **SAML lubatud**.
    
    b.  Klõpsake nuppu **Uus**.

9.  Klõpsake jaotise SAML ühekordse sisselogimise sätted tehke järgmist.

    ![SAML ühekordse sisselogimise sätted] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML ühekordse sisselogimise sätted")

    lisamine.  Tippige tekstiväljale nimi konfiguratsiooni nimi (nt: *SPSSOWAAD\_testi*).
    
    b.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Salesforce'i Liivakasti** dialoogi **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **väljaandja** tekstiväli.
    
    c.  Tippige tekstiväljale **Üksuse Id** **https://test.salesforce.com** , kui see on esimene Salesforce'i Liivakasti eksemplar, kuhu soovite lisada oma kataloogi. Kui olete juba lisanud eksemplari Salesforce'i Liivakasti ja seejärel **Üksus ID** tüübi **Logi sisse URL**, mis peaks olema selles vormingus:`http://company.my.salesforce.com`
    
    d.  Klõpsake nuppu **Sirvi** allalaaditud serdi üleslaadimine.
    
    e.  **SAML identiteedi tüüp**, valige **argument sisaldab kasutaja objekti Federation ID -d**.
    
    f.  **SAML identiteedi asukoht**, valige **teema lause NameIdentifier element on**.
    
    g.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Salesforce'i Liivakasti** dialoogi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli.
    
    h.  SFDC ei toeta SAML Logi välja.  Lahendusena, kleepige 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' see **Identiteedi pakkuja välju URL-i** tekstiväli.
    
    Ma.  Valige **Teenuse pakkuja algatatud taotlemine Köitmine**, **HTTP POST**.
    
    j. Klõpsake nuppu **Salvesta**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Ühekordse sisselogimise konfigureerimine")

##<a name="enabling-your-domain"></a>Oma domeeni lubamine
  
Selles jaotises eeldab, et olete juba loonud domeeni.  
Lisateavet leiate teemast [Määratlemine teie domeeni nimi](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Oma domeeni lubamiseks tehke järgmist.

1.  Klõpsake vasakpoolsel navigeerimispaanil **Domeenide haldamine**ja seejärel klõpsake nuppu **Minu domeeni.**

    ![Oma domeeni] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Oma domeeni")

    >[AZURE.NOTE]Veenduge, et teie domeen on õigesti konfigureeritud.

2.  **Sisselogimise lehe sätted** jaotises **redigeerimine**nuppu, seejärel **Autentimisteenus**, valige SAML ühekordse sisselogimise säte nimi eelmise jaotise ja lõpuks klõpsake nuppu **Salvesta**.

    ![Oma domeeni] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Oma domeeni")
  
Kohe, kui teil on konfigureeritud domeeni, kasutajate kasutada domeeni URL-i Salesforce'i Liivakasti sisse logida.  
URL-i väärtus saamiseks klõpsake SSO profiili loomist eelmises jaotises.
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutaja ettevalmistamine Active Directory kasutajakontode Salesforce'i Liivakasti lubamise kohta.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Salesforce'i portaalis ülemisel navigeerimisribal, valige oma nimi oma kasutaja menüü laiendamiseks:

    ![Minu sätted] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Minu sätted")

2.  Valige menüüst kasutaja **Minu sätted** , et avada oma lehe **Minu sätted** .

3.  **Isikliku** isikliku jaotise laiendamiseks klõpsake vasakul paanil ja seejärel klõpsake nuppu **Lähtesta minu Turvalisus Turbeloa**:

    ![Minu sätted] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Minu sätted")

4.  **Lähtesta minu Turvalisus Turbeloa** lehel nuppu **Lähtesta turvalisuse loa** taotlemiseks meili, mis sisaldab teie Salesforce.com Turbeloa.

    ![Uue loa] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Uue loa")

5.  Märkige oma e-posti postkasti**salesforce.com.com turvalisus kinnituse"**Salesforce.com meilisõnumi teema.

6.  Vaadake selle e-posti ja kopeerige turvalisus Turbeloa väärtus.

7.  Klõpsake lehel **salesforce'i Liivakasti** rakenduse integreerimine Azure klassikaline portaalis **konfigureerimine kasutaja ettevalmistamise** **Konfigureerimine kasutaja ettevalmistamise** dialoogiboksi avamiseks.

    ![Konfigureerimine kasutaja ettevalmistamine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Konfigureerimine kasutaja ettevalmistamine")

8.  Sisestage lehel **Sisestage mandaat Salesforce'i Liivakasti lubamiseks automaatse kasutaja ettevalmistamise** konfiguratsiooni järgmised sätted.

    ![Salesforce'i Liivakasti] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce'i Liivakasti")

    lisamine.  Tippige tekstiväljale **Salesforce'i Liivakasti administraatori kasutajanime** Salesforce'i Liivakasti konto nime, millel on määratud Salesforce.com **Süsteemiadministraator** profiili.

    b.  **Salesforce'i Liivakasti administraatoriparooli** tekstivälja, tippige parool selle konto jaoks.

    c.  Kleepige tekstiväljale **Kasutaja turvalisus Turbeloa** turvalisus Turbeloa väärtus.

    d.  Klõpsake nuppu **kontrolli** , et kontrollida oma konfiguratsioon.

    e.  Klõpsake **kinnituslehel** avamiseks nuppu **edasi** .

9.  Klõpsake lehel **Confirmation** **lõpuleviimine** salvestada oma konfiguratsioon.
##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Salesforce'i Liivakasti kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Salesforce'i Liivakasti **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Jah")
  
Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Salesforce'i Liivakasti.
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](https://msdn.microsoft.com/library/dn308586).
