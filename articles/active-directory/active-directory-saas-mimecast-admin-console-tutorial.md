<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Mimecast halduskonsoolis | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Mimecast halduskonsoolis kasutamine!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Õpetus: Azure'i Active Directory integreerimine Mimecast halduskonsoolis
  
Selle õpetuse eesmärk on integreerimine Azure ja Mimecast halduskonsoolis kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Mimecast halduskonsoolis ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Mimecast halduskonsoolis ühekordse sisselogimise rakendusse Mimecast halduskonsoolis ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Mimecast halduskonsoolis lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Rakenduste integreerimise jaoks Mimecast halduskonsoolis lubamine
  
Selle jaotise eesmärk on liigendamine Mimecast halduskonsoolis jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Mimecast halduskonsoolis lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Mimecast halduskonsoolis**.

    ![Rakenduse Galerii] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Mimecast Admin Console**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Mimecast halduskonsoolis] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast halduskonsoolis")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Mimecast halduskonsoolis oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Mimecast halduskonsoolis** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Ühekordse sisselogimise konfigureerimine")

2.  **Kuidas soovite kasutajad logida Mimecast halduskonsoolis** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Ühekordse sisselogimise konfigureerimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Mimecast Admin Console Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Mimecast halduskonsoolis rakenduse (nt: "https://webmail-uk.mimecast.com" või "https://webmail-us.mimecast.com"), ja seejärel klõpsake nuppu **edasi**.

    >[AZURE.NOTE] Sisselogimise URL-i on teatud piirkond.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Mimecast halduskonsoolis veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas, logige sisse oma Mimecast halduskonsoolis administraatorina.

6.  Minge **teenuste \> rakenduse**.

    ![Teenused] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Teenused")

7.  Klõpsake nuppu **autentimise profiilid**.

    ![Autentimise profiilid] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Autentimise profiilid")

8.  Klõpsake nuppu **Uus autentimise profiil**.

    ![Uus autentimise profiilid] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Uus autentimise profiilid")

9.  Jaotises **Autentimine profiil** , tehke järgmist.

    ![Profiili autentimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Profiili autentimine")

    1.  Tippige tekstiväljale **Kirjeldus** nimi oma konfiguratsioon.
    2.  Valige **Jõusta SAML autentimise Mimecast halduskonsoolis**.
    3.  Valige **pakkuja**, **Azure Active Directory**.
    4.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Mimecast halduskonsoolis** dialoogiboksi kopeerige **Väljaandja URL-i** väärtus ja seejärel kleepige **URL väljaandja** tekstiväli.
    5.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Mimecast halduskonsoolis** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    6.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Mimecast halduskonsoolis** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **URL välju** tekstiväli.  

        >[AZURE.NOTE]Sisselogimise URL-i väärtus ja välju URL-i väärtus on Mimecast halduskonsoolis sama.

    7.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

    8.  Avatud base-64-kodeeritud sertifikaat Notepadis eemaldamine esimene rida ("*--*") ja viimane rida ("*--*"), kopeerige see ülejäänud sisu teie lõikelauale ja kleepige see siis tekstivälja **Identiteedi pakkuja sert (metaandmed)** .
    9.  Valige **Luba ühekordse sisselogimise kohta**.
    10. Klõpsake nuppu **Salvesta**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Mimecast halduskonsoolis sisse logida, ta peab olema ettevalmistatud Mimecast halduskonsoolis.  
Puhul Mimecast halduskonsoolis, ettevalmistamise on käsitsi ülesanne.
  
Tuleb teil registreerida domeeni, enne kui saate luua kasutajaid.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige oma **Mimecast halduskonsoolis** administraatorina.

2.  Minge **kataloogide \> sisemise**.

    ![Kataloogide] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Kataloogide")

3.  Klõpsake nuppu **Registreeri uus domeen**.

    ![Uue domeeni registreerimine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Uue domeeni registreerimine")

4.  Pärast uue domeeni on loodud, klõpsake nuppu **Uus aadress**.

    ![Uus aadress] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Uus aadress")

5.  Uus aadress dialoogiboksis tehke järgmist.

    ![Salvestamine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Salvestamine")

    1.  Tippige **Meiliaadress**, **Globaalne nimi**, **parool** ja **Parooli kinnitus** atribuutide kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE]Saate kasutada mis tahes Mimecast halduskonsoolis kasutaja konto loomise tööriistade või API-de esitatud Mimecast halduskonsoolis kasutajakontode AAD.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Mimecast halduskonsoolis kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Mimecast halduskonsoolis **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).