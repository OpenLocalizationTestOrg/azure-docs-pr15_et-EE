<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Jitbit kasutajatoe | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Jitbit kasutajatoe kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Õpetus: Azure'i Active Directory integreerimine Jitbit kasutajatugi
  
Selle õpetuse eesmärk on integreerimine Azure ja Jitbit kasutajatoe kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Rentniku Jitbit kasutajatugi
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Jitbit kasutajatoe ühekordse sisselogimise rakendusse Jitbit kasutajatoe ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Jitbit kasutajatoe lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Rakenduste integreerimise jaoks Jitbit kasutajatoe lubamine
  
Selle jaotise eesmärk on liigendamine Jitbit tugimeeskonna jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Jitbit kasutajatoe lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Jitbit klienditoe poole**.

    ![Rakenduse Galerii] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Jitbit kasutajatugi**, ja klõpsake **lõpuleviimine** rakenduse lisamiseks.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Jitbit kasutajatoe oma konto abil SAML protokolli federation Azure AD lubamise kohta. .  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Selleks et oma Jitbit kasutajatoe rentniku ühekordse sisselogimise konfigureerimiseks, peate pöörduma esmalt Jitbit kasutajatoe tehnilise toe saamiseks see funktsioon on sisse lülitatud.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Jitbit kasutajatoe** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Jitbit kasutajatoe logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Jitbit kasutajatoe sisselogimise URL** väljale Tippige oma URL-i abil järgmise mustriga "*https://\<rentniku nime\>. JitBit.com*", ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Jitbit kasutajatoe veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**, ja seejärel salvestage serdi fail kohalikult **c:\\Jitbit Helpdesk.cer**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate web brauseriaknas, logige sisse saidile Jitbit kasutajatoe ettevõtte administraatorina.

6.  Klõpsake tööriistaribal ülaosas **haldus**.

    ![Haldus] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Haldus")

7.  Klõpsake **üldsätted**.

    ![Kasutajad, ettevõtete ja õigused] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Kasutajad, ettevõtete ja õigused")

8.  **Autentimise sätete** konfigureerimine jaotises tehke järgmist.

    ![Autentimissätted] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Autentimissätted")

    1.  Valige **Luba SAML 2.0 ühe sisse logida** sisse logida kasutades **OneLogin**ühekordse sisselogimise (SSO).
    2.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Jitbit kasutajatoe** dialoogi kopeerige **teenuse pakkuja (SP) algatatud lõpp-punkti** väärtus ja seejärel kleepige see **Lõpp-punkti URL-i** tekstiväli.
    3.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.
        
        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avage oma base – 64 encoded sert, kopeerige see sisu teie lõikelauale ja kleepige **x.509 vastav sert** tekstiväli
    5.  Klõpsake nuppu **Salvesta muudatused**.

9.  Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Jitbit kasutajatoe sisse logida, ta peab olema ettevalmistatud Jitbit kasutajatoe.  
Puhul Jitbit kasutajatugi, ettevalmistamise on käsitsi ülesanne.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kasutajakontode, tehke järgmist.

1.  Logige sisse oma **Jitbit kasutajatoe** rentniku.

2.  Klõpsake menüü peal, **haldus**.

    ![Haldus] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Haldus")

3.  Valige **kasutajad, ettevõtete ja õigused**.

    ![Kasutajad, ettevõtete ja õigused] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Kasutajad, ettevõtete ja õigused")

4.  Klõpsake nuppu **Lisa kasutaja**.

    ![Kasutaja lisamine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Kasutaja lisamine")

5.  Tippige jaotises Loo Azure AD konto säte järgmised tekstiväljad soovite andmed: **kasutajanimi**, **e-posti**, **eesnimi**, **Perekonnanimi**

    ![Loomine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Loomine")

6.  Klõpsake nuppu **Loo**.

>[AZURE.NOTE] Saate kasutada mis tahes muud Jitbit kasutajatoe kasutaja konto loomise tööriistade või API-de esitatud Jitbit kasutajatoe kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Jitbit kasutajatoe kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Jitbit kasutajatoe **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).