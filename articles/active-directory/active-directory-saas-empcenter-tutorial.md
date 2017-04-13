<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine EmpCenter | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory EmpCenter abil!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Õpetus: Azure'i Active Directory integreerimine EmpCenter
  
Selle õpetuse eesmärk on integreerimine Azure ja EmpCenter kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   EmpCenter ühekordse sisselogimise lubatud tellimuse
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud EmpCenter ühekordse sisselogimise rakendusse EmpCenter ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks EmpCenter lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-empcenter"></a>Rakenduste integreerimise jaoks EmpCenter lubamine
  
Selle jaotise eesmärk on liigendamine EmpCenter jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Rakenduste integreerimise jaoks EmpCenter lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **EmpCenter**.

    ![Rakenduse Galerii] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **EmpCenter**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks EmpCenter oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **EmpCenter** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate EmpCenter logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Ühekordse sisselogimise konfigureerimine")

3.  Klõpsake lehel **Rakenduse sätete konfigureerimine** tehke järgmist.

    ![Rakenduse sätete konfigureerimine] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Rakenduse sätete konfigureerimine")

    1.  Tippige tekstiväljale **Logi sisse URL-i** URL, mida kasutatakse kasutajatele sisselogimise EmpCenter rakenduse järgi (nt: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Klõpsake nuppu **Järgmine**

4.  Lehel **Konfigureeri ühekordse sisselogimise EmpCenter veebisaidil** oma metaandmete allalaadimiseks klõpsake **metaandmete alla laadida**, ja salvestage metaandmete faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Ühekordse sisselogimise konfigureerimine")

5.  Metaandmete allalaaditud faili EmpCenter tugimeeskonnale saata.

    >[AZURE.NOTE] EmpCenter tugimeeskonnale on tegelik SSO konfigureerimine.
Saate teatise, kui tellimuse jaoks on lubatud SSO-d.

6.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate EmpCenter sisse logida, nad peavad olema ettevalmistatud EmpCenter.  
Puhul EmpCenter, Kasutajakontod vaja luua oma EmpCenter tugimeeskonna poole.

>[AZURE.NOTE] Saate kasutada mis tahes muud EmpCenter kasutaja konto loomise tööriistade või API-de esitatud EmpCenter ettevalmistamine Azure Active Directory Kasutajakontod.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>EmpCenter kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **EmpCenter **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).