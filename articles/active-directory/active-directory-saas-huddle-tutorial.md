<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine kobarasse | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud Azure Active Directory kobarasse kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Õpetus: Azure'i Active Directory integreerimine kobarasse
  
Selle õpetuse eesmärk on integreerimine Azure ja kobarasse kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Kobarasse ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud kobarasse ühekordse sisselogimise rakendusse kobarasse ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks kobarasse lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Ühekordse sisselogimise konfigureerimine")
##<a name="enabling-the-application-integration-for-huddle"></a>Rakenduste integreerimise jaoks kobarasse lubamine
  
Selle jaotise eesmärk on liigendamine kobarasse jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Rakenduste integreerimise jaoks kobarasse lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **kobarasse**.

    ![Rakenduse Galerii] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **kobarasse**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Kobarasse] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Kobarasse")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks kobarasse oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **kobarasse** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas kas soovite kasutajad logida kobarasse** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Kobarasse Logi sisse URL** väljale Tippige oma kobarasse rentniku, kasutades järgmist mustrit "*http://company.huddle.com*" URL ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise kobarasse veebisaidil** , tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Ühekordse sisselogimise konfigureerimine")

    1.  Klõpsake nuppu **serdi allalaadimine**ja seejärel salvestage serdi faili oma arvutist.
    2.  Kopeerige **Väljaandja URL-i** väärtus, **SAML SSO URL-i** väärtus ja allalaaditud serdi ja saatke neile kobarasse toe poole.

    >[AZURE.NOTE] Ühekordse sisselogimise peab olema lubatud kobarasse tugimeeskonnalt.
Saate teatise, kui konfiguratsioon on lõpule viidud.

5.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate kobarasse sisse logida, ta peab olema ettevalmistatud kobarasse.  
Puhul kobarasse, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma ettevõtte **kobarasse** saidile administraatorina.

2.  Klõpsake käsku **Tööruum**.

3.  Klõpsake **inimesed \> Kutsu inimesi**.

    ![Inimesed] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Inimesed")

4.  Jaotise **Loo uus kutse** , tehke järgmist.

    ![Uue kutse] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Uue kutse")

    1.  Valige loendis **Valige Kutsu inimesi liituma meeskonna** **meeskonnatöö**.
    2.  Tippige **Meiliaadress** kehtiv AAD konto, mida soovite säte seotud tekstiväli.
    3.  Klõpsake nuppu **Kutsu**.

    >[AZURE.NOTE] Azure AD konto omanik saab e-posti, sh link konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud kobarasse kasutaja konto loomise tööriistade või API-de esitatud kobarasse ettevalmistamine AAD Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Kobarasse kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  **Kobarasse **rakenduse integreerimise, klõpsake lehel **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).