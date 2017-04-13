<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine SAP-i HANA pilve platvormi | Microsoft Azure'i" 
    description="Saate teada, kuidas SAP-i HANA pilve platvormi kasutamine Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja rohkem!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Õpetus: Azure'i Active Directory ja SAP-i HANA pilve platvormi integreerimine
  
Selle õpetuse eesmärk on integreerimine Azure ja SAP-i HANA pilve platvormi kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   SAP-i HANA pilve platvormi konto
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud SAP-i HANA pilve platvormi ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.

>[AZURE.IMPORTANT]Peate oma rakenduse juurutamine või tellida rakenduse SAP-i HANA pilve platvormi kontol testimiseks ühekordse sisselogimise kohta. Selles õpetuses rakendus on juurutatud konto.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  SAP-i HANA Cloud platvormi rakenduse integreerimise lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Rolli määramine kasutajale
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>SAP-i HANA Cloud platvormi rakenduse integreerimise lubamine
  
Selle jaotise eesmärk on liigendamine SAP-i HANA Cloud platvormi rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>SAP-i HANA Cloud platvormi rakenduse integreerimise lubamiseks tehke järgmist.

1.  Klõpsake vasakpoolsel navigeerimispaanil Azure'i haldusportaal **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsing**, **SAP HANA pilve platvormi**.

    ![Rakenduse Galerii] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **SAP-i HANA pilve platvormi**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![SAP-i Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP-i Hana")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks SAP-i HANA pilve platvormi oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma SAP-i HANA pilve platvormi rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **SAP-i HANA pilve platvormi** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas soovite kasutajad logida SAP-i HANA pilve platvormi** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Ühekordse sisselogimise konfigureerimine")

3.  Erinevate web brauseriaknas logige SAP-i HANA Cloud platvormi kokpiti https://account juures. \<horisontaalpaigutus host\>.ondemand.com/cockpit (nt: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Klõpsake vahekaarti **usaldada** .

    ![Usalda] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Usalda")

5.  Usalda haldus jaotises tehke järgmist.

    ![Metaandmete hankimine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Metaandmete hankimine")

    1.  Klõpsake vahekaarti **Kohaliku teenusepakkuja** .
    2.  SAP-i HANA pilve platvormi metaandmete faili alla laadida, klõpsake nuppu **Saada metaandmete**.

6.  Azure Active klassikaline portaalis lehel **Rakenduse URL-i konfigureerimine** tegema järgmised toimingud ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Rakenduse URL-i konfigureerimine")

    1.  Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse teie kasutajad **SAP-i HANA pilve platvormi** rakenduse sisse logida. See on kaitstud ressursi oma SAP-i HANA pilve platvormi rakenduse URL-i konto kohased. URL-i põhineb järgmise mustriga: *https://\<applicationName\>\<account_name\>.\< horisontaalpaigutusega host\>.ondemand.com/\<tee\_et\_kaitstud\_ressurss\> * (nt: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]See on teie SAP-i HANA pilve platvormi rakendus, mis nõuab kasutaja autentida URL.

    2.  SAP-i HANA pilve platvormi metaandmete allalaaditud faili avamiseks ja seejärel otsige **ns3:AssertionConsumerService** silt.
    3.  Kopeeri **asukohta** atribuudi väärtust ja seejärel kleepige **SAP-i HANA Cloud platvormi vastus URL-i** tekstiväli.

7.  Lehel **Konfigureeri ühekordse sisselogimise SAP-i HANA Cloud platvormi** oma metaandmete allalaadimiseks klõpsake nuppu **metaandmete allalaadimine**ja seejärel salvestage fail oma arvutis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Ühekordse sisselogimise konfigureerimine")

8.  SAP-i HANA Cloud platvormi kabiini, klõpsake jaotises **Kohaliku teenusepakkuja** tehke järgmist:

    ![Usalda haldus] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Usalda haldus")

    1.  Klõpsake nuppu **Redigeeri**.
    2.  **Konfiguratsiooni tüüp**, valige väärtus **kohandatud**.
    3.  **Kohaliku pakkuja nimi**, jätke vaikeväärtus.
    4.  **Sisselogimise võti** ja **Serdi allkirjastamise** võti paari loomiseks nuppu **Loo võti sidumiseks**.
    5.  Kui **Põhisumma paljundamine**, valige **keelatud**.
    6.  **Jõusta autentimine**, valige **keelatud**.
    7.  Klõpsake nuppu **Salvesta**.

9.  Klõpsake vahekaarti **Usaldusväärsed identiteedipakkuja** ja klõpsake **Lisamine usaldusväärsete identiteedipakkuja juures**.

    ![Usalda haldus] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Usalda haldus")

    >[AZURE.NOTE]Usaldusväärsete identiteedipakkujad loendi haldamiseks peate valinud kohandatud Otsingukonfiguratsiooni jaotises kohaliku teenusepakkuja. Vaikimisi konfiguratsiooni tüüp, peate-redigeeritavad ja peidetud usalda, SAP-i ID teenusega. Ükski, ei pea iga Usalduskeskuse sätted.

10. Vahekaarti **üldist** ja seejärel klõpsake nuppu **Sirvi** metaandmete allalaaditud faili üles laadida.

    ![Usalda haldus] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Usalda haldus")

    >[AZURE.NOTE] Pärast metaandmete faili üleslaadimisel **ühekordse sisselogimise URL-i**, **Ühe välju URL-i** ja **Serdi allkirjastamise** väärtused täidetakse automaatselt.

11. Klõpsake vahekaarti **Atribuudid** .

12. **Atribuudid** vahekaardil, tehke järgmist.

    ![Atribuudid] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Atribuudid")

    1.  Klõpsates **Add Assertion-Based atribuut**järgmisi atribuute kinnituse vastavalt lisamiseks tehke järgmist.

        |Kinnituse atribuut| Atribuut põhimakse.|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/givenName|   eesnimi|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/surname|        perekonnanimi|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/EmailAddress|e-posti|

    >[AZURE.NOTE]Konfiguratsiooni atribuute, sõltub sellest, kuidas rakendustes HCP kohta on välja töötatud, st millist attribute(s) nad loodavad vastuseks SAML ja mille nimi (põhisumma atribuut) juurdepääs atribuudi kood.
    >  
    >lisamine.  **Atribuut Default** kuvatõmmis on ainult illustreerimiseks. See on vaja teha seda stsenaariumi töötada.  
    >
    >b.  Nimede ja väärtuste **Põhisumma atribuut** pildil näha sõltuvad sellest, kuidas rakendus on välja töötatud. On võimalik, et teie rakendus nõuab erinevate vastendused.

13. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise SAP-i HANA Cloud platvormi** dialoogi, valige ühekordse sisselogimise konfigureerimine kinnituse ja **nuppu**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Ühekordse sisselogimise konfigureerimine")
  
Valikuline sammuna saate konfigureerida rühmade kinnituse vastavalt oma Azure Active Directory identiteedipakkuja

>[AZURE.NOTE]SAP-i HANA pilve platvormi rühmade abil saate määrata ühe või mitme kasutaja dünaamiliselt SAP-i HANA pilve platvormi rakenduste, määratud väärtused atribuudid SAML 2.0 kinnitus ühe või mitme rollidele. Näiteks seisukoht sisaldab atribuut "*lepingu = ajutine*", võite soovida kõikide mõjutatud kasutajate "*ajutine*" rühma lisada. Jaotises "*ajutine*" võib sisaldada ühe või mitme rollid juurutatud konto SAP-i HANA pilve platvormi üks või mitu rakendust.
>  
>Kasutage argumendi vastavalt rühmad, kui soovite mass-määramine üks või mitu rollidele rakenduste kontol SAP-i HANA pilve platvormi paljud kasutajad. Kui soovite määrata ühe- või kasutajate arv (a) teatud role(s) soovitame need otse "**lube**" menüüs SAP-i HANA pilve platvormi Kokpit määramine.

##<a name="assigning-a-role-to-a-user"></a>Rolli määramine kasutajale
  
Selleks, et Azure AD kasutajate SAP-i HANA pilve platvormi sisse logida, peate määrama neile SAP-i HANA pilve platvormi rollid.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Kasutaja rollid määramiseks tehke järgmist.

1.  Logige sisse oma **SAP-i HANA pilve platvormi** piloodikabiini.

2.  Tehke järgmist.

    ![Luba] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Luba")

    1.  Klõpsake nuppu **Luba**.
    2.  Klõpsake vahekaarti **Kasutajad** .
    3.  Tippige tekstiväljale **kasutaja** kasutaja meiliaadress.
    4.  Klõpsake kasutajal määrata rolli **määramine** .
    5.  Klõpsake nuppu **Salvesta**.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>SAP-i HANA pilve platvormi kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **SAP-i HANA pilve platvormi** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).