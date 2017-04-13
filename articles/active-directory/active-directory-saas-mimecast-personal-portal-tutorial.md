<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Mimecast isiklik portaali | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Mimecast isikliku portaali Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja rohkem!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Õpetus: Azure'i Active Directory integreerimine Mimecast isiklik portaal
  
Selle õpetuse eesmärk on integreerimine Azure ja Mimecast isikliku portaali kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Mimecast isikliku portaali ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, saab Azure AD kasutajate olete määranud Mimecast isikliku portaali ühekordse sisselogimise rakendusse Mimecast isikliku portaali ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise Mimecast isikliku portaali lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Stsenaarium")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>Rakenduste integreerimise Mimecast isikliku portaali lubamine
  
Selle jaotise eesmärk on liigendamine rakenduste integreerimise Mimecast isikliku portaali lubamise kohta.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Rakenduste integreerimise Mimecast isikliku portaali lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Mimecast isikliku portaalis**.

    ![Rakenduse Galerii] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Mimecast isikliku portaali**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Mimecast isiklik portaal] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast isiklik portaal")
##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine, kuidas kasutajad saavad oma konto abil SAML protokolli federation Azure AD Mimecast isikliku portaali autentimiseks.  
Selle toimingu käigus saate vajalike base-64-kodeeritud sertifikaat faili loomine.  
Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Mimecast isikliku portaali** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Ühekordse sisselogimise konfigureerimine")

2.  Lehel **Kuidas soovite kasutajate Mimecast isikliku portaali logida** , valige **Microsoft Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Ühekordse sisselogimise konfigureerimine")

3.  Tippige lehel **Rakenduse URL-i konfigureerimine** **Mimecast isikliku portaali logi sisse URL** väljale URL, mida kasutatakse teie kasutajad rakenduse Mimecast isikliku portaali logida (nt: "https://webmail-uk.mimecast.com" või "https://webmail-us.mimecast.com"), ja seejärel klõpsake nuppu **edasi**.

    >[AZURE.NOTE] Sisselogimise URL-i on teatud piirkond.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "Rakenduse URL-i konfigureerimine")

4.  Lehel **Konfigureeri ühekordse sisselogimise Mimecast isikliku portaalis** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Ühekordse sisselogimise konfigureerimine")

5.  Erinevate web brauseriaknas logige oma Mimecast isikliku portaali sisse administraatorina.

6.  Minge **teenuste \> rakenduse**.

    ![Rakenduste] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Rakenduste")

7.  Klõpsake nuppu **autentimise profiilid**.

    ![Autentimise profiilid] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Autentimise profiilid")

8.  Klõpsake nuppu **Uus autentimise profiil**.

    ![Uus autentimise Profil] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Uus autentimise Profil")

9.  Jaotises **Autentimine profiil** , tehke järgmist.

    ![Profil autentimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Autentimise Profil")

    1.  Tippige tekstiväljale **Kirjeldus** nimi oma konfiguratsioon.
    2.  Valige **Jõusta SAML autentimise Mimecast isiklik portaalis**.
    3.  Valige **pakkuja**, **Azure Active Directory**.
    4.  Azure'i klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Mimecast isikliku portaalis** lehel **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **Väljaandja URL-i** tekstiväli.
    5.  Azure'i klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Mimecast isikliku portaalis** lehel kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige see **Sisselogimise URL-i** tekstiväli.
    6.  Azure'i klassikaline portaalis, klõpsake dialoogiboksi **konfigureerimine ühekordse sisselogimise Mimecast isikliku portaalis** lehel **Remote sisselogimise URL-i** väärtus kopeerida ja seejärel kleepige **URL välju** tekstiväli.  

        >[AZURE.NOTE] Sisselogimise URL-i väärtus ja välju URL-i väärtus on-kohta Mimecast isikliku portaalis sama.

    7.  Teie allalaaditud serdi **base-64-kodeeritud** faili loomine.  

        >[AZURE.TIP]Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

    8.  Avatud base-64-kodeeritud sertifikaat Notepadis eemaldamine esimene rida ("*--*") ja viimane rida ("*--*"), kopeerige see ülejäänud sisu teie lõikelauale ja kleepige see siis tekstivälja **Identiteedi pakkuja sert (metaandmed)** .
    9.  Valige **Luba ühekordse sisselogimise kohta**.
    10. Klõpsake nuppu **Salvesta**.

10. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Ühekordse sisselogimise konfigureerimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selleks, et Azure AD kasutajate Mimecast isikliku portaali sisse logida, ta peab olema ette valmistatud Mimecast isikliku portaali sisse.  
Puhul Mimecast isikliku portaalis ettevalmistamise on käsitsi ülesanne.
  
Tuleb teil registreerida domeeni, enne kui saate luua kasutajaid.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige oma **Mimecast isikliku portaali** administraatorina.

2.  Minge **kataloogide \> sisemise**.

    ![Kataloogide] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Kataloogide")

3.  Klõpsake nuppu **Registreeri uus domeen**.

    ![Uue domeeni registreerimine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Uue domeeni registreerimine")

4.  Pärast uue domeeni on loodud, klõpsake nuppu **Uus aadress**.

    ![Uus aadress] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Uus aadress")

5.  Uus aadress dialoogiboksis tehke järgmist.

    ![Salvestamine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Salvestamine")

    1.  Tippige **Meiliaadress**, **Globaalne nimi**, **parool** ja **Parooli kinnitus** atribuutide kehtiv AAD konto, mida soovite seotud tekstiväljad säte.
    2.  Klõpsake nuppu **Salvesta**.

>[AZURE.NOTE]Saate kasutada mis tahes Mimecast isikliku portaali kasutaja konto loomise tööriistade või API-de kasutajakontode AAD Mimecast isikliku portaalis esitatud.

##<a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Mimecast isikliku portaali kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Mimecast isikliku portaali **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Jah")
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).