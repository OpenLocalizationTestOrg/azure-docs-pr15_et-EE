<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Jobscience | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Jobscience abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Õpetus: Azure'i Active Directory integreerimine Jobscience
  
Selle õpetuse eesmärk on integreerimine Azure ja Jobscience kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Tellimuse Jobscience ühekordse sisselogimise lubatud
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Jobscience ühekordse sisselogimise rakendusse Jobscience ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Jobscience lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-jobscience"></a>Rakenduste integreerimise jaoks Jobscience lubamine
  
Selle jaotise eesmärk on liigendamine Jobscience jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Jobscience lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **jobscience**.

    ![Rakenduse Galerii] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Jobscience**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Jobscience oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Ühekordse sisselogimise jaoks Jobscience konfigureerimise nõuab teilt sõrmejälje väärtuse toomiseks sert.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas tuua serdi sõrmejälje väärtus](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma Jobscience ettevõtte saidi administraator.

2.  Minge **häälestamise**.

    ![Häälestamine] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Häälestamine")

3.  Klõpsake vasakpoolsel navigeerimispaanil jaotises **Administreerimine** käsku **Domeenihaldus** seotud jaotise laiendamiseks ja klõpsake **Minu domeeni** **Minu domeeni** lehe avamiseks. 

    ![Oma domeeni] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Oma domeeni")

4.  Veenduge, et teie domeen on õigesti häälestatud, veenduge, et see on "**Samm 4 juurutatud kasutajate**" ja vaadake üle oma "**Minu domeeni sätted**".

    ![Kasutaja juurutatud Doman] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Kasutaja juurutatud Doman")

5.  Erinevate web brauseriaknas, logige sisse oma Azure klassikaline portaali.

6.  Klõpsake lehel **Jobscience** rakenduse integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Ühekordse sisselogimise konfigureerimine")

7.  Lehel **Kuidas soovite kasutajate Jobscience logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Ühekordse sisselogimise konfigureerimine")

8.  Lehel **Rakenduse URL-i konfigureerimine** **Jobscience sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://company.my.salesforce.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Rakenduse URL-i konfigureerimine")

9.  Lehel **Konfigureeri ühekordse sisselogimise Jobscience veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Ühekordse sisselogimise konfigureerimine")

10. Jobscience ettevõtte saidil, klõpsake **Turbemeetmed**, ja klõpsake **Ühekordse sisselogimise sätteid**.

    ![Turbemeetmed] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Turbemeetmed")

11. **Ühekordse sisselogimise sätted** jaotises tehke järgmist.

    ![Ühekordse sisselogimise sätted] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Ühekordse sisselogimise sätted")

    1.  Valige **SAML lubatud**.
    2.  Klõpsake nuppu **Uus**.

12. Klõpsake dialoogiboksis **SAML ühekordse sisselogimise säte redigeerimiseks** tehke järgmist:

    ![SAML ühekordse sisselogimise säte] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML ühekordse sisselogimise säte")

    1.  Tippige **nimi** rühmitusaluse nimi oma konfiguratsioon.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Jobscience** dialoogi **Väljaandja URL-i** väärtus kopeerida, ja seejärel kleepige **väljaandja** tekstiväli
    3.  Tippige tekstiväljale **Üksuse Id** **https://salesforce-jobscience.com**
    4.  Klõpsake nuppu **Sirvi** üles laadida oma Azure AD sert.
    5.  **SAML identiteedi tüüp**, valige **argument sisaldab kasutaja objekti Federation ID -d**.
    6.  **SAML identiteedi asukoht**, valige **teema lause NameIdentfier element on**.
    7.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Jobscience** dialoogi **Remote sisselogimise URL-i** väärtus kopeerida, ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli
    8.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Jobscience** dialoogi **Remote välju URL-i** väärtus kopeerida, ja seejärel kleepige **Identiteedi pakkuja välju URL-i** tekstiväli
    9.  Klõpsake nuppu **Salvesta**.

13. Klõpsake vasakpoolsel navigeerimispaanil jaotises **Administreerimine** käsku **Domeenihaldus** seotud jaotise laiendamiseks ja klõpsake **Minu domeeni** **Minu domeeni** lehe avamiseks. 

    ![Oma domeeni] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Oma domeeni")

14. Klõpsake lehel **Minu domeeni** **Login sümboolika leht** jaotises **redigeerimine**.

    ![Sisselogimislehe sümboolika] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Sisselogimislehe sümboolika")

15. Lehel **Sisselogimine lehe sümboolika** **Autentimisteenus** jaotises kuvatakse nime **SAML SSO sätted** . Valige see ja klõpsake siis nuppu **Salvesta**.

    ![Sisselogimislehe sümboolika] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Sisselogimislehe sümboolika")

16. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Ühekordse sisselogimise konfigureerimine")
  
SP algatatud ühekordse sisselogimise sisselogimise URL-i nuppu **ühekordse sisselogimise sätted** jaotises **Turbemeetmed** menüü.

![Turbemeetmed] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Turbemeetmed")
  
Klõpsake ülal kirjeldatud toimingus loodud SSO profiili.  
Sellel lehel kuvatakse ühekordse sisselogimise URL-i (nt *https://companyname.my.salesforce.com?so=companyid*) oma ettevõtte jaoks.
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Jobscience sisse logida, nad peavad olema ettevalmistatud Jobscience.  
Puhul Jobscience, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **Jobscience** ettevõtte administraatorina.

2.  Installimine

    ![Häälestamine] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Häälestamine")

3.  Minge **kasutajate haldamine \> kasutajate**.

    ![Kasutajatele] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Kasutajatele")

4.  Klõpsake nuppu **Uus kasutaja**.

    ![Kõik kasutajad] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Kõik kasutajad")

5.  Klõpsake dialoogiboksis **Kasutaja redigeerimiseks** tehke järgmist:

    ![Kasutaja redigeerimine] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Kasutaja redigeerimine")

    1.  Tippige eesnimi, perekonnanimi, pseudonüümi, e-posti, kasutaja nimi ja hüüdnimi soovite säte seotud tekstiväljad Azure AD kasutaja atribuudid.
    2.  Klõpsake nuppu **Salvesta**.

    >[AZURE.NOTE] Azure AD konto omanik saavad meili, mis sisaldab linki konto kinnitada, enne kui see on aktiveeritud.

>[AZURE.NOTE] Saate kasutada mis tahes muud Jobscience kasutaja konto loomise tööriistade või API-de esitatud Jobscience AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Jobscience kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Jobscience **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).