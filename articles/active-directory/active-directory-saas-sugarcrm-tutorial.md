<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine integreerimine SugarCRM | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory SugarCRM kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-sugarcrm"></a>Õpetus: Azure'i Active Directory integreerimine SugarCRM integreerimine
  
Selle õpetuse eesmärk on integreerimine Azure ja suhkru CRM kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Suhkru CRM ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud suhkru CRM ühekordse sisselogimise rakendusse suhkru CRM ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise suhkru CRM-i lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-sugar-crm"></a>Rakenduste integreerimise suhkru CRM-i lubamine
  
Selle jaotise eesmärk on liigendamine suhkru CRM-i rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-sugar-crm-perform-the-following-steps"></a>Rakenduste integreerimise suhkru CRM-i lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Suhkru CRM**.

    ![Rakenduse Galerii] (./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Suhkru CRM**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Suhkru CRM-iga] (./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Suhkru CRM-iga")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks suhkru CRM oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma suhkru CRM rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Suhkru CRM** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate suhkru CRM logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Suhkru CRM-i Logi sisse URL** väljale tippige URL, mida kasutatakse kasutajatele sisselogimise suhkru CRM rakenduse järgi (nt: "*http://company.sugarondemand.com*" ja klõpsake seejärel nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise suhkru CRM-i veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas logige saidile suhkru CRM ettevõtte administraatorina.

6.  Valige **administraator**.

    ![Administraator] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Administraator")

7.  Klõpsake jaotises **haldus** **Parooli haldus**.

    ![Haldus] (./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Haldus")

8.  Valige **Luba SAML autentimine**.

    ![Haldus] (./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Haldus")

9.  **SAML autentimine** jaotises tehke järgmist.

    ![SAML autentimine] (./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML autentimine")

    1.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil suhkru CRM** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil suhkru CRM** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **SLO URL** tekstiväli.
    3.  Teie allalaaditud serdi **Base-64-kodeeritud** faili loomine.

        >[AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avage oma base-64-kodeeritud sertifikaat Notepadis, kopeerige see sisu teie lõikelauale ja kleepige kogu sert **x.509 vastav sert** tekstiväli.
    5.  Klõpsake nuppu **Salvesta**.

10. Azure klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise suhkru CRM-i veebisaidil** lehel Valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **valmis**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate suhkru CRM sisse logida, ta peab olema ette valmistatud suhkru CRM-iga.  
Suhkru CRM-i puhul ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige saidile **Suhkru CRM** ettevõtte administraatorina.

2.  Valige **administraator**.

    ![Administraator] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Administraator")

3.  Klõpsake jaotises **haldus** **Kasutajate haldamine**.

    ![Haldus] (./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Haldus")

4.  Minge **kasutajate \> luua uue kasutaja**.

    ![Uue kasutaja loomine] (./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Uue kasutaja loomine")

5.  Klõpsake menüü **Kasutajaprofiili** tehke järgmist.

    ![Uus kasutaja] (./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "Uus kasutaja")

    1.  Tippige seotud tekstiväljad kasutajanimi, viimase lubatud Azure Active Directory kasutaja nimi ja meiliaadress.

6.  **Oleku**, valige **aktiivne**.

7.  Klõpsake menüü parooli tehke järgmist:

    ![Uus kasutaja] (./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "Uus kasutaja")

    1.  Tippige parool seotud tekstiväli.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE] Saate kasutada mis tahes muud suhkru CRM-iga kasutaja konto loomise tööriistade või API-de osutavad suhkru CRM kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-sugar-crm-perform-the-following-steps"></a>Suhkru CRM kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Suhkru CRM** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).