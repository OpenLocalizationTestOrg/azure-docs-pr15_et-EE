<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Citrix GoToMeeting | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Citrix GoToMeeting Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!." 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Õpetus: Azure'i Active Directory integreerimine Citrix GoToMeeting  
Kehtib: Azure'i

>[AZURE.TIP]Tagasiside, klõpsake [siin](http://go.microsoft.com/fwlink/?LinkId=522412).

Selle õpetuse eesmärk on integreerimine Azure ja Citrix GoToMeeting kuvamiseks. Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Klõpsake Citrix GoToMeeting rentniku

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Citrix GoToMeeting lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Konfigureerimine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfigureerimine")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Rakenduste integreerimise jaoks Citrix GoToMeeting lubamine

Selle jaotise eesmärk on liigendamine Citrix GoToMeeting jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Citrix GoToMeeting lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Galeriist rakenduse lisamine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Galeriist rakenduse lisamine")

6.  Tippige **väljale Otsi** **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  Tulemuste paanil valige **Citrix GoToMeeting**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Citrix GoToMeeting oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus teil on vaja oma Citrix GoToMeeting rentniku base – 64 encoded serdi üleslaadimine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Klõpsake lehel **Citrix GoToMeeting** rakenduse integreerimise **konfigureerimine Ühekordne sisselogimine** **ÜHEKORDSE SISSELOGIMISE sees** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Ühekordse sisselogimise lubamiseks")

2.  Valige lehel **Kuidas soovite kasutajad logida Citrix GoToMeeting** **Microsoft Azure AD ühekordse sisselogimise**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Konfigureerimine Ühekordne sisselogimine")


3. **Rakenduse sätete konfigureerimine** lehel klõpsake nuppu **edasi**. 

    ![Ühekordse sisselogimise lubamiseks] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Ühekordse sisselogimise lubamiseks")

4.  Lehel **Konfigureeri ühekordse sisselogimise Citrix GoToMeeting juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi faili oma arvutist.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Konfigureerimine Ühekordne sisselogimine")

5.  Erinevate brauseriaknas, logige sisse Citrix ettevõtte Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Klõpsake vahekaarti **Identiteedipakkuja** ja seejärel tehke järgmist.  

    ![SAML häälestamine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML häälestamine")

    lisamine. Valige **käsitsi**

    
    b. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Citrix GoToMeeting** dialoogiboksi kopeerige **Sisselogimise lehe URL-i** väärtus ja seejärel kleepige see **sisselogimise lehe URL-i** tekstiväli. 

    
    c. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Citrix GoToMeeting** dialoogiboksi kopeerige **Sign-Out lehe URL-i** väärtus ja seejärel kleepige **väljunud lehe URL-i** tekstiväli.

    
    d. Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Citrix GoToMeeting** dialoogiboksi **Üksuse ID** väärtus kopeerida ja seejärel kleepige **identiteedi pakkuja üksuse ID** tekstiväli.

   
    e. Laadige oma allalaaditud serdi, klõpsake **Serdi üleslaadimine**.

    
    f. Klõpsake nuppu **Salvesta**.

6.  Azure'i klassikaline portaalis, valige ühekordse sisselogimise konfigureerimine kinnituse ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Konfigureerimine Ühekordne sisselogimine")


7. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.

    ![SAML häälestamine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML häälestamine")





##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine

Selle jaotise eesmärk on liigendamine ettevalmistamine Active Directory kasutajakontosid, et Citrix GoToMeeting lubamise kohta.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Klõpsake lehel **Citrix GoToMeeting** rakenduse integreerimine Azure klassikaline portaalis **konfigureerimine kasutaja ettevalmistamise** **Konfigureerimine kasutaja ettevalmistamise** dialoogiboksi avamiseks.

    ![Konfigureerimine kasutaja ettevalmistamine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Konfigureerimine kasutaja ettevalmistamine")

2.  Klõpsake lehel **sätted ja administraatori identimisteave** tehke järgmist.

    ![Konfigureerimine kasutaja ettevalmistamine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Konfigureerimine kasutaja ettevalmistamine")

    lisamine. Tippige tekstiväljale **Citrix GoToMeeting administraatori kasutajanime** administraatori kasutajanimi.

    
    b. Tekstiväljale **Citrix GoToMeeting administraatoriparooli** administraatori parooli.

    
    c. Klõpsake nuppu **edasi**.

3.  Klõpsake lehel **Confirmation** konfiguratsioonile salvestamiseks märke.

4.  Klõpsake nuppu **Valideeri** konfiguratsioonist kinnitamiseks.


##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Citrix GoToMeeting kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Citrix GoToMeeting** rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Jah")

Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Dropbox for Business.

Esimese sammuna kinnitamine, saate ettevalmistamise olek, klõpsates armatuurlaua d Azure klassikaline portaalis lehel **Citrix GoToMeeting** rakenduse integreerimine.

![Armatuurlaua] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Armatuurlaua")

Tsükkel ettevalmistamise edukalt lõpule viidud kasutaja on tähistatud seotud olek:

![Integreerimine olek] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integreerimine olek")

Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani.

Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](https://msdn.microsoft.com/library/dn308586).
