<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Bonus.ly | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Bonus.ly abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Õpetus: Azure'i Active Directory integreerimine Bonus.ly

Selle õpetuse eesmärk on integreerimine Azure ja Bonus.ly kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Klõpsake Bonus.ly rentniku test

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Bonus.ly lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-bonusly"></a>Rakenduste integreerimise jaoks Bonus.ly lubamine

Selle jaotise eesmärk on liigendamine Bonus.ly jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Bonus.ly lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Ühekordse sisselogimise lubamiseks")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Bonus.ly**.

    ![Rakenduse Galerii] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Bonus.ly**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Bonus.ly oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Bonus.ly konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Bonus.ly** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Bonus.ly logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Bonus.ly rentniku URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Bonus.ly*", ja seejärel klõpsake nuppu **Järgmine**: 

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Bonus.ly juures** nuppu **Laadi alla serdi**ja salvestage serdi fail kohalikult **c:\\Bonusly.cer**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate brauseriaknas, logige sisse oma **Bonus.ly** rentniku.

6.  Klõpsake tööriistaribal nuppu **sätted**ja valige **integratsioon ja rakendused**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  Valige jaotises **Ühekordse sisselogimise** **SAML**.

8.  Klõpsake lehel **SAML** dialoogiboksi tehke järgmist:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Bonus.ly** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ning kleepige need **IdP SSO sihtkoht URL-i** tekstiväli.
    2.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Bonus.ly** dialoogiboksi **Väljaandja ID** väärtus kopeerida ja seejärel kleepige **IdP väljaandja** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Bonus.ly** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **IdP sisselogimise URL-i** tekstiväli.
    4.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Cert sõrmejälje** tekstiväli.

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

9.  Klõpsake nuppu **Salvesta**.

10. Microsoft Azure'i klassikaline portaalis valige konfiguratsiooni kinnituse ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate Bonus.ly sisse logida, nad peavad olema ettevalmistatud Bonus.ly.  
Puhul Bonus.ly, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Web brauseriaknas, logige sisse oma Bonus.ly rentniku.

2.  Valige **sätted**

    ![Sätted] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Sätted")

3.  Klõpsake vahekaarti **kasutajad ja lisatasud** .

    ![Kasutajate ja lisatasud] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Kasutajate ja lisatasud")

4.  Klõpsake **kasutajate haldamine**.

    ![Kasutajate haldamine] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Kasutajate haldamine")

5.  Klõpsake nuppu **Lisa kasutaja**.

    ![Kasutaja lisamine] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Kasutaja lisamine")

6.  Klõpsake dialoogiboksis **Lisa kasutaja,** tehke järgmist.

    ![Kasutaja lisamine] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Kasutaja lisamine")

    1.  Tippige "**e-posti**, **eesnimi**, **perekonnanimi**" kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Salvesta**.

    >[AZURE.NOTE] AAD konto omanik saavad meili, mis sisaldab linki konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud Bonus.ly kasutaja konto loomise tööriistade või API-de esitatud Bonus.ly AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Bonus.ly kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel Bonus.ly rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
