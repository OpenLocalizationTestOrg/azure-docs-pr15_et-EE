<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Onit | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Onit abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Õpetus: Azure'i Active Directory integreerimine Onit
  
Selle õpetuse eesmärk on integreerimine Azure ja Onit kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Onit ühekordse sisselogimise lubatud tellimuse
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Onit ühekordse sisselogimise rakendusse Onit ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Onit lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-onit-tutorial/IC791166.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-onit"></a>Rakenduste integreerimise jaoks Onit lubamine
  
Selle jaotise eesmärk on liigendamine Onit jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Onit lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-onit-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-onit-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-onit-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Onit**.

    ![Rakenduse Galerii] (./media/active-directory-saas-onit-tutorial/IC791167.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Onit**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Onit oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Onit konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).
  
Rakenduse Onit loodab SAML kinnitused kindlas vormingus, mis nõuab teilt kohandatud atribuutide vastenduste lisamine oma **saml Turbeloa atribuutide** konfigureerimine.  
Järgmine pilt kuvatakse järgmine näide.

![Ühekordne sisselogimine] (./media/active-directory-saas-onit-tutorial/IC791168.png "Ühekordne sisselogimine")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Onit** rakenduse integreerimise menüü ülaosas nuppu **Atribuudid** **SAML Turbeloa atribuutide** dialoogiboksi avamiseks.

    ![Atribuudid] (./media/active-directory-saas-onit-tutorial/IC791169.png "Atribuudid")

2.  Nõutavate atribuutide vastendused lisamiseks tehke järgmist.

    
  	|Atribuudi nimi|Atribuudi väärtus|
  	|---|---|
  	|Nimi|User.userPrincipalName|
  	|e-posti|User.mail|

    1.  Ülaltoodud tabelis andmeid iga rea, nuppu **Lisa kasutaja atribuut**.
    2.  Tippige väljale **Atribuudi nimi** kuvatud selle rea atribuudi nimi.
    3.  Valige loendist **Atribuudi väärtus** atribuudi selle rea kuvatud väärtus.
    4.  Klõpsake nuppu **valmis**.

3.  Klõpsake nuppu **Rakenda muudatused**.

4.  Brauseri nuppu **tagasi** dialoogiboksi **Kiirkäivituse** uuesti avada.

5.  Klõpsake nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-onit-tutorial/IC791170.png "Ühekordse sisselogimise konfigureerimine")

6.  Lehel **Kuidas soovite kasutajate Onit logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-onit-tutorial/IC791171.png "Ühekordse sisselogimise konfigureerimine")

7.  Lehel **Rakenduse URL-i konfigureerimine** **Onit Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Onit rakenduse (nt: "*https://ms-sso-test.onit.com*"), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-onit-tutorial/IC791172.png "Rakenduse URL-i konfigureerimine")

8.  Lehel **Konfigureeri ühekordse sisselogimise Onit veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-onit-tutorial/IC791173.png "Ühekordse sisselogimise konfigureerimine")

9.  Erinevate web brauseriaknas, logige sisse saidil Onit ettevõtte administraatorina.

10. Klõpsake menüü peal, **haldus**.

    ![Haldus] (./media/active-directory-saas-onit-tutorial/IC791174.png "Haldus")

11. Klõpsake nuppu **Redigeeri Corporation**.

    ![Redigeeri Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Redigeeri Corporation")

12. Klõpsake vahekaarti **Turvalisus** .

    ![Redigeeri ettevõtte teave] (./media/active-directory-saas-onit-tutorial/IC791176.png "Redigeeri ettevõtte teave")

13. Klõpsake vahekaarti **Turvalisus** , tehke järgmist:

    ![Ühekordne sisselogimine] (./media/active-directory-saas-onit-tutorial/IC791177.png "Ühekordne sisselogimine")

    1.  Kui **Autentimine strateegia**, valige **ühekordse sisselogimise ja parooli**.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Onit** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **URL-i Idp Target** tekstiväli.
    3.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Onit** dialoogiboksi **Remote välju URL-i** väärtus kopeerida ja seejärel kleepige **URL-i Idp välju** tekstiväli.
    4.  Eksporditud serdi **sõrmejälje** väärtus kopeerida ja kleepida selle tekstivälja **Idp Cert sõrmejälje (SHA1)** .  

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI)

    5.  **SSO tüüp**, valige **SAML**.
    6.  Tippige tekstiväljale **SSO sisselogimise nupu tekst** nuppu teksti, mis teile meeldib.
    7.  Valige **sisselogimise SSO: domeeni ja kasutajate jaoks nõutav**, Tippige meiliaadress, testkasutaja seotud tekstiväli ja klõpsake nuppu **Värskenda**.
        ![Redigeeri Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Redigeeri Corporation")

14. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-onit-tutorial/IC791179.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Onit sisse logida, nad peab olema ettevalmistatud Onit.  
Puhul Onit, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **Onit** ettevõtte administraatorina.

2.  Klõpsake nuppu **Lisa kasutaja**.

    ![Haldus] (./media/active-directory-saas-onit-tutorial/IC791180.png "Haldus")

3.  Klõpsake lehel **Lisa kasutaja** dialoogiboksi tehke järgmist:

    ![Kasutaja lisamine] (./media/active-directory-saas-onit-tutorial/IC791181.png "Kasutaja lisamine")

    1.  Tippige **nimi** ja **Meiliaadress** kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Loo**.  

        >[AZURE.NOTE] Konto omanik saab e-posti, sh link konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud Onit kasutaja konto loomise tööriistade või API-de esitatud Onit AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Onit kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Onit **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-onit-tutorial/IC791182.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-onit-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).