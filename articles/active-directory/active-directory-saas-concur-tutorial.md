<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Concur | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Concur abil!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Õpetus: Azure'i Active Directory integreerimine Concur  


Selle õpetuse eesmärk on integreerimine Azure ja Concur kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Rentniku jaoks sisse Concur

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Concur lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-concur-tutorial/IC769766.png "Stsenaarium")

>[AZURE.NOTE] Tellimuse Concur SAML kaudu ühendatud SSO konfiguratsiooni on eraldi tööülesande, mis tuleb teil ühendust võtta Concur sooritamiseks.

##<a name="enabling-the-application-integration-for-concur"></a>Rakenduste integreerimise jaoks Concur lubamine

Selle jaotise eesmärk on liigendamine Concur jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Concur lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **kataloogi** kausta mis soovite kataloogi integreerimise lubamine.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-concur-tutorial/IC700994.png "Rakenduste")

4.  **Rakenduse Galerii**avamiseks klõpsake nuppu **Lisa An rakendus**ja seejärel klõpsake nuppu **Lisa rakenduse kasutamise oma asutuse jaoks**.

    ![Mida te soovite teha?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Mida te soovite teha?")

5.  Tippige **väljale Otsi** **Concur**.

    ![Rakenduse Galerii] (./media/active-directory-saas-concur-tutorial/IC721727.png "Rakenduse Galerii")

6.  Tulemuste paanil valige **Concur**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Nõus] (./media/active-directory-saas-concur-tutorial/IC721728.png "Nõus")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Concur oma konto abil SAML protokolli federation Azure AD lubamise kohta.

>[AZURE.NOTE] Tellimuse Concur SAML kaudu ühendatud SSO konfiguratsiooni on eraldi tööülesande, mis tuleb teil ühendust võtta Concur sooritamiseks.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Concur **rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-concur-tutorial/IC769767.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Concur logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-concur-tutorial/IC769768.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Nõus sisselogimise URL** väljale Tippige oma concur rentniku sisselogimise URL ja klõpsake nuppu **edasi**. 

    ![URL-i sisse logida konfigureerimine] (./media/active-directory-saas-concur-tutorial/IC769769.png "URL-i sisse logida konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Concur veebisaidil** , tehke järgmist.

    ![URL-i sisse logida konfigureerimine] (./media/active-directory-saas-concur-tutorial/IC769770.png "URL-i sisse logida konfigureerimine")

    1.  Klõpsake käsku metaandmete ja seejärel turvaliste faili allalaadimiseks oma arvutisse.
    2.  Pöörduge oma rentniku SSO konfigureerimiseks Concur tugimeeskonna poole.
    3.  Valige kinnituse ühekordse sisselogimise konfigureerimine, ja klõpsake nuppu **täielik** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.  

    >[AZURE.NOTE] Tellimuse Concur SAML kaudu ühendatud SSO konfiguratsiooni on eraldi tööülesande, mis tuleb teil ühendust võtta Concur sooritamiseks.

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selle jaotise eesmärk on liigendamine ettevalmistamine Active Directory kasutajakontosid, et Concur lubamise kohta.

Rakenduste kulude teenus, et lubada peab olema õige häälestamise ja kasutamise Web teenuse administraatori profiili. Ära lisa oli administraatoriroll lihtsalt T & E haldus funktsioone kasutavate olemasolevasse administraatori profiili.

Nõus konsultandid või kliendi administraator peab erinevate Web teenuse administraatori profiili loomine ja kliendi administraator peab Kasuta seda profiili, Web Services administraatori funktsioonide (nt rakenduste lubamine). Need profiilid hoida eraldi profiilist kliendi administraator iga päev T & E administraator (T & E administraatori profiili peab olema määratud WSAdmin roll).

Rakendus, mis võimaldab kasutada profiili loomisel Sisestage väljadele kasutaja profiili kliendi administraatori nimi. See on määrata omaniku profiilile. Kui soovitud profiili on loodud, peab klient sisse logida seda profiili nuppu "*lubamine*" partneri rakenduse veebiteenuste menüü.

Järgmistel põhjustel ei saa teha nad kasutavad tavaline T & E halduse profiiliga need toimingut.

1.  Kliendi peab olema üks, mis klõpsab "*Jah*" dialoogi aknas, mis kuvatakse pärast rakenduse on lubatud. Klõpsake selle tunnustab klient soovib partneri rakenduse avada oma andmeid, et teie või partneri ei saa klõpsake seda nuppu Jah.
2.  Kui kliendi administraator, mis on lubanud rakenduse abil T & E administraatori profiili jätab ettevõtte (mille tulemuseks on inaktiveeritud profiili), rakendustel lubatud abil, et profiil ei toimi, kuni rakendus on lubatud teise aktiivne oli administraator profiiliga. Sellepärast, et siis peaks luua erinevate oli Admin profiilid.
3.  Kui administraator jätab ettevõtte, seotud oli administraatori profiili nimi saab muuta asendamine administraator soovi mõjutamata lubatud rakendus, kuna see profiil ei pruugi inaktiveeritud

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **Concur** rentniku.

2.  Valige menüü **Administreerimine** **Veebiteenused**.

    ![CONCUR rentniku] (./media/active-directory-saas-concur-tutorial/IC721729.png "CONCUR rentniku")

3.  Valige vasakus servas paanilt **Veebiteenuste** **Lubamine partneri rakendus**.

    ![Partneri rakenduse lubamine] (./media/active-directory-saas-concur-tutorial/IC721730.png "Partneri rakenduse lubamine")

4.  Loendist **Rakenduse lubamiseks** valige **Azure Active Directory**ja seejärel klõpsake nuppu **Luba**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Klõpsake nuppu **Jah** **Kinnitage toimingu** dialoogiboksi sulgemiseks.

    ![Veenduge, et toiming] (./media/active-directory-saas-concur-tutorial/IC721732.png "Veenduge, et toiming")

6.  Azure'i klassikaline portaalis valige **Concur** **Concur** dialoogiboksi lehe avamiseks rakenduste loendist.

7.  **Konfigureerimine kasutaja ettevalmistamise** dialoogiboksi lehe avamiseks nuppu **Konfigureeri kasutaja ettevalmistamise**.

8.  Sisestage kasutajanimi ja parool Concur administraator ja seejärel klõpsake nuppu **edasi**.

9.  Konfiguratsiooni, klõpsake lehel **Confirmation** lõpuleviimiseks nuppu **valmis** .

Nüüd saate luua testkontoga, oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Concur.
##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Concur kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Concur **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-concur-tutorial/IC769771.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-concur-tutorial/IC767830.png "Jah")

Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Concur.

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
