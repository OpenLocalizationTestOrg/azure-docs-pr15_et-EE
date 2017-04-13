<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine IdeaScale | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory IdeaScale abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Õpetus: Azure'i Active Directory integreerimine IdeaScale
  
Selle õpetuse eesmärk on integreerimine Azure ja IdeaScale kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   IdeaScale ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud IdeaScale ühekordse sisselogimise kasutamine [Accessi paani Sissejuhatus](active-directory-saas-access-panel-introduction.md)rakendusse.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks IdeaScale lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-ideascale"></a>Rakenduste integreerimise jaoks IdeaScale lubamine
  
Selle jaotise eesmärk on liigendamine IdeaScale jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Rakenduste integreerimise jaoks IdeaScale lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **IdeaScale**.

    ![Rakenduse Galerii] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **IdeaScale**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks IdeaScale oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks IdeaScale konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **IdeaScale** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate IdeaScale logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **IdeaScale Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida IdeaScale rakenduse (nt: "*https://company.IdeaScale.com*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise IdeaScale veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil IdeaScale ettevõtte administraatorina.

6.  Minge **Kogukonnafoorumi sätted**.

    ![Kogukonnafoorumi sätted] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Kogukonnafoorumi sätted")

7.  Minge **Turvalisus \> ühe logima sätted**.

    ![Ühe logima sätted] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Ühe logima sätted")

8.  **Ühe-logima tüüp**, valige **SAML 2.0**.

    ![Ühe logima tüüp] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Ühe logima tüüp")

9.  Klõpsake dialoogiboksis **Ühe logima sätted** tehke järgmist.

    ![Ühe logima sätted] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Ühe logima sätted")

    1.  Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise veebisaidil IdeaScale** lehel **Üksuse ID** väärtus kopeerida ja seejärel kleepige **SAML IdP üksuse ID** tekstiväli.
    2.  Metaandmete allalaaditud faili sisu ja seejärel kleepige **SAML IdP metaandmete** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil IdeaScale** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **URL-i Logi välja edu** tekstiväli.
    4.  Klõpsake nuppu **Salvesta muudatused**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate IdeaScale sisse logida, nad peavad olema ettevalmistatud IdeaScale.  
Puhul IdeaScale, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **IdeaScale** ettevõtte administraatorina.

2.  Minge **Kogukonnafoorumi sätted**.

    ![Kogukonnafoorumi sätted] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Kogukonnafoorumi sätted")

3.  Minge **põhisätted \> liikmete haldamine**.

4.  Klõpsake nuppu **Lisa liige**.

    ![Liikmete haldamine] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Liikmete haldamine")

5.  Uue liikme lisamiseks jaotises tehke järgmist.

    ![Uue liikme lisamiseks] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Uue liikme lisamiseks")

    1.  **E-posti aadressid** väljale Tippige meiliaadress, kehtiv AAD konto, mida soovite ette valmistada.
    2.  Klõpsake nuppu **Salvesta muudatused**.

    >[AZURE.NOTE] Azure Active Directory konto omanik saab e-posti konto kinnitada, kuni see muutub aktiivne link.

>[AZURE.NOTE] Saate kasutada mis tahes muud IdeaScale kasutaja konto loomise tööriistade või API-de esitatud IdeaScale AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>IdeaScale kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **IdeaScale **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).