<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine ArcGIS | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada ArcGIS Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamine ja muud!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Õpetus: Azure'i Active Directory integreerimine ArcGIS

Selle õpetuse eesmärk on integreerimine Azure ja ArcGIS kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   ArcGIS ühekordse sisselogimise lubatud tellimuse

Pärast selle õpetuse, saab Azure AD kasutajate olete määranud ArcGIS ühekordse sisselogimise rakendusse ArcGIS ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise rakendusest tulu lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-arcgis"></a>Rakenduste integreerimise rakendusest tulu lubamine

Selle jaotise eesmärk on liigendamine rakendusest tulu rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Rakenduste integreerimise rakendusest tulu lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **ArcGIS**.

    ![Applcation Galerii] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation Galerii")

7.  Tulemuste paanil valige **ArcGIS**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks ArcGIS oma konto abil SAML protokolli federation Azure AD lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **ArcGIS** rakenduste integreerimine Azure klassikaline portaalis.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida ArcGIS** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **ArcGIS sisselogimise URL** väljale tippige URL-i kasutada oma kasutajatele logida kasutades järgmist mustrit "*https://company.maps.arcgis.com*" ja klõpsake siis nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **konfigureerimine ühekordse sisselogimise ArcGIS veebisaidil** klõpsake **metaandmete allalaadimine**ja seejärel salvestage fail metaandmete kohapeal arvutisse.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse saidil ArcGIS ettevõtte administraatorina.

6.  Klõpsake **sätete redigeerimine**.

    ![Sätete redigeerimine] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Sätete redigeerimine")

7.  Klõpsake nuppu **Turvalisus**.

    ![Turvalisus] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Turvalisus")

8.  Klõpsake jaotises **Ettevõtte sisselogimise**, **Määrata identiteedipakkuja juures**.

    ![Ettevõtte logimine] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Ettevõtte logimine")

9.  **Määrata identiteedipakkuja** konfiguratsiooni lehele, tehke järgmist.

    ![Määrake identiteedipakkuja] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Määrake identiteedipakkuja")

    1.  Tippige väljale nimi oma ettevõtte nime.
    2.  **Metaandmete suurettevõtetele identiteedipakkuja edastamisel kasutatakse**, valige **Fail**.
    3.  Metaandmete allalaaditud faili üleslaadimiseks klõpsake **faili valimine**.
    4.  Klõpsake nuppu **Määra identiteedipakkuja**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selleks, et Azure AD kasutajate ArcGIS sisse logida, nad peavad olema ettevalmistatud ArcGIS.  
Puhul ArcGIS, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma **ArcGIS** rentniku.

2.  Klõpsake nuppu **Kutsu liikmed**.

    ![Liikmete kutsumine] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Liikmete kutsumine")

3.  Valige **Lisa liikmeid automaatselt ilma meilisõnumi saatmine**ja seejärel klõpsake nuppu **edasi**.

    ![Liikmete lisamine automaatselt] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Liikmete lisamine automaatselt")

4.  Klõpsake lehel **liikmete** dialoogiboksis tehke järgmist.

    ![Lisamine ja läbivaatus] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Lisamine ja läbivaatus")

    1.  Sisestage **eesnimi**, **Perekonnanimi** ja **e-posti** kehtiv AAD konto, mida soovite näha.
    2.  Klõpsake nuppu **Lisa ja vaadake üle**.

5.  Vaadake sisestatud andmed ja seejärel klõpsake käsku **Lisa liikmeid**.

    ![Liikme lisamine] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Liikme lisamine")

>[AZURE.NOTE] Saate kasutada mis tahes muud ArcGIS kasutaja konto loomise tööriistade või API-de osutavad ArcGIS kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>ArcGIS kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **ArcGIS **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Jah")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).
