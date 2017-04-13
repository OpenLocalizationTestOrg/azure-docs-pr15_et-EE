<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine hõbepaber turvalisus | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada hõbepaber turvalisus Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Õpetus: Azure'i Active Directory integreerimine hõbepaber turvalisus
  
Selle õpetuse eesmärk on kuvamiseks integreerimine Azure ja hõbepaber Turve.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Hõbepaber turvalisus ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud hõbepaber turvalisus ühekordse sisselogimise rakendusse hõbepaber turvalisus ettevõtte saidi (identiteedi pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise hõbepaber väärtpaberi lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Ühekordse sisselogimise konfigureerimine")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Rakenduste integreerimise hõbepaber väärtpaberi lubamine
  
Selle jaotise eesmärk on liigendamine hõbepaber turvalisuse rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Rakenduste integreerimise hõbepaber turvalisuse lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Hõbepaber turvalisus**.

    ![Rakenduse Galerii] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Hõbepaber turvalisus**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Hõbepaber turvalisus] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Hõbepaber turvalisus")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks hõbepaber turvalisus oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise hõbepaber turvalisuse konfigureerimine nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Hõbepaber turvalisus** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajad logida hõbepaber turvalisus** valige **Microsoft Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Hõbepaber turvalisus vasta URL** väljale Tippige oma hõbepaber turvalisus kinnituse tarbija teenus (ACS) URL (nt: "*https://www.tinfoilsecurity.com/saml/consume*" ja klõpsake seejärel nuppu **edasi**.

    >[AZURE.NOTE] Mida peaks oskama ACS URL-i toomine hõbepaber turvalisus metaandmete (https://www.tinfoilsecurity.com/saml/metadata).

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise hõbepaber turvalisuse** teie serdi allalaadimiseks klõpsake nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\hõbepaber Security.cer**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil hõbepaber turvalisus ettevõtte administraatorina.

6.  Klõpsake tööriistaribal nuppu **Minu konto**.

    ![Armatuurlaua] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Armatuurlaua")

7.  Klõpsake nuppu **Turvalisus**.

    ![Turvalisus] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Turvalisus")

8.  **Ühekordse sisselogimise** konfiguratsiooni lehele, tehke järgmist.

    ![Ühekordne sisselogimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Ühekordne sisselogimine")

    1.  Valige **Luba SAML**.
    2.  Klõpsake nuppu **käsitsi konfigureerimine**.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise hõbepaber turvalisuse** dialoog, kopeerige **SAML SSO URL-i** väärtus ja seejärel kleepige **URL-i SAML postituse** tekstiväli.
    4.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **SAML serdi sõrmejälje** tekstiväli.  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    5.  Kopeerige **oma konto ID-d**.
    6.  Klõpsake nuppu **Salvesta**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Ühekordse sisselogimise konfigureerimine")

10. Klõpsake menüü peal, avage **SAML Turbeloa atribuutide** dialoogiboks **Atribuudid** .

    ![Atribuudid] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Atribuudid")

11. Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    ![Atribuudid] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Atribuudid")

    1.  Klõpsake nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** **accountid**.
    3.  **Atribuudi väärtus** tekstiväli, kleepige kopeeritud eelmises jaotises konto ID väärtus.
    4.  Klõpsake nuppu **valmis**.

12. Klõpsake nuppu **Rakenda muudatused**.

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate logige hõbepaber turvalisus, ta peab olema ettevalmistatud hõbepaber turvalisus.  
Ettevalmistamise hõbepaber puhul, on käsitsi ülesanne.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>Kasutaja ette valmistatud saamiseks tehke järgmist.

1.  Kui kasutaja on osa Enterprise konto, peate ühendust klienditoega hõbepaber turvalisus saada loodud kasutajakonto.

2.  Kui kasutaja on hõbepaber turvalisus SaaS regulaarne kasutaja, siis kasutaja lisamiseks SNEle kasutaja saitide. See käivitab protsessi hõbepaber turvalisus uue kasutajakonto loomine määratud e-posti kutse saatmiseks.

>[AZURE.NOTE] Saate kasutada mis tahes muud hõbepaber turvalisus kasutaja konto loomise tööriistade või API-de esitatud AAD kasutajakontode hõbepaber turvalisus.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Hõbepaber turvalisus kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Hõbepaber turvalisus **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).