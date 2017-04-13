<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Freshdesk | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Freshdesk abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Õpetus: Azure'i Active Directory integreerimine Freshdesk
  
Selle õpetuse eesmärk on integreerimine Azure ja Freshdesk kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Freshdesk rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Freshdesk ühekordse sisselogimise rakendusse Freshdesk ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Freshdesk lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Rakenduste integreerimise jaoks Freshdesk lubamine
  
Selle jaotise eesmärk on liigendamine Freshdesk jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Freshdesk lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Freshdesk**.

    ![Rakenduse Galerii] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Freshdesk**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Freshdesk oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Freshdesk konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Freshdesk** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Freshdesk logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Freshdesk sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. Freshdesk.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Freshdesk veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\Freshdesk.cer**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil Freshdesk ettevõtte administraatorina.

6.  Klõpsake menüüs ülaosas linki **administraator**.

    ![Administraator] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Administraator")

7.  Klõpsake jaotises **Üldsätted** **Turvalisus**.

    ![Turvalisus] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Turvalisus")

8.  Jaotises **Turvalisus** , tehke järgmist.

    ![Ühekordse sisselogimise] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Ühekordse sisselogimise")

    1.  ** **Ühekordse sisselogimise kohta (SSO)**, valige.**
    2.  Valige **SAML SSO-d**.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Freshdesk** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **SAML sisselogimise URL-i** tekstiväli.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Freshdesk** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.
    5.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Turvalisus serdi sõrmejälje** tekstiväli.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    6.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Freshdesk sisse logida, nad peavad olema ettevalmistatud Freshdesk.  
Puhul Freshdesk, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Freshdesk** rentniku.

2.  Klõpsake menüüs ülaosas linki **administraator**.

    ![Administraator] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Administraator")

3.  Klõpsake jaotises **Üldsätted** **agentide**.

    ![Agentide] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agentide")

4.  Klõpsake nuppu **Uus Agent**.

    ![Uus Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Uus Agent")

5.  Klõpsake dialoogiboksis agendi teave tehke järgmist.

    ![Agendi teave] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agendi teave")

    1.  Tippige väljale **Täisnimi** nimi Azure AD konto, mida soovite ette valmistada.
    2.  Tippige tekstiväljale **e-posti** Azure AD meiliaadress Azure AD konto, mida soovite ette valmistada.
    3.  Tippige väljale **tiitel** nimi Azure AD konto, mida soovite ette valmistada.
    4.  Valige **agentide rolli**ja seejärel klõpsake nuppu **Määra**.
    5.  Klõpsake nuppu **Salvesta**.
    
        >[AZURE.NOTE] Azure AD konto omanik saavad meili, mis sisaldab linki konto kinnitada, enne kui see on aktiveeritud.

>[AZURE.NOTE] Saate kasutada mis tahes muud Freshdesk kasutaja konto loomise tööriistade või API-de esitatud Freshdesk AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Freshdesk kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Freshdesk **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).