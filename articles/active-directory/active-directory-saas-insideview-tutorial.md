<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine InsideView | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory InsideView abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Õpetus: Azure'i Active Directory integreerimine InsideView
  
Selle õpetuse eesmärk on integreerimine Azure ja InsideView kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   InsideView rentniku jaoks
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud InsideView ühekordse sisselogimise rakendusse InsideView ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks InsideView lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-insideview"></a>Rakenduste integreerimise jaoks InsideView lubamine
  
Selle jaotise eesmärk on liigendamine InsideView jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Rakenduste integreerimise jaoks InsideView lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **InsideView**.

    ![Rakenduse Galerii] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **InsideView**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks InsideView oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **InsideView** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Ühe logima konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida InsideView** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Ühe logima konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **InsideView vastus URL-i** tekstivälja, tippige InsideView SSO URL-i (nt: `https://my.insideview.com/iv/<STS Name>/login.iv`), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise InsideView veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Ühe logima konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil InsideView ettevõtte administraatorina.

6.  Tööriistaribal ülaosas **administraator**, **SingleSignOn sätted**, klõpsake nuppu ja klõpsake **SAML lisada**.

    ![SAML ühekordse sisselogimise sätted] (./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML ühekordse sisselogimise sätted")

7.  Klõpsake jaotises **Lisa uus SAML** tehke järgmist.

    ![Lisage uus SAML] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Lisage uus SAML")

    1.  Tippige **STS nimi** rühmitusaluse nimi oma konfiguratsioon.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil InsideView** dialoogiboksi kopeerige **Teenuse pakkuja (SP) algatatud lõpp-punkti** väärtus ja seejärel kleepige see **SamlP/oli-Fed Unsolicated lõpp-punkti** tekstiväli.
    3.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.
        
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avage oma base – 64 encoded serdi Notepadis, kopeerige see sisu teie lõikelauale ja kleepige **STS -i sert** tekstiväli
    5.  Tippige tekstiväljale **CRM-iga kasutaja Id vastendamise** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  Tippige tekstiväljale **CRM-i e-posti vastendamise** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Tippige tekstiväljale **Crm eesnimi vastendamise** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  Tippige tekstiväljale **Crm perekonnanimi vastendamise** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Klõpsake nuppu **Salvesta**.

8.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühe logima konfigureerimine] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Ühe logima konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate InsideView sisse logida, nad peavad olema ettevalmistatud InsideView.  
Puhul InsideView, ettevalmistamise on käsitsi ülesanne.
  
Kasutajate või loodud InsideView kontaktide saamiseks võtke ühendust oma kliendi edu halduri või e-posti saatmine**support@insideview.com**

>[AZURE.NOTE] Saate kasutada mis tahes muud InsideView kasutaja konto loomise tööriistade või API-de esitatud InsideView ettevalmistamise Azure AD Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>InsideView kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **InsideView **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).