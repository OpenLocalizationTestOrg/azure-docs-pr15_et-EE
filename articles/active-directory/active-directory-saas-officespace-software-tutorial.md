<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine OfficeSpace tarkvara | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada OfficeSpace tarkvara Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Õpetus: Azure'i Active Directory integreerimine OfficeSpace tarkvara
  
Selle õpetuse eesmärk on integreerimine Azure ja OfficeSpace tarkvara kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   OfficeSpace tarkvara ühekordse sisselogimise lubatud tellimuse
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud OfficeSpace tarkvara ühekordse sisselogimise rakendusse OfficeSpace tarkvara ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise OfficeSpace tarkvara lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Rakenduste integreerimise OfficeSpace tarkvara lubamine
  
Selle jaotise eesmärk on liigendamine OfficeSpace tarkvara rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Rakenduste integreerimise OfficeSpace tarkvara lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **OfficeSpace tarkvara**.

    ![Rakenduse Galerii] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **OfficeSpace tarkvara**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![OfficeSpace tarkvara] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace tarkvara")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks OfficeSpace tarkvara oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise OfficeSpace tarkvara konfigureerimine nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **OfficeSpace tarkvara** rakenduste integreerimine Azure klassikaline portaalis.

    ![Konfigureerimine ühekordse sisselogimise kohta =] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Konfigureerimine ühekordse sisselogimise kohta =")

2.  Lehel **Kuidas soovite kasutajad logida OfficeSpace tarkvara** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **OfficeSpace tarkvara Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida rakenduse OfficeSpace tarkvara (nt: "*https://company.officespacesoftware.com*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise OfficeSpace tarkvara** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidil OfficeSpace tarkvara ettevõtte administraatorina.

6.  Minge **administraator \> konnektorid**.

    ![Administraator] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Administraator")

7.  Klõpsake **SAML autoriseerimine**.

    ![Konnektorid] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Konnektorid")

8.  **SAML autoriseerimine** jaotises tehke järgmist.

    ![SAML konfigureerimine] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML konfigureerimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise OfficeSpace tarkvara** dialoogi **Remote sisselogimise URL-i** väärtus kopeerida ja seejärel kleepige **URL-i Logi välja pakkuja** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise OfficeSpace tarkvara** dialoogi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige see **Kliendi idp sihtkoht URL-i** tekstiväli.
    3.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle **Kliendi idp cert sõrmejälje** tekstiväli.  

        >[AZURE.TIP]
        Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    4.  Klõpsake nuppu **Salvesta sätted**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate logige OfficeSpace tarkvara, ta peab olema ette valmistatud OfficeSpace tarkvara. OfficeSpace tarkvara ettevalmistamise on automatiseeritud ülesanne.  
On teie jaoks toimingu üksusi pole.  
Kasutajate luuakse automaatselt vajadusel ühekordse sisselogimise katsel ajal.

>[AZURE.NOTE]Saate kasutada mis tahes muud OfficeSpace tarkvara kasutaja konto loomise tööriistu või API-de esitatud OfficeSpace tarkvara kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>OfficeSpace tarkvara kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **OfficeSpace tarkvara **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).