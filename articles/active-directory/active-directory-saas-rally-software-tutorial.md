<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ralli tarkvara | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada tarkvara Rally Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Õpetus: Azure'i Active Directory integreerimine ralli tarkvara
  
Selle õpetuse eesmärk on integreerimine Azure ja tarkvara Rally kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Tarkvara Rally rentniku jaoks
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise Rally tarkvara lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-rally-software"></a>Rakenduste integreerimise Rally tarkvara lubamine
  
Selle jaotise eesmärk on liigendamine Rally tarkvara rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Rakenduste integreerimise Rally tarkvara lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ralli**.

    ![Rakenduse Galerii] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Rally tarkvara**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Rally tarkvara] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally tarkvara")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Rally tarkvara oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus on vaja serdi üleslaadimine Rally tarkvara.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Lehe **Tarkvara Rally **rakenduse integreerimine Azure klassikaline portaalis nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  **Kuidas kas soovite kasutajad logida ralli** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ning seejärel klõpsake nuppu **edasi**.

    ![Microsoft Azure'i AD ühekordse sisselogimise] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure'i AD ühekordse sisselogimise")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Rally tarkvara rentniku URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. rally.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Rakenduse URL-i konfigureerimine")

4.  Klõpsake lehel **Konfigureeri ühekordse sisselogimise Rally** nuppu Laadi alla metaandmete ja oma arvutisse salvestada.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Konfigureerimine Ühekordne sisselogimine")

5.  Logige sisse oma **Tarkvara ralli** rentniku.

6.  Klõpsake tööriistaribal nuppu **häälestus**ja valige **tellimuse**.

    ![Tellimuse] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Tellimuse")

7.  Tööriistaribal klõpsake parempoolses ülanurgas nuppu **toiming** ja seejärel valige **Redigeeri tellimus**.

8.  Klõpsake lehel **tellimuse** dialoogiboksis järgmiste toimingute ja seejärel klõpsake nuppu **Salvesta ja Sule**.

    ![Autentimine] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Autentimine")

    1.  Valige **ralli või SSO autentimise** rippmenüüst autentimine
    2.  Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Rally tarkvara** lehel **Identiteedi pakkuja ID** väärtus kopeerida, ja seejärel kleepige **URL-i identiteedi pakkuja** tekstiväli
    3.  Kopeerige Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise Rally tarkvara** dialoogiboksi **Remote välju URL-i** väärtus.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
AAD kasutajad saaksid sisse logida, ta peab olema ette valmistatud Rally tarkvara rakendusse kasutajanime Azure Active Directory abil.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma tarkvara ralli rentniku.

2.  Minge **häälestamise \> kasutajate**, ja seejärel klõpsake nuppu **+ Lisa uus**.

    ![Kasutajatele] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Kasutajatele")

3.  Tippige nimi väljale uus kasutaja ja seejärel klõpsake nuppu **Lisa üksikasjadega**.

4.  Jaotises **Kasutaja loomiseks** tehke järgmist.

    ![Kasutaja loomine] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Kasutaja loomine")

    1.  Tippige väljale **Kasutajanimi** Azure AD kasutaja, mida soovite ette valmistada.
    2.  Tippige väljale **Meiliaadress** meiliaadress Azure AD kasutaja soovite ette valmistada.
    3.  Klõpsake nuppu **Salvesta ja Sule**.

>[AZURE.NOTE]Saate kasutada mis tahes muud Rally tarkvara kasutaja konto loomise tööriistade või API-de osutavad Rally tarkvara kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Rally tarkvara kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Rally tarkvara** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).




