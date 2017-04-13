<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine lülitatud raamatuid | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada lülitatud raamatud Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Õpetus: Azure'i Active Directory integreerimine lülitatud raamatud
  
Selle õpetuse eesmärk on integreerimine Azure ja lülitatud kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Lülitatud ühekordse sisselogimise lubatud tellimuse
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud lülitatud ühekordse sisselogimise rakendusse lülitatud ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks lülitatud lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-overdrive"></a>Rakenduste integreerimise jaoks lülitatud lubamine
  
Selle jaotise eesmärk on liigendamine lülitatud jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Rakenduste integreerimise jaoks lülitatud lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **lülitatud**.

    ![Rakenduse Galerii] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **lülitatud**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Lülitatud] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "Lülitatud")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks lülitatud oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **lülitatud** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Ühekordse sisselogimise lubamiseks")

2.  Lehel **Kuidas kas soovite kasutajate logige lülitatud** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Lülitatud sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*http://mslibrarytest.libraryreserve.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise lülitatud juures** metaandmete faili alla laadida ja saatke see lülitatud toe poole.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Konfigureerimine Ühekordne sisselogimine")

    >[AZURE.NOTE]Lülitatud tugimeeskonna ühekordse sisselogimise jaoks konfigureerib ja saadab isikule teate, kui konfiguratsioon on lõpule viidud.

5.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
On teil konfigureerida kasutaja ettevalmistamine lülitatud toimingu üksusi pole.  
Kui mõni määratud kasutaja üritab logige sisse lülitatud, luuakse automaatselt konto lülitatud vajaduse korral.

>[AZURE.NOTE]Saate kasutada mis tahes muud lülitatud kasutaja konto loomise tööriistade või API-de osutavad lülitatud kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Kasutajate määramine lülitatud, tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **lülitatud **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).