<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine 15Five | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory 15Five abil!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Õpetus: Azure'i Active Directory integreerimine 15Five

Selle õpetuse eesmärk on integreerimine Azure ja 15Five kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   15Five ühekordse sisselogimise lubatud tellimus

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud 15Five ühekordse sisselogimise rakendusse 15Five ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks 15Five lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-15five-tutorial/IC784667.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-15five"></a>Rakenduste integreerimise jaoks 15Five lubamine

Selle jaotise eesmärk on liigendamine 15Five jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Rakenduste integreerimise jaoks 15Five lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-15five-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-15five-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-15five-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **15Five**.

    ![Rakenduse Galerii] (./media/active-directory-saas-15five-tutorial/IC784668.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **15Five**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks 15Five oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **15Five** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-15five-tutorial/IC784670.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate 15Five logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-15five-tutorial/IC784671.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **15Five sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://company.15Five.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-15five-tutorial/IC784672.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise 15Five veebisaidil** klõpsake **metaandmete allalaadimine**ja seejärel edasi metaandmete tugimeeskonnale 15Five faili.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-15five-tutorial/IC784673.png "Konfigureerimine Ühekordne sisselogimine")

    >[AZURE.NOTE] Ühekordse sisselogimise peab olema lubatud 15Five tugimeeskonnalt.

5.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-15five-tutorial/IC784674.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate 15Five sisse logida, nad peab olema ettevalmistatud 15Five.  
Puhul 15Five, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile **15Five** ettevõtte administraatorina.

2.  Minge **ettevõtte haldamine**.

    ![Ettevõtte haldamine] (./media/active-directory-saas-15five-tutorial/IC784675.png "Ettevõtte haldamine")

3.  Minge **inimesed \> , milles osutatakse**.

    ![Inimesed] (./media/active-directory-saas-15five-tutorial/IC784676.png "Inimesed")

4.  Jaotises uue isiku lisamiseks tehke järgmist:

    ![Uue isiku lisamine] (./media/active-directory-saas-15five-tutorial/IC784677.png "Uue isiku lisamine")

    1.  Tippige **eesnimi**, **Perekonnanimi**, **pealkirja**, soovite seotud tekstiväljad säte lubatud Azure Active Directory konto **meiliaadress** .
    2.  Klõpsake nuppu **valmis**.

    >[AZURE.NOTE] Azure AD konto omanik saab e-posti, sh link konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud 15Five kasutaja konto loomise tööriistade või API-de esitatud 15Five AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>15Five kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **15Five **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-15five-tutorial/IC784678.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-15five-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
